---
title: "Review of CPU Power Consumption Experiment and Results Analysis of Intel i7-4820k"
categories:
  - Paper Review
tags:
  - Review
  - Technical Report 
  - Power
---

# Review
Title : CPU Power Consumption Experiment and Results Analysis of Intel i7-4820k
Author(s) : Matthew Travers(Newcasthle University)
Link : http://async.org.uk/tech-reports/NCL-EEE-MICRO-TR-2015-197.pdf

# 읽은 이유
 연구를 진행하면서 Intel's RAPL을 활용할 필요가 있었고, RAPL을 활용해서 얻은 cpu core의 energy 값을 검증하기 위한 방법을 찾고 있었다. 해당 논문에서 Intel i7-4820k에서 RAPL과 직접 측정 방식 모두를 통해 데이터를 측정했다고 해서 읽게 되었다.  
  
# 요약
 **해당 논문은 Technical Report로 Multi-core를 통해 energy를 save할 수 있다는 사실을 실험을 통해 증명하였고, 결과 보다는 실제 Intel CPU 환경에서 어떻게 실험을 했나를 중점적으로 볼만한 논문이었다.**  
   
#### 종합
CPU에서 사용하는 Power에 대한 기본 개념을 잘 설명이 되어있다. (**2.1 Power Consumption of CPU Theory**)  
 **Intel CPU에서 power를 측정하기 위해 사용한 software에 대한 설명을 잘해 두었다.** 해당 software들을 어떻게 이용했으며 어떤 결과를 얻었는지 기술이 잘 되어 있어, 이 것들의 사용 방법과 사용 이유를 잘 정리해두면 추후에 도움이 될 수도 있을 것 같다. 
 해당 논문에는 **왜 Device에서의 Power 관리가 중요한가**를 잘 설명하고 있다.  그것을 기반으로 정확한 측정이 중요한 이유도 설명하고 있다. 나중에 power 관련 논문을 작성할 때 참고하면 좋을 듯.
 실측 하는 방법과 RAPL 사용 방안에 대해 공부해 보고 싶었는데 생각보다 원하는 만큼의 정보를 얻진 못했다.

  
#### RAPL
 해당 논문에서는 RAPL의 값이 Voltage를 고려하지 않고 출력되는 것으로 보아 pre-defined된 값을 출력해주는 것 같고, 이로인해 false power로 인한 문제를 만들 수 있을 것 같다고 한다.  
  
#### Intel CPU power 실측  
 해당 논문에서 사용한 실측 방법은 CPU의 power supply  circuit에 **shunt register**를 병렬적으로 설치하여 해당 레지스터를 통해 사용하는  power를 측정했다고 한다. 해당 작업을 위해선 특수한 motherboard가 필수인 것으로 보인다. 논문에서는 EATX12V connection을 통해 파워를 공급하는 motherboard를 이용했다고 한다.  **하지만 해당 방식이 CPU에서 사용하는 모든 power값을 대표한다고 할 수 없다고 한다(5.3).** 그래서 RAPL을 사용해 보고자 한 것 같지만, 그것도 완전히 정확한 것은 아닌듯.
  현재 실측 방식을 하지 않고 RAPL값을 믿기로 결정해서 해당 방법을 진행하지 않지만 후에 필요가 하다면 이 부분을 더 깊게 조사해 봐야 할 것 같다.

# 사용한 software 정리
Software는 CPU를 가동하기위한 것, frequency측정, voltage측정, power consumption, energy consumption을 직접 측정하는 방법들로 나눌 수 있을 것 같다. 우분투에서 사용할 수 있는 것들임  
  
#### Top
Display information on current processes running and which cores they are running on.  
그냥 리눅스에서 사용하는 top, htop 이거 말하는 듯  
  
#### Sensors
This program is used to view information on core voltages and temperature.  
watch command   
  
#### Cpufreq
  
This program is usefull for both monitoring and setting parameters of the CPU.  
``sudo  cpufreq-set``
``sudo cpufreq-info``
  
#### Stress
Set up stress situation on individual cores. It is completely computational and does not stress the hard drive or memory.  
``stress -c 1 -t 30s`` : run 1 process for 30 seconds.  
  
#### Sys benchmark
Primary benchmarking program. It works by testing if numbers are prime numbers or not to a user defined maximum.
It is completely computational and uses no memory and therefore is a good test for measuring the power consumption of CPU's. 
 ``sysbench -test=cpu -cpu-max-prime=20000 run`` : test up to 20000 for prime numbers.
   
#### Likwid
Likwid can dispaly a software model of power consumption based on Intel's RAPL counters.
``likwid-powermeters -s 1``
  

# History
210104 첫 작성
> Written with [StackEdit](https://stackedit.io/).
