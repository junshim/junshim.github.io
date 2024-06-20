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
### The poor write endurance of NAND flash memory
  

## Key Idea
### Make throttling decisons dynamically based on the predicted future write demand of a workload  
the required SSD lifetime can be guraneed with less performance dgradation.  

### Self-recovery effect of floating-gate transistors  
Improves the endurance of SSDs  
Enabling to guarantee the required lifetime with less write throttling.  

# Note for Remembering