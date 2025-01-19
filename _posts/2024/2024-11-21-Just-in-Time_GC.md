---
title: "[Paper Review] To Collect or Not Collect: Just-in-Time Garbage Collection for High-Performance SSDs with Long Lifetimes"
categories:
  - Paper Review
tags:
  - Review
  - SSD
  - Garbage Collection

last_modified_at: 2024-11-21T21:00:00+09:00
toc: true
---

# Paper Info
Titile: Reinforcement Learning-Based SLC Cache Technique for Enhancing SSD Write Performance
Publisher: HotStorage'20
Initial release: July, 2020

# 읽은 이유
Study on dynamic SSD management.  

# Research Objectives
Managing garbage collection efficiently.  
Invoke timing of background GC operations which is a key parameter for effcient GC.  
High write performance, extend the SSD lifetime.  

# Summary
JIT-GC performs background garbage collection operations only when necessary.  
Therefore it reserves necessary free space in advance so that high write performance can be achieved while it exctends the SSD lifetime by preventing permature block erasures.   
Using an estimated future write demand from two predictors, JIT-GC decides when to invoke a BGC operation. 

## Motivation & Problem
### Research Scope
Since out-place updates generate invalid pages with old data, garbage collection is necessary to reclaim invalid pages for future writes.  
Since GC requires valid page migrations as well as block erasures, it incurs a significant performance overhead to a NAND flash-based storage system.  
In order to hide the performance penalty of a foreground GC operation, background garbage collection (BGC) is commonly used when a storage system is idle so that most foreground GC operations can be avoided.  
Careful considerations are required in deciding when to invoke BGCs.  
### Motivation
Selecting the right time for invoking a background GC operation is important for designing a high-performance SSD with a long lifetime.  
\- An aggressive BGC policy: reserves a large free space in advance, can avoid most FGC operations and achieves high write performance, but, its performance improvement comes with a shortened lifetime.  
\- A lazy BGC policy: maintains a small free space, does not sacrifice the lifetime of an SSD, but, some foreground GC operations may be needed.  

## Key Idea
Just-in-Time(JIT) GC technique: performs background garbage collection operations only when necessary, thus improving both the performance and lifetime of NAND-based storage systems. 
JIT-GC reserves necessary free space in advance so that high write performance can be achieved while it exctends the SSD lifetime by preventing permature block erasures.   
The timeliness of GC invocations depends on how much future writes are performed.  
JIT-GC employs two different prediction techniques in estimating the amount of future disk writes depending on the type of disk writes.  
\- For buffered writes, analyze the page cache management algorithm.  
\- For direct writes, JIT dedicates over-provisioning space and employs a heuristic predictor for estimating future directwrites.  
Using an estimated future write demand from two predictors, JIT-GC decides when to invoke a BGC operation.  
JIT-GC simultaneously achieves both the performance of an aggressive BGC policy and the life-
time of a lazy BGC policy.


# Note for Remembering
soon-to-be invalidated pages may be selected as GC victims