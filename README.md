# WASM GKE EXAMPLE

## First steps for develop Wasm Apps with Rust
1. Install rustup
2. Install wasm32-wasi (Local testing)
```
rustup target add wasm32-wasi
```
3. Install WasmEdge (Local testing)
```
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash
```
4. Create an application
```
cargo new api
```
5. Change to api directory
6. Update the main.rs with your desired code
7. Add wasm32-wasi support
```
rustup target add wasm32-wasi
```
8. Add necessary libraries to create a API Rest with Rust
```
cargo add hyper_wasi --features full
cargo add tokio_wasi --features tokio_wasi/rt,macros,net,time,io-util
```
9. Build the binary
```
cargo build --target wasm32-wasi --release
```
10. Run the code
```
wasmedge target/wasm32-wasi/release/api.wasm
```

## Running the Wasm Kubernetes Example
1. Create a GKE Cluster
```
gcloud container clusters create k8s-demo --num-nodes=2 --tags=allin,allout --machine-type=n1-standard-2 --no-enable-network-policy
```
2. Clone the repository
3. Change to the src directory
```
cd api-rest/src
```
4. Build the container image
```
/bin/bash build.sh YOUR_DOCKER_USER
```
5. Install NGINX Ingress controller using Helm
```
kubectl create ns nginx-ingress
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx 
helm repo update 
helm install nginx-ingress ingress-nginx/ingress-nginx -n nginx-ingress
kubectl get services -n nginx-ingress (To get the Load Balancer IP Address)
```
6. Get the Load Balancer IP and change it in the api-rest.yaml ingress section
7. Apply the YAML configuration to run the example:
```
kubectl apply -f api-rest.yaml
```
8. If you want to test the API you can run something like this:
```
while true; do curl http://<LOAD_BALANCER_IP>.nip.io/echo -H "Content-Type: application/json" --data '{"key":"value"}'; sleep 0.5; done
```



# REFERENCES
- https://github.com/WasmEdge/wasmedge_reqwest_demo
- https://wasmedge.org/docs/develop/rust/http_service/client