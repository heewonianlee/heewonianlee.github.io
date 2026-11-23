---
layout: default
title: Engineering Portfolio
---

# Engineering Portfolio

I'm Ian, an Electrical and Electronic Engineering undergraduate at Yonsei University and an exchange student at Michigan State University. I study how hardware design choices affect the speed and resource use of machine learning and memory systems.

## Selected projects

### [An HLS-Based Audio CNN Accelerator for the Kria KV260]({{ '/projects/kv260/' | relative_url }})

At MSU's HATLab, I use Vitis HLS to build a CNN accelerator for speaker verification. I compare convolution designs by latency, DSP use, and on-chip memory use, then integrate the accelerator with the processing system in Vivado.

### [DRAM caching for CXL-attached SSDs]({{ '/projects/cxl-cache/' | relative_url }})

For my Yonsei capstone, I studied how the amount of data cached per entry affects throughput. I compared cache granularities from 64 bytes to 16 KB across reads, updates, deletes, and inserts.

### [A Verilog accelerator for handwritten digits]({{ '/projects/mnist-rtl/' | relative_url }})

I implemented an FPGA accelerator for MNIST classification. The design uses pipelined processing elements, reuses data across convolutions, and prefetches weights from DDR3.
