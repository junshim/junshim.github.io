---
title: "Embedded Recipes"
categories:
  - Embedded Recipes
tags:
  - Embedded System
  - 공부
  - 책정리
last_modified_at: 2025-01-07T18:00:00+09:00
toc: true
---

해당 카테고리에서는 **Embedded Recipes**을 공부하면서 기억해 둘 것을 정리해논 곳 입니다.  
Start: 2025 01 07
Last: 2025 01 08
End:

# 1. Hardware Collage
시간 축의 신호, 주파수 영역의 주파수  
+\) 신호는 시간에 따라 변하는 물리적 양  
+\) 주파수는 진자운동에서 단위 시간당 같은 것이 일어나는 횟수  
Fourier Transform:세상의 모든 신호는 cos sin의 합으로 나타낼 수 있다.  
+\) 시간 영역에서 다루기 힘든 내용을 주파수의 영역에서 해결하기 위해 사용  
+\) 신호가 얼마나 다양한 주파수 성분으로 구성되어 있는 지를 나타냄   
+\) 주파수 영역에서 표현되는 모든 주파수를 시간 영역에서 더하면 원래의 신호로 복원  

Analog 신호는 AC와 DC 성분으로 이루어짐  
+\) AC(교류)는 극성이 바뀌는 신호로 주파수를 가지기만 하면 됨, 크게 보면 주파수 0인 신호 외 모든 것  
+\) DC(직류)는 극성이 바뀌지 않고 steady 한 상태의 신호, 크게 보면 주파수가 0인 신호
Digital 신호는 DC 성분으로만 따짐
+\) 신호는 한계값/임계값(Threshold)가 특정 값 이상이면 high, 아니면 low  
+\) 신호가 없냐 있냐로만 따짐  
+\) 따지고 보면 Analog 신호의 일종이긴 함  
+\) 실제로는, high와 low의 변하는 과정에서 Bounce가 발생하고, 이는 결국 AC성분임  
+\) noise도 AC 
GND(Grounbd)는 전자 회로에서 다른 모든 전위에 대하여 기준이 되는 0V 

저항은 회로의 특정 부분에 흐르는 전류의 양 제한  
+) V = IR  
+) 정해진 V에서, R이 클수록 전류 적게  
+) 주파수 영향 없음  
전류가 저항을 지나면 그만큼의 전압이 빠짐. 전위차. potential.  

캐패시터와 인턱터는 주파수를 가진 전압, 전류 입장에서 주파수에 따라 다른 저항 값.  
+) 캐패시터와 인턱터의 전압 자체가 전체 회로에선 저항

캐패시터는 높은 주파수 전압 -> 저항 낮음 (전압의 시간에 따른 변화율이 클수록 전류를 더 잘 통과)  
+) 교류 성분은 통과, 직류 성분 통과 못함, 즉 DC 성분과 AC 성분 분리 능력 가짐
+) dV/dt = I / C 
+) C는 캐패시터 용량, F 패럿, 클 수록 전류 많이 흐름  
+) 전류를 충전했다 방전하는 성질 있음
+) 정해진 V에서, C가 클수록 저항이 작아져 전류많이  
+) 높은 주파수 일수록 저항 작음  
+) 고주파 통과  

인덕터는 높은 주파수 전류 -> 저항 크게  
+) 전류가 변화하지 못하도록 함  
+) V = L dI/dt  
+) 저주파 전류만 통과 가능, 즉 급격한 신호의 흐름 막음  
+) H, 헨리
+) 정해진 V에서, L이 클수록저항이 커져 전류 조금  
+) 높은 주파수 일수록 저항이 큼  
+) 저주파 통과  

모든 신호는 저주파에서 고주파까지의 주파수를 가짐  
필터를 통해 원하는 것만 통과  
LPF (Low Pass Filter), HPF (High Pass Filter), BPF (Band Pass Filter)  
대충 고주파는 교류, 저주파는 직류 

Transistor, 전류의 양 조절
+) Trans-Resistor, 레지트터 값을 변화시킬 수 있다.  
+) B(Base), E(Emitter), C(Collector)  
+) B에 넣어주는 전압에 따라 E에서 C로 흐르는 전류량이 달라짐  
+) B는 B와 E의 전압을 얼마나 세게 주느냐  
+) 활성 영역, 차단 영역, 포화 영역  
+) Switching 기능은 ON/OFF 기능  
+) 증폭 기능으로 Amplifier 가능

Pull up & Pull down은 신회의 기본적인 default level을 유지하게 해주는 역할  
+) ideal world에서는 필요 없지만 real world에서는 외부요인의 변수가 있어 이러한 유지를 위한 역할을 하는 것이 필요함  
Low Active Pull Up  
+) Low 되었을 대만 Active  
+) 평소 Default 값을 High로 유지  
+) /,_N, -, * 등으로 표기  
High Active Pull Down  
+) High 되었을 대만 Active  
+) 평소 Default 값을 Low로 유지  
Open Collector: transistor switch가 master chip에 들어되 컬렉터만 회로 외부에 노출되어 있는 형태  
+) 내부에서 E는 GND에 연결  
+) Base는 input으로 inverter가 연결되어 input 상태를 반전시킴
+) Open Collector 출력은 트랜지스터가 ON일 때 GND로 신호를 당기고, OFF일 때 노드를 부동 상태로 둡니다.

Bypass Capacitor / Decoupling Capacitor  
+) Noise, Ripple 성분을 주파수의 특성을 이용하여 GND로 통과시켜버린다/ 전원 공급에서 떼 내어 버린다.  
Chip과 Capacitor를 병렬로 달아 Chip으로 가는 AC를 차단해줌  
+) Capacitor가 AC에 대한 낮은 저항을 보여 AC는 Capacitor로, DC는 Chip으로 전달하게 해줌  
+) I_Chip = I_AC + I_DC
+) 전원의 Nosie 같은 것을 제거해줌  
전원과 Chip의 Input 사이의 L과 C  
+) L을 사용해 AC에 큰 저항으로 제거  
+) C를 사용해 AC를 GND로 떼 냄  

논리회로: 디지털 신로를 input으로 넣을 때, 논리적인 순서로 data를 manupulation해서 output을 만들어 내는 것  
TR을 이용하여 만든 Logic Gate  
Transistor의 두가지 종류 BJT, FET  
BJT(Bipolar Junction Transistor)  
+) 전류 제어 소자  
+) Base에 소량의 전류를 흘려 컬렉터(Collector)와 이미터(Emitter) 사이의 전류 제어  
+) NPN 트랜지스터 : 전류가 C -> E  
+) PNP 트랜지스터 : 전류가 E -> C  
+) Vcc 전원, Vee Ground  
+) BJT로 꾸민 회로를 TTL (transistor-transistor logic)  
+) 고전류 증폭기 Amplifier에 적함  
FET(Field Effect Transistor)  
+) 전압 제어 소자  
+) Gate에 가해지는 전압으로 Source에서 Drain 사이의 전류 제어  
+) MOSFET: 게이트가 절연되어 있고, 게이트의 전압으로 전류를 제어  
+) Vdd 전원, Vss Ground  
+) FET을 이용하여 꾸민 회로를 CMOS  
+) 전력 소모가 낮으나 속도가 느려 스위치로 적합하며 Logic 쪽에 어울림  

IC (Intergrated Circuit)  
삼각형은 edge trigger  
NC는 no connection  

Register: Flip Flop의 집합  
+) Flip Flop은 1 bit의 정보를 저장할 수 있는 회로  
+) n bit  정보 저장이란 n개의 FF  
+) Latch: 0 또는 1을 기억하는 소자로, 대표적인 예가 FF  
+) CPU가 적은 양의 데이터나 처리하는 동안의 중간 결과를 일시적으로 저장하기 위해 사용하는 회로  
+) 메모리 보다 빨라 CPU가 주요 활용  
+) CPU 외부에도 있을 수 있음  
RS FF: NOR gate 두개의 output이 서로를 다시 input  
+) Enable 상태일 때, R=1, S=0 => output 0  
+) Enable 상태일 때, R=0, S=1 => output 0  
+) Disable 상태일 때, R= 0, S =0, output은 유지   
+) 이를 통해 Memory 기능 가짐  
+) Level Trigger Latch  
+) 신호가 high인 동안의 input 반영  
Edge Trigger D Latch는 RS FF 두개를 통해 만듬  
+) clk가 올라가는 순간이나 내려가는 순간에만 write 가능  
Level Trigger Flip Flop을 Latch, Edge Trigger Flip Flop을 그냥 Flip Flop으로 부르는 경향이 있음  
Register란 이러한 latch나 ff를 n개 엮어 만든 것  

Register는 크개 두 종류로 나눌 수 있다  
General Purpose Register  
+) Address Register, Data Register, Instruction Pipeline Register  
Special Purpose Register  
+) Program Counter, Stack Pointer, Linked Register, Status Register  

Clock은 디지털 회로의 심장 박동  
+) 주기적인 전기적 pulse  
+) 회로의 모든 것이 clock에 synchronization (HW- 박자를 맏추다, SW - 순서를 맞추다)  
+) 시스템이 하는 모든 일의 순서와 timing을 정해줌  
+) 빠를 수록 좋은 것은 아님, 회로별 처리 시간이 있음  

BUS는 장치들이 정보 공유를 위해서 송유하는 선들의 집합  

전달 지연 시간: 게이트에서 입력 신호가 들어온 후 출력 신호가 나올 때까지 약간의 시간  
+) time low to high (tTLH), tr (Risigin time): 전압을 기준으로 10% -> 90%  
+) time high to low (tTHL), tf (Falling time): 90% -> 10%  
Timing과 관련된 다양한 Nitation 존재: clock, high to low, transient, high/low to high, bus stable, bus to high impedance, bus change, high impedance to stable bus, heavy line indicates region of interest  






