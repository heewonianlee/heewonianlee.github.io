---
layout: default
title: Vivado Block Design
permalink: /projects/kv260/block-design/
---

[← KV260 project]({% link kv260.md %})

# Vivado Block Design

[View full size](https://github.com/user-attachments/assets/b517330d-5cb6-438a-b1ac-1944311ed069)

<h2>PS Side</h2>
<figure style="margin: 0 0 1.5rem;">
  <a href="https://github.com/user-attachments/assets/b517330d-5cb6-438a-b1ac-1944311ed069" aria-label="Open the complete Vivado block design" style="display:block; overflow:hidden; aspect-ratio:850 / 450;">
    <img src="https://github.com/user-attachments/assets/b517330d-5cb6-438a-b1ac-1944311ed069" alt="PS Side of the Vivado block design. The Zynq processing system starts the CNN through AXI-Lite." style="display:block; width:100%; height:100%; object-fit:cover; object-position:left center; cursor:zoom-in;">
  </a>
  <figcaption>The Zynq processing system starts the CNN through AXI-Lite.</figcaption>
</figure>

<h2>PL Side</h2>
<figure style="margin: 0 0 1.5rem;">
  <a href="https://github.com/user-attachments/assets/b517330d-5cb6-438a-b1ac-1944311ed069" aria-label="Open the complete Vivado block design" style="display:block; overflow:hidden; aspect-ratio:1110 / 450;">
    <img src="https://github.com/user-attachments/assets/b517330d-5cb6-438a-b1ac-1944311ed069" alt="PL Side of the Vivado block design. The CNN accelerator reads input features from DDR and model parameters from BRAM, then writes two output logits to DDR." style="display:block; width:100%; height:100%; object-fit:cover; object-position:right center; cursor:zoom-in;">
  </a>
  <figcaption>The CNN accelerator, designed in Vitis HLS, reads input features from DDR and model parameters from BRAM, then writes two output logits to DDR.</figcaption>
</figure>
