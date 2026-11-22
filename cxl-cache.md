---
layout: default
title: DRAM caching for CXL-attached SSDs
permalink: /projects/cxl-cache/
---

[← All projects]({{ '/' | relative_url }}#selected-projects)

# DRAM caching for CXL-attached SSDs

In my Yonsei capstone, I studied how much data a DRAM cache should hold per entry when it sits in front of a CXL-attached SSD.

## Experiment

I varied the cache granularity from 64 bytes to 16 KB and compared throughput for Get, Update, Delete, and Insert operations. A 4 KB entry matched the SSD page size and served as a reference point.

## Results

Among the sizes tested, 64-byte entries gave the highest throughput for Get, Update, and Delete. Insert performed best with 16 KB entries. The choice of granularity therefore depended on the workload's mix of operations.
