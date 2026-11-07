# Intel Arc Pro B65 Krea-2 LoRA Training Performance Report

Real benchmark data for Krea-2 LoRA training on Intel Arc Pro B65 GPUs.

## Key Results

| Metric | Single B65 | Dual B65 x2 | Delta |
|--------|-----------|-------------|-------|
| Throughput | 0.0515 samples/s | 0.0766 samples/s | **1.49x speedup** |
| Step Time | 19.4 s/it | 26.1 s/it | Dual processes 2 samples/step |
| Same-Data Time | 11.9h | ~8.0h | **33% faster** |
| VRAM | 31 GiB | 31 GiB x2 | Full fp8 model resident |

## Core Finding

The 31 GB VRAM of the B65 enables **swap=0 training** - the full 13 GB fp8 model resides entirely in GPU memory with zero PCIe transfers. This is the hard advantage over 16 GB cards, which require block-swap and suffer PCIe bottleneck.

Dual-GPU delivers **1.49x throughput** via data parallelism (2 samples per step) minus 27% communication overhead.

## Report

- **[index.html](index.html)** - Full training performance report (English)
- **[images/](images/)** - Loss curves and comparison charts

## Test Environment

- CPU: AMD Ryzen 5 9600X
- RAM: 32 GB DDR5
- GPU: Intel Arc Pro B65 (31.16 GiB, ECC off) x1 or x2
- Model: Krea-2 RAW bf16 (26.28 GB)
- Framework: musubi-tuner + 8 XPU patches
- Dataset: 37 images (dual-bucket), 2200 steps, seed 42

## Price-Performance

| Configuration | VRAM | Training Throughput | Reference Price |
|--------------|------|-------------------|-----------------|
| Single B50 | 16 GB | Constrained | ,499 CNY |
| **Single B65** | **31 GB** | **0.0515 samples/s** | **,999 CNY** |
| **Dual B65** | **31 GB x2** | **0.0766 samples/s** | **9,998 CNY** |

---

*All numbers from real training logs on actual hardware. No estimates.*
