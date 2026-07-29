---
layout: page
title: unified-runtime
description: A compile-time multi-backend inference runtime
img: assets/img/unified-runtime.svg
importance: 1
category: work
---

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/unified-runtime.svg" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Architecture overview: Application Code → Common API → Backend Selection → CPU / TensorRT / Horizon
</div>

**unified-runtime** is a compile-time multi-backend inference runtime, with a design influenced by MNN and ONNX Runtime. It separates application code from hardware backends through a common API, enabling the same model-serving code to target different chips without modification.

## Architecture

The runtime abstracts hardware backends behind two interfaces:

- **`IBackend`** — responsible for compiling a model into a backend-specific executable form.
- **`ICompiledModel`** — holds the compiled result and executes inference.

Backend selection happens at model load time (not per-operator), and one model maps to one chip at deploy time. Application code is identical across all backends.

## Supported Backends

| Backend    | Status          | Notes                                                              |
| ---------- | --------------- | ------------------------------------------------------------------ |
| **CPU**    | Built-in        | C++ executor with MatMul, Conv, ReLU, and other core ops           |
| **TensorRT** | Simulation    | Replaceable with `nvinfer1::createInferBuilder()` on Orin/Thor     |
| **Horizon**  | Simulation    | Replaceable with `hbDNNInitializeFromFiles()` on J6 BPU            |

## Adding a New Backend

To add a new backend, implement the `IBackend` and `ICompiledModel` interfaces and register it via a static `BackendRegistrar`. The rest of the codebase and all existing application code works unchanged.

---

<div class="repo-link">
  <a href="https://github.com/qzying/unified-runtime" target="_blank" rel="noopener noreferrer">
    <i class="fab fa-github"></i> View on GitHub
  </a>
</div>
