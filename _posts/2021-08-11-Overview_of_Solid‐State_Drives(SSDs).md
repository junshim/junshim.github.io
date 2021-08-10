---
title: "[SSDs] Overview of Solid‐State Drives (SSDs)"
categories: 
  - SSDs
tags:
  - SSDs

last_modified_at: 2021-08-11T18:00:00+09:00
toc: true
---

이번 포스트에서는 저의 주요 연구 관심사 중 하나인 Solid‐State Drives (SSDs)에 대한 간단한 소개를 해보고자 합니다. SSDs 이전의

# 작성 중에 있습니다.
last modified at 210811

# Storage Device
저장장치는 컴퓨터에서 데이터를 일시적으로, 또는 영구히 보존하는 장치를 말합니다. 메모리(Memory)라는 용어와 혼용되어 사용되기도 하며, 일반적으로, 일시적으로 데이터를 저장하는 장치를 **주기억 장치 (1차 저장장치, primary storage)** 와, 데이터를 영구희 보존하는 **보조 기억장치(2차 저장장치, secondary storage)** 로 나눌 수 있습니다.  

 1차 저장장치는 컴퓨터의 좋은 성능을 위해, 데이터를 CPU로 빠르게 전달하는 것을 주요 목표로 합니다. 즉, 데이터를 영구히 저장하지 못(휘발성, volatile memory)하더라도 빠른 성능을 필요로 하는 저장장치 입니다. 대표적으로 CPU cache로 이용되는 SRAM과 메인 메모리(Main Memory)로 사용되는 DRAM이 있습니다.  

2차 저장장치는 데이터를 영구히 보존하는 것을 목표로 하고, 따라서 기본적으로 **비휘발성 메모리(non-volatile memory)** 라는 특징이 필수적 입니다. ROM도 비휘발성 메모리에 포함되지만, 2차 저장장치로써는 전통적으로, Hard Disk Drive(HDD), 디스켓 드라이브, 마그네틱 테이프와 같이 마그네틱 메모리를 사용하는 장치들이 사용되어 왔습니다. 하지만, **최근에는 플래시 메모리(Flash Memory)를 기반으로하는 SSDS와 같은 저장장치가 각광 받고 있습니다.**

저는 이번 포스트에서 HDD에 대한 얘기를 시작으로 SSDs 얘기를 해보도록 하겠습니다.

# Hard Disk Drive (HDD) : SSDs 이전의 대표 2차 저장장치
지금은 2차 저장장치로써 SSDs가 많이 사용되고 있지만, 약 10년 전만해도 전세계 모든 2차 저장장치의 역활은 HDD가 수행했다 해도 과언이 아닐 것입니다. 이번 장에서는 이 HDD에 대해 간단하게 설명해 보도록 하겠습니다.