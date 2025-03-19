---
title: "[Paper Review] Understanding and Optimizing Hybrid SSD with High-Density and Low-Cost Flash Memory"
categories:
  - Paper Review
tags:
  - Review
  - Hybrid SSD
  - QLC

last_modified_at: 2025-02-07T21:00:00+09:00
toc: true
---

# Paper Info
Titile: Understanding and Optimizing Hybrid SSD with High-Density and Low-Cost Flash Memory  
Publisher: ICCD'15

# 읽은 이유
# Research Objectives
This paper reveals that previous studies on hybrid SSDs have not fully explored internal behaviors, suggesting that performance, endurance, and reliability can be optimized through better coordination between SLC and QLC modes.
Improve bandwidth and performance with reducing GC-induced writes.
Optimize inefficient GC in the SLC region.  

# Summary
Proose HyFlex, which redesigns the strategies of data placement and flash-modemanagement of hybrid SSDs.
Two novel optimization strategies are proposed: velocity-based I/O scheduling (VIS) and garbage collection (GC)-aware capacity tuning (GCT)  

## Research Scope
Hybrid SSDs: High-density and low-cost flash memory, two flash modes can be dynamically switched, such as single-level cell (SLC) mode and quad-level cell (QLC) mode.  

## Motivation & Problem
Modern SSDs with high-density flash memory have increased capacity but lower performance and reliability, adopting a hybrid architecture with a partitioned SLC region to achieve both high performance and capacity simultaneously.  
pseudo-SLC technique.  
Extensive research have not adequately investigated the internal behaviors.
There are the performance potential which has not been fully explored.

### Two interesting findings in internal mechannisms in Hybrid SSDs
First, the I/O performance of hybrid SSDs can be worse than either SLC or QLC flash individually when data is continuously written to the SLC buffer.  
Second, the small size front-end SLC region can suffer from inefficient GC-induced performance fluctuation.  

## Key Idea
This paper revisits the internal architecture of modern hybrid SSDs and proposes HyFlex.  
The key idea of HyFlex is to re-design the strategies of data placement and flash-mode management in a flexible approach.  

### Velocity-based I/O scheduling (VIS)
Avoid a large amound of data migration between the SLC and QLC regions.  
1. Monitor velocity of new data written to the hybrid SSDs and determine the scheduling of subsequent I/O requests.  
2. High velocity, it means that there is a large amount of new data, should be scheduled to the QLC region to avoid SLC region being filled quickly.  
3. Low veloicty, they are scheduled to the SLC region to maintain high access performance.  

### GC-aware capacity tuning (GCT)
Flexible tune the size of the SLC region to avoid the infefficient GC in this region.  
Rent some QLC blocks to the SLC region when the GC is inefficient.  

# Note for Remembering