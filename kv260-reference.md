---
layout: default
title: Reference paper for the KV260 audio CNN
permalink: /projects/kv260/reference/
---

[← KV260 project]({% link kv260.md %})

# Reference paper

[An FPGA-Accelerated Audio CNN for Alzheimer’s Disease Detection Using Hardware–Software Co-Design](https://doi.org/10.1109/MWSCAS67364.2026.11680880)  
IEEE International Midwest Symposium on Circuits and Systems, 2026.

## What the paper implemented

| Stage | Operation |
| --- | --- |
| Input | 400 × 64 log-Mel spectrogram, one channel |
| Conv block 1 | Two 3 × 3 convolutions, 16 filters each |
| Conv block 2 | Two 3 × 3 convolutions, 32 filters each |
| Conv block 3 | Two 3 × 3 convolutions, 64 filters each |
| Classification head | Global average pooling → Dense (64, ReLU) → Dense (2 logits) |

Each convolution is followed by ReLU, and each block ends with 2 × 2 max pooling. The classification head uses dropout (0.3) during training.

Source: [Sheikh and Yazdi (2026), Fig. 2](https://doi.org/10.1109/MWSCAS67364.2026.11680880).

The authors classified speech samples using a CNN that takes log-Mel spectrograms as input. On the Kria KV260, the ARM processor handled audio preprocessing and the FPGA's DPU ran the INT8 network. The authors reported an inference latency of 0.876 ms.

## How I used the reference

I used the paper's inference latency as a benchmark for my custom HLS accelerator on the same FPGA platform. The paper measured a deployed DPU running at 300 MHz. My 5.969 ms result covers the interval from accelerator start to completion in a 100 MHz full-system RTL simulation. Both the clock rate and the measurement method matter when interpreting these numbers.
