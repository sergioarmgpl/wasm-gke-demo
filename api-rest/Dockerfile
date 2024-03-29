FROM rust:1.77.0 as build-env
WORKDIR /app
ADD . /app
RUN rustup target add wasm32-wasi
RUN cargo build --target wasm32-wasi --release

FROM wasmedge/slim-runtime:0.10.1
COPY --from=build-env /app/target/wasm32-wasi/release /
CMD ["wasmedge", "--dir", ".:/", "/api-rest.wasm"]
