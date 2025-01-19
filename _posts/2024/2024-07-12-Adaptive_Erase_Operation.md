---
title: "[Paper Review] AERO: Adaptive Erase Operation for Improving Lifetime and Performance of Modern NAND Flash-Based SSDs"
categories:
  - Paper Review
tags:
  - Review
  - SSD
  - Erase

last_modified_at: 2024-07-15T21:00:00+09:00
toc: true
---

# Paper Info
Titile: AERO: Adaptive Erase Operation for Improving Lifetime and Performance of Modern NAND Flash-Based SSDs
Publisher: ASPLOS'24
Initial release: May 2024

# 읽은 이유
Reading papers in my field of interest.

# Scopes of Work
NAND Flash Memory.  
Improve the lifetime and performance of modern SSDs.  

# Summary
Goal: Improve the lifetime and performanbce of modern SSDs by mitigating the negative impact of erase operations.  
Propose AERO (Adaptive ERase Operation).  
Dynamically adjust erase latency to be just long enough for reliably erasing the target cells, depending on the cells' current erase characteristics.  

## Motivation Problem
### The efficiency of erase operation significantly affects SSD lifetime and I/O performance.
The high erase voltage physically damages flash cells, a flash cell cannot reliably store data, limiting SSD lifetime.  
Erase latency (3.5 us) is significantly higher than read and write latencies (40 us, 350 us).  
\- lifetime perspective -> much higher impact on cell endurance compared to a program operation that applies a high voltage.  
\- I/O performance poerspective, an erase operation often delays I/O requests for a long time,. incrasing the SSD tail latency.  

### Erase failure occurs quite frequently in modern SSDs with density-optimized NAND flash chips.
Modern SSDs often requires multiple erase loops (ISPE, Incremental Step Pulse Eraure).  
Erase faliure: default erase voltage may fail to sufficiently erase where a falsh cell becomes more P/E cycles.  
Retries an erase loop with progressively higher voltages until it can successfully erase all the cell in the block.  

### No work has yet investigated how erase latency should be set to fully exploit the potential of NAND flash memory.  
Most existing techniques use a fixed latency for every erase operation which is set to cover the worst-case operation conditions.  
A fixed latency for every erase operation, which is set by the manufacturers at design time based on the worstcase operating conditions.  
Morden NAND flash memory also exhibits high process variation, which introduces significant differences in physical characteristics across flash cells.  
This means that flash cells frequenctly suffer from more erase-induced damage than needed, degrades both SSD lifetime and I/O performance.  

### Challengint to accurately predict the minimum latency for a block in mordern NAND flash memory.
There are a strong correlation between the erase latency and P/E-cycle count (PEC) of a block.  
But, PEC alone is insufficient for accurate prediction of the monimum erase latency due to high process variationacross blocks.  
In real NAND flsh chips, show high erase-latency variation even across blocks with the same PEC.  

## Key Idea
### AERO ( Adaptive ERase Operation )
A new erase scheme that dynamically adjusts erase latency to be just long enough for reliably erasing target cells, depending on the cells' current erase characteristics.  
Preduct near-optimal erase latency based on the number of fail bits during an erase operation.  
First, attempt to erase the cells for a short time (1ms), obtain the nuber of fail bits necessary to accurately predict the near optimal erase latency.  
\- shallow erasure, apply the erase voltage for a reduced amount of time  
Second, Aggressively yet safely reduces erase latency by leveraging a large reliability margin present in modern SSDs.  
\- remainder erasure, complete erasing the block only with the necessary time determined based on the fil-bit ocunt in the shallow eraure.  
Leverage the high error-correction capability of modern SSDs to further reduce erase latenccy without compromising reliability.  
\- Modern SSDs adopt ECC, large ECC-capavility margin, reduce erase latency for certain operating conditions.  
No modification to NAND flash chips, only to existing SSD firmware or controller, achieving high applicability and practicality.  

### Fail-bit-count-based Erase Latency Prediction (FELP)
Predicts near-optimal latency for an erase loop based on the number of fail bits that occur in the previous loop.  
At the end of each erase loop, the ISPE scheme senses all the cells in the target block simulaneously and counts the number of fil bits (the number of bitlines that contain one or more insufficiently-erased cells).  
Find that the fail-bit count can be an accurate proxy for the minimum latency of the next loop.  
Construct a model between the fail-bit count in an erase loop and the minimum erase latency required for the next loop.  

# Note for Remembering
NAND flash memory, erase operation, high voltage (over 20V), long time (over 3.5 ms).  
