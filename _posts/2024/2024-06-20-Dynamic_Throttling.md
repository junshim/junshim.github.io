---
title: "[Paper Review] Lifetime Management of Flash-Based SSDs Using Recovery-Aware Dynamic Throttling"
categories:
  - Paper Review
tags:
  - Review
  - SSD
  - Throttling

last_modified_at: 2024-06-20T21:00:00+09:00
toc: true
---

# Paper Info
Titile: "Lifetime Management of Flash-Based SSDs Using Recovery-Aware Dynamic Throttling"  
Publisher: FAST'12  
Initial release: FEB 12

# 읽은 이유
Dynamic Throttling 공부
연구실 논문

# Scopes of Work
Limited and unpredictable lifetime of SSDs.  
SSDs in enterprise systems.  

# Summary
Propose a novel recovery-aware dynamic throttling technique, called READY, which guarantees the SSD lifetime required by the enterprise market while exploiting the self-recovery effect of floating-gate transistors.  

## Motivation Problem
### The poor write endurance of NAND flash memory, two key problems on SSD lifetime
### First, The endurance of flash device is rapidly decreasing
The enduracne is dependent upon the number of program/erase (P/E) cycles.  
The endurance of a floating-gate transistor is significantly degraded, as the semiconductor process is sacaled down ad with multi-level cell technology.  

### Seoncd, unpredictable lifetime of flash devices.
The SSD lifetime is determined by extra data written by garbage collection and wear-leveling as well as by the number of bytes writeen by application.  
The SSD lifetime is a function of a workload.  
Strongly depends on the write intensiveness of the workload.  

### The limitation of No throttling and Static throttling
In the example of Figure 2(a) which does not use write throttling, the required lifetime cannot be satisfied because the number,$W_{work}$, of bytes written to the SSD exceeds $C_{ssd}$ before $C_{ssd}$.  

Static throttling guarantees the required lifetime by limiting the maximum bandwidth of the SSD to a certain fixed value,
which is denoted by Bstatic.  
Static throttling determines the value of Bstatic based on the assumption of the worst case scenario where the number of bytes written per second is always larger than $C_{ssd}$/$C_{ssd}$.  
The drawback of this approach is that it is likely to underutilize the maximum endurance of the SSD.   
Actual workloads may not be that intensive all the time.  
The I/O response time is degraded with static throttling.  

### The dynamic throttling technique inevitably reduces the overall write performance.  


## Key Idea
### READY: Recovery-Aware DYnamic throttling technique
Throttle write performance by adding throttling delays to write requests, so as to guarantee the required SSD lifetime.
With dynamic throttling, the IOPS and bandwidth of SSDs is reducced to a certain extent.
More optimistic on the total number of data written, thus employing a smaller throttling delay.  

By dynamically throttling write requests write requests according to the characteristics of a workload and the remaining SSD lifetime, READY fully utilizes the given endurance of the SSD up to the maximum, while minimizing performance degradation.  

Focus on two aspects of the design requirements of SSDS.  
First, throttling delay as low as possible.  
Seoncd, distribute a throttling delay over every write request as evenly as possible.

### Make throttling decisons dynamically based on the predicted future write demand of a workload  
The required SSD lifetime can be guraneed with less performance dgradation.  
The dynamic throttling technique inevitably reduces the overall write performance.  
Determin throttling delay by predicting future write demands and distribute the predicted delay over the entire SSD lifetime => Better wrtie response time can be optained withe less variation on the response time.  

### Self-recovery effect of floating-gate transistors  
Improves the endurance of SSDs  
Enabling to guarantee the required lifetime with less write throttling.  
The damage caused by repetitive P/E cycles can be partially recovered during the idle period between two consecutime P/E cylces, inproving the endurance of a floating-gate transistor.  

### Three main functions.  
Write demand predictor: predicting future write demands.  
The entire lifetime($T_{ssd}$) of the SSD is divided into epochs.
At teh beginning of each epoch, estimate the number of bytes that is to be written during the epoch, based on the number of bytes actually written to teh SSD during the latest epoch.  
Enterprise workloads often exhivit cyclic behavior with periods between servaral minutes and several days.  
This means that if the length of an epoch is properly decided to include the cyclic period of a workload, the write demand observed in the latest poch can be used as a factor that indicates future write demands.
The best epoch length may be different depending on a workload and its characteristic.

Throttling delay estimator: determines a throttling delay by considering both the future write demand and the remaining lifetime of SSDs.  
Epoch-capacity regulator: apply a throttling delay to each write request.  


# Note for Remembering
## Endurance Characteristics of Flash Memory
P/E operations inevitably cause damage to floating-gate transistors, reducing the overall endurance of memory cells.  
At the device level, memory cells are gradually worn out as charges get trapped in the inferface and oxide layers of a floating-gate transistor during P/E cycles.  
This charge trapping increase the threshold voltage of a floating-gate, which indicates a logical bit values of a cell, and the cell becomes unreliable when the threshold voltage is higher than a certain voltage margin.  

A floating-gate transistor also has a self-recovery property which heals the damage of a cell by detrapping charges captured in the oxide of a cell.   
This recovery(or detrapping) process occurs during the idle time between P/E cycles on the same cell, and its effect in general increases as the logarithm of the idle time.  
External temperature and a programmed threshold voltage affect the cell recovery.  
The achievable P/E cycles are gradually increased in proportional to the length of the idle time.   