---
layout: default
title: KV260 CNN Optimizations
permalink: /projects/kv260/optimizations/
---

[← KV260 project]({% link kv260.md %})

# Core Optimization Techniques

## 1. Parallelism Tuning

### Why

Full channel-level parallelism in the deeper convolution layers consumed enough FPGA resources to make full-CNN integration difficult.

### How

- I used two-lane processing in Conv Block 1 to increase parallelism in the early layers.
- In Conv Block 3, I reused each set of MAC lanes across two phases. `Conv3_1` processed 32 input channels with 16 lanes per phase. `Conv3_2` processed 64 input channels with 32 lanes per phase.
- Within each lane, all nine taps of the 3 × 3 kernel still ran in parallel.

### Result

HLS synthesis estimates for Conv Block 3:

| Metric | Full Parallel | Reuse16/32 | Change |
| --- | ---: | ---: | ---: |
| Latency | 1.730 ms | 2.603 ms | +50.5% |
| DSP | 449 | 255 | **−43.2%** |
| LUT | 94,423 | 77,071 | **−18.4%** |
| FF | 79,159 | 53,117 | **−32.9%** |
| BRAM_18K | 19 | 19 | No change |

Reusing the MAC lanes freed 194 DSPs for the rest of the CNN at the cost of 0.873 ms of additional Block 3 latency.

## 2. Data Reuse & Streaming

Within each convolution, a line buffer retains recent rows while a sliding window reuses the activations shared by adjacent 3 × 3 windows. Between layers, I passed intermediate activations through `hls::stream` FIFOs. HLS `DATAFLOW` lets a downstream layer start once it has enough input, so intermediate feature maps stay on chip throughout inference.

## 3. Memory Architecture Optimization

Each Block 3 lane needs nine INT8 weights for a 3 × 3 kernel. I packed those weights into one 72-bit word so a lane could fetch all nine taps in one read. This reduced the logical weight-bank counts from 144 to 16 and from 288 to 32 for the respective lane configurations. I used `ARRAY_PARTITION` across lanes to supply concurrent reads and selected LUTRAM or BRAM for the resulting on-chip arrays.

## 4. Arithmetic & Bit-Width Optimization

I used INT8 weights and activations and explicitly mapped selected multiplications to DSPs. In one convolution datapath, an INT8 × INT8 multiply produces a 16-bit product. The partial sum uses 19 bits, and the reduction uses 23 bits. A layer-specific shift requantizes the result to INT8 for the next layer.

## Supporting implementation details

I reduced the configured stream FIFO depth from 64 to 16 entries and partitioned the bias and fully connected arrays to support their access patterns. The sequential parameter loader targets an initiation interval of one cycle with `PIPELINE II=1`.

## Current bottleneck: parameter loading

Before the DATAFLOW region starts, the accelerator copies weights and biases into internal arrays. In the 100 MHz Vivado RTL simulation, that preload took **3.434 ms of the 5.969 ms inference (57.5%)**. It exceeded the HLS load estimate of 0.763 ms by 2.671 ms, accounting for about 89% of the total estimation gap. The transfer path is the first place I would look to reduce frame latency.

## Early HLS study: MAC cascade and memory banking

In an earlier cascaded MAC design, I changed the bank count from six to eight to simplify bank indexing. HLS estimated **78 DSPs with six banks and 9 DSPs with eight banks**, with the same estimated latency of 4.354 ms in both configurations.

[Accelerator implementation and timing breakdown on GitHub](https://github.com/heewonianlee/cnn-hls-accelerator)
