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
At the beginning of each epoch, estimate the number of bytes that is to be written during the epoch, based on the number of bytes actually written to teh SSD during the latest epoch.  
Enterprise workloads often exhivit cyclic behavior with periods between servaral minutes and several days.  
This means that if the length of an epoch is properly decided to include the cyclic period of a workload, the write demand observed in the latest poch can be used as a factor that indicates future write demands.
The best epoch length may be different depending on a workload and its characteristic.

Throttling delay estimator: determines a throttling delay by considering both the future write demand and the remaining lifetime of SSDs.  
At the beginning of each i-th epoch, the delay estimator increases or decreases a throttling delay, based on the expected write demand and the capacity of each epoch.  
The expected write demand ($w_{i}$) is equal to the number of bytes written during the (i-1)-th epoch.  
The capacity of i-th epoch ($c_{i}$) is determined by dividing the remaining capacity of SSD by the number of remaining epochs.  
If $w_{i} == c_{i}$, then $t^{delay}_{i} == t^{delay}_{i-1}$.  
If $w_{i} > c_{i}$, then $t^{delay}_{i} == t^{delay}_{i-1} + \Delta t^{delay}_{i-1}$, where $\Delta t^{delay}_{i-1} = t_{epoch} \cdot (w_{i}/c_{i} - 1) / n$.  
If $w_{i} < c_{i}$, then $t^{delay}_{i} == t^{delay}_{i-1} - \Delta t^{delay}_{i-1}$, where $\Delta t^{delay}_{i-1} = t_{epoch} \cdot (c_{i}/w_{i} - 1) / n$.  
The estimator then calculates the remaining capacity based on teh achievable P/E cycles and distributes it to the remaining epochs.
The number of achievable P/E cycles is estimated, using the average idle time of every block in the SSD.

Epoch-capacity regulator: apply a throttling delay to each write request and adopt an epoch-capacity enforcement policy.  
Once a throttling delay is decided, we throttle SSD performance by distributing throttling delays across evert write as evenly as possible.  
If write demand prediction fails and unexpectedly high write traffic comes from the host, it can not guarnantee the required lifetime.  
Do prevent more data than the epoch capacity from being written to the SSD.  
Pessimistic epoch-capacity enforcement policy: Stop writing if the epoch capacity is likey to be exhausted before the epoch ends.  
\- Divide one epoch into periods whose lengths are 1 second each and then distributes the capacity of an epoch to all periods evenly.  
\- If there is an unused, reallocates it to the next period.  
\- Maintain the minimum write throughput.  
\- If stop after the eoch capacity is exhausted, significant performance degradation.  
\- Does not efficiently handle a bursty I/O pattern.  
Optimistic epoch-capacity enforcement policy: maintains a relatively small amount of the spare capacity for each epoch and forcibly throttles write performance only when both the capacity of a period and the spare capacity are exhausted.  
\- The spare capacity must be carefully chosen.  
\- In this work, the spare capacity is empirically set to 10% of the remaining capacity of the SSD.  


### Epoch Length Selection
Since a throttling delay is determinced by the write demand of the previous epoch, the incorrect poch length can make a large fluctuation in teh overall I/O reponse time of the SSD.  
Monitor write demands of a workload and fine repeated cycles that show similar write demands.
The new epoch length is determined under the assumption that there are no throttling delays.
Calculate the write-demand difference ratio of two consecutive epochs i and i+1 with the same length.
Calculate the average write-demand difference ratio.
INcrrease the length of a candidate epoch by one unit-time window and repeat.
Chose smallest difference ratio as epoch length.
To mitigate the computational overhead caused by epoch length selection, the poch length is recalcuated when the write-demand difference ratio between the predicted write demand and actural one is higer than 0.25, three times successively.





# Note for Remembering
## Endurance Characteristics of Flash Memory
P/E operations inevitably cause damage to floating-gate transistors, reducing the overall endurance of memory cells.  
At the device level, memory cells are gradually worn out as charges get trapped in the inferface and oxide layers of a floating-gate transistor during P/E cycles.  
This charge trapping increase the threshold voltage of a floating-gate, which indicates a logical bit values of a cell, and the cell becomes unreliable when the threshold voltage is higher than a certain voltage margin.  

A floating-gate transistor also has a self-recovery property which heals the damage of a cell by detrapping charges captured in the oxide of a cell.   
This recovery(or detrapping) process occurs during the idle time between P/E cycles on the same cell, and its effect in general increases as the logarithm of the idle time.  
External temperature and a programmed threshold voltage affect the cell recovery.  
The achievable P/E cycles are gradually increased in proportional to the length of the idle time.   