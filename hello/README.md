# Hello WasmEdge

This is a modified version based on the course, as I use Podman, crun, and microk8s.
Please rust and WasmEdge first! See [WasmEdge: Podman](https://wasmedge.org/docs/develop/deploy/podman/) for the specific stack used here.

## Quick start with Podman 

```
$ podman run --rm --annotation module.wasm.image/variant=compat localhost/baugereau/rust-example-hello.wasm:latest
```

## Code

The [`src/main.rs`](src/main.rs) source code shows

* A standalone Rust app must have a `main()` function as the entry point.
* In the `main()` function, we create a string. The string is `&str` type. In Rust, it is called a string slice meaning that this string is immutable.
* We print the string using the `println!()` marco. The `!` indicates that it is a macro, which is a set of functions to perform the task of printing to the OS console. The Rust compile expands the macro into a set of functions at compile time. There are many such macros in Rust and it is crucial that you learn them!

## Step by step guide

Compile the Rust source code project to a Wasm bytecode file.

```
$ cargo build --target wasm32-wasip1 --release
```

Run the Wasm bytecode file in WasmEdge CLI.

```
$ wasmedge target/wasm32-wasip1/release/hello.wasm
Hello WasmEdge!
```

## Build and publish on Docker Hub

The `Containerfile` follows the above steps to build and package a lightweight OCI-compliant container image for the Wasm app.
Now, we need to publish the container image to Docker Hub. The process is slightly different depending on how you plan to use the image.

### For Podman and crun 

For crun based systems, such as the Podman and microk8s Kubernetes, you just need to add an annotation specifying that the image is a `compat` or `compat-smart` variant (the latest should let the container runtime detecting that it is a wasm image).

```
$ podman build --annotation module.wasm.image/variant=compat -t docker.io/baugereau/rust-example-hello.wasm:latest .
... ...
$ podman login docker.io
... ...
podman push docker.io/baugereau/rust-example-hello.wasm:latest
```
