---
layout: default
title: A Verilog accelerator for handwritten digits
permalink: /projects/mnist-rtl/
---

[← All projects]({{ '/' | relative_url }}#selected-projects)

# A Verilog accelerator for handwritten digits

I implemented an FPGA accelerator for MNIST digit classification in Verilog. Its CNN runs a 3 × 3 convolution, ReLU, max pooling, and a fully connected layer.

## Datapath and control

I organized convolution around pipelined processing elements. The design processes data in tiles so several calculations can reuse values already on chip. It also prefetches weights from DDR3 for upcoming operations.

## Hardware and software integration

A finite-state machine sequences the accelerator's work. I used MicroBlaze and Vitis on the software side to control the hardware and tested the classifier with MNIST images.
