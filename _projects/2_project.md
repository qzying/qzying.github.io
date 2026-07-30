---
layout: page
title: triton-learning
description: A step-by-step tutorial to learn Triton GPU kernel programming
img: assets/img/triton-learning.png
importance: 2
category: work
---

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/triton-learning.png" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Learning path: Vector Add → Naive MatMul → Tiled MatMul → Fused Linear+GELU → Benchmark
</div>

**triton-learning** is a progressive, 4-step project for ML engineers (especially those in autonomous driving) to learn GPU kernel programming with Triton. No prior CUDA experience required — if you know PyTorch, you can start.

## Motivation

Autonomous driving models run on resource-constrained hardware (Orin, J6 BPU). To ship a model, you need to understand not just *what* your model computes, but *how* it maps to the chip. Triton is the bridge between PyTorch and GPU instructions — higher-level than CUDA, but low-level enough to control memory hierarchy and operator fusion.

## What You'll Learn

| Step | File | Key Concepts |
|------|------|-------------|
| 01   | `hello_triton.py` | `@triton.jit`, `program_id`, `tl.load/store`, mask |
| 02   | `matmul_naive.py` | 2D grid, stride calculation, why naive is slow |
| 03   | `matmul_tiled.py` | Tiling, shared memory, `tl.dot`, Tensor Core |
| 04   | `fused_kernel.py` | Operator fusion (Linear+GELU), register-level compute |
| 05   | `benchmark.py` | PyTorch vs Triton vs `torch.compile` comparison |

Each file is self-contained and runnable with a single `python` command.

## Why Operator Fusion Matters

In the fused Linear+GELU kernel, the intermediate activation never leaves the GPU registers — saving one HBM write and one HBM read per forward pass. For a transformer FFN block (Linear→GELU→Linear), the savings compound across dozens of layers.

---

<div class="repo-link">
  <a href="https://github.com/qzying/triton-learning" target="_blank" rel="noopener noreferrer">
    <i class="fab fa-github"></i> View on GitHub
  </a>
</div>
