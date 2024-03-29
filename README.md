# WASM GKE EXAMPLE

## First steps for develop Wasm Apps with Rust
1. Install rustup
2. Install wasm32-wasi (Local testing)
```
rustup target add wasm32-wasi
```
3. Install WasmEdge (Local testing)
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash
4. Create an application
```
cargo new api
```
5. Change to api directory
6. Create the main.rs file
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
wasmedge target/wasm32-wasi/release/api.wasm

## Running the Wasm Kubernetes Example
1. Create a GKE Cluster
```
gcloud container clusters create k8s-demo --num-nodes=2 --tags=allin,allout --machine-type=n1-standard-2 --no-enable-network-policy
```
2. Clone the repository
3. cd api-rest/src
4. /bin/bash build.sh YOUR_DOCKER_USER
5. Install NGINX Ingress controller
6. Get the Load Balancer IP and change it in the api-rest.yaml ingress section
5. kubectl apply -f api-rest.yaml

# REFERENCES
- https://github.com/WasmEdge/wasmedge_reqwest_demo
- https://wasmedge.org/docs/develop/rust/http_service/client