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
Last: 2025 02 03
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

Memory: System 구성과 Performance에 가장 큰 영향을 행사  
RAM: 휘발성, Random Access(byte 단위 접근) 가능; SRAM, PSRAM, DRAM (SDRAM, DDR SDRAM)  
+) DRAM은 일정 시간마자 전하를 충전해 주어야함  
+) SDRAM:  synchronous dram
+) DDR: double data rate, system bus의 rising edege와 falling edge 에서 모두 동작 가능  
ROM: 비휘발성, Flash Memory, 지우고 다시 쓸 수 있음; NOR Memory, Random Access 가능; NAND Memory, 큰 단위 접근  
+) NOR는 코드 저장용, NAND는 데이터 저장용으로 많이 활용됨        
XIP (eXecution-In-Place): software를 메모리에 로드하여 실행하는 대신 플래시에서 직접 수행하는 기술  
+) flash에 용례를 국한하기도 하지만, 모든 메모리 소자에 사용되기도 함  
+) word 단위 access 가 가능하며 software를 execution 하는 것  
+) NAND는 불가능  

RAM Memory는 Address Pin과 Data Pin, 그리고 RD WR pin으로 구성  
+) Address Pin으로 접근할 Address 선정, RD/WR Pin으로 동작 결정, Data Pin으로 쓸/읽을 데이터 결정  
+) 1 byte 당 1 address 차지  
+) Address bus Width가 16 bit이라면, 2^16 * 1 byte으로 64KB size의 RAM  

CPU는 여러가지 일을 해낼 수 있는 논리회로 집합  
+) 프로그램의 명령어를 처리하기 위한 논리회로의 집합  
+) CPU에 나와있는 pin들에 약속된 신호를 받아 일을 수행  
+) 하나의 기계어 명령을 수행하기 위해서는 여러개의 마이크로 명령이 수행  
+) 마이크로 명령어는 CPU내의 각종 게이트를 구동하는 비트신호로 CPU 내 제어 기억 장치에 저장  
CPU 내부에는 여러 장치가 존재  
+) 제어 장치 CU: 각종 제어 신호 발생, Decoder에서 받아온 것을 각종 제어 신호로 변환  
+) Decoder: 명령어를 읽어 해석, IR에서 가져온 intruction을 해석해서 CU에 넘김    
+) 연산장치 ALU: 산술 연산을 담당  
+) 저장공간 Register  
+) Program Counter: CPU가 실행하는 intruction 주소    
+) Instruction Register (IR): Intruction 주소에서 읽어온 instruction을 담음  
+) Address Register:현재 사용하는 Data를 Access 하기 위한 Data의 주소  
+) Data Register: Address Register가 가리키는 주소의 실제값  
CPU외 여러 기능을 덧붙여 한 개의 chip으로 만든 것이 MCU(Moro Controller Unit)  
CPU의 step (stage)
+) Fetch: intruction을 메모리로부터 가지고옴, PC -> Address register -> External Memory -> IR-> PC update  
+) Decode: 가지고온 intruction을 해석하고 Register 값 확인, IR -> Decoder
+) Execution: intruction 실행, CU -> External Memory -> Data Register -> ALU -> ACC, CU -> ALU -> ACC -> External Memory  
Pipeline: 각 Stage가 서로 다른 순차 intruction을 실행  
+) PC 값은 fetch하고 있는 곳을 가르키기에 현재 Execution 하는 것보다 앞섬  

ARM Processor 다룰 내용의 기본 특징: 32-bit RISC, Big/Little Endian 지원, Fast Interrupt Response  
+) 주요 개념: Mode, Register, Exception  
+) MCU의 CPU 부분인 core만 의미함  
Micro Processor는 CISC(Complex Instrcution Set Cimputer)와 RISC(Reduced Instruction Set Computer)로 나뉨  
+) CISC는 많은 수의 명령어와 데이터 형태, 그리고 Addressiong 기법들을 모두 채택하여 Chip의 크기가 크며, 명령어가 복잡하고, Chip이 복잡  
+) RISC는 많이 사용되는 명령어, 데이터 형태, Addressing 기법등만을 모아 단순하게 만듬  
+) RISC는 명령어 길이가 고정, 종류가 많지 않고, 적은 수의 Addressiong 기법으로 chip의 복잡도도 단순하고 크기도 작아지고 ,전력소비도 작음  
+) CISC에서 한줄이면 해결될 일등리 RISC에서는 여러 줄을 거치기도 함, 즉 Performance가 떨어질 수 있음  

ARM mode: 32 bit 동작 (bus line)  
Thumb mode: 16 bit 동작 모드 (bus line)  

ARM 동작 모드, 사용자 취향에 맞게 쓰라고 되어있음  
+) User mode만 normal mode이고 나머진 privileged mode임  
+) User Mode: normal program exectution mode  
+) System model: Run priviledged operating sstem tasks  
+) FIQ: When a high priority (fast) interrupt is raised  
+) IRQ: Wheb a low priority (normal) interrupt is raised  
+) Supervisor: A proitected mode ofr the operating system, entered when a SWI insturciton is executed  
+) Abort: Used to handle memotry access violations  
+) Undef: Used to handle undefined instructions  
+) privileged mode는 스스로 mode 변경이 가능하며 interrupt의 사용 가능 유미를 직접 설정 가능  

Register는 Core가 사용할 수 있는 가장 빠른 저장 매체  
+) ARM의 동작은 모두 register들을 어떻게 장난칠 것이냐임  
동작 모드가 바뀌면 Register Set도 바뀜  
총 37개  
+) R0~R15 (16개)  
+) CPSR (1개)  
+) FIQ의 R8~R14, SPSR (8개)  
+) SVC의 R13 R14 SPSR (3개)  
+) Abort의 R13 R14 SPSR (3개)  
+) IRQ의 R13 R14 SPSR (3개)  
+) UND의 R13 R14 SPSR (3개)  
CPSR (Current Program Status Register)  
+) Flag field: NZCV, 방금 처리된 연산 결과의 상태  
+) IF field: IRQ, FIQ의 Enable, Disable, Interrupt가 걸리지 않도록 control  
+) T field: Thumb mode or ARM mode  
+) Mode field: SVC, UND, ABT 등 CPU 상태 나타냄, 원하는 mode로 세팅하면 그 모드로 전환  
SPSR (Saved Program Status Register)  
+) CPSR을 복사해 넣는 register (Backup), 모드가 바뀌고 다시 돌아오기 위해  
R14 Linked Register: 어디서 branch 해 왔는지 표기  
R13 Stack Pointer: 현재 stack이 어디까지 쌓였는지  
R15 Program counter: 현재 어디를 수행하고 있는건지, fetch해온 위치  
R0~R12는 General Register, R13~R15 + CPSR + SPSR은 Special Purpose Register  
Context: Register Set의 Snap Shot, 현재 Register 값들의 상황  

ARM Exception: hardware적으로 특정 mode로의 진입 유발자  
+) interrupt는 excption의 한 종류  
+) Exception이라는 사건을 통해 hardware 적으로 정해진 reaction이 발생  
+) Reaction은, exception이 발생하면, 진행하던 동작을 멈추고, exception에 해당하는 mode에 진입하고, exception이 물려있는 주소로 pc를 jump시킨 뒤 처리하는 형태  
+) SVC mode는 ARM에 전원이 인가(power on)되거나 reset되면 해당 주소로 jump  
+) IRQ mode는 hardware적인 interrupt가 발생하여 ARM Core에 알려주면 해당 주소로 jump  
+) FIQ mode는 interrupt 중 fast interrupt 가 발생하면 해당 주소로 jump  
+) ABORT mode는 data abort(access 하려는 주소가 불가한 주소, memory faults), prefetch abort(intruction fetch를 해오려는 데 못해온 경우) 같은 경우 해당 주로소 jump  
+) UNDEF mode의 경우 undef exception(unstruction을 decode 했는데 ARM이 모르는 경우)이 났을 경우 jump  
+) 특정 주소로 PC를 Jump한다는 것은, pc 값을 그 값으로 setting한다는 의미  
Exception Vector Table: exception이 났을 때 jump하는 address를 모은 것  
+) exception vector: excption이 발생하면 하드웨어가 수행시키는 미리 정해진 address  
CPSR register 값이 어떻게 setting 되냐에 따라 mode가 구분됨  
+) 이 값에 따라 사용되는 Register set과 stack이 바뀜  
Exception이ㅔ도 우선 순위가 있음  
Exception이 발생하면...  
+) 이전 모드로 돌아가기 위한 세팅 진행  
+) CPSR을 변경될 모드의 SPSR에 저장  
+) CPSR을 변경될 모드의 값으로 변경 및 실제 mod 변경과 register 변경  
+) ARM mode로 변경: exception은 무존건 ARM mode  
+) R14를 현재 PC (돌아갈 PC로 설정)  
+) PC를 reaction 주소로 변경  
+) R0~R12를 R13이 가리키는 stack에 저장  
+) SW적으로 reaction 수행 후 돌아가는 (return) 같은 명령어로 돌아갈 수 있게 해줘야함
Excption이 종료되면...  
+) CPSR에 SPSR에 있는 값으로 변경 (이전 mode로 돌아감)  
+) stack에 저장된 register 값들 복원  
+) PC에 저장되어 있던 R14의 값 복원  

ARM Procedure Call Standard (PSC): ARM 내부의 Register 사용법 총칭  
+) 여러 버전이 존재함  
R0~R3: Argument / Result / Scratch  
+) Argument: 함수를 호출할 때의 Argument가 저장 됨, 순서대로  
+) Result: 함수가 return 될 때의 결과들이 저장되며, 기본적으로 R0가 사용되나 크기가 크면 다른 것 까지  
+) Scratch: 함수가 호출된 후에는 규칙 내에서 자유롭게 쓰일 수 있다는 의미  
R4~R11: variable 용  
+) 로컬 변수에서 활용  
+) 호출 당한 함수는 이전 함수의 값을 유지해주어야 하기 때문에 stack에 저장 후 사용하고 끝나기 전에 복원해 줘야함  
R12~R15: 특수 register들  
+) R12: ARM-Rhumb interworking, long branch등에 사용  
+) R13: SP, Stack Pointer  
+) R14: LP, Link Register, 돌아올 주소를 넣는 목적  
+) R15: PC, Program Counter, 현재 실행하고 있는 주소 ( Fetch 하고 있는 주소 )  

Interrupt: 전기 신호를 통한 끼어들기  
+) 그렇게 큰 일이 아니면서도, 내가 그일을 먼저 하게 만들어서 그 일 먼저 처리하고, 원해 하던 일을 계속 하게 만듬  
+) ARM은 Interrupt가 걸ㄹ미ㅕㄴ exception vector 중 IRQ나 FIQ vector를 실행하고, 해당 vector가 실제 handler로 branch하기 위한 code가 존재  
+) IRQ, FIQ Handler  
+) Interrupt Service Routine (ISR): 각 intrrupt 별 응답  
+) MCU안에 Interrupt Controller가 존재하며, 외부나 내부 pin과 control bus연결을 통해 전달  
+) CPU interface에서는 nIRQ나 nFIQ로 연결  
+) interrupt는 system이 하던 일 중간에 짬내서 처리하는 것으로, 짧고 간결하게 handle하는 것이 좋음  

ARM SoC: System on Chip  
+) ARM은 cpu architecture를 파는 회사  
+) 다른 회사에서 이 architecture를 기반으로 MCU (Micro controller unit) 또는 MPU (Micro processor unit)을 만들어 business  
+) SoC: core에 여러가지 기능을 덧붙여서 한개의 chip을 만든 것  
+) SoC 내부에 각 기능별로 된 block을 IP(intellectual property)라고 명명  
+) AMBA protocal: ARM core와 다른 IP끼리 interaction에 대한 intecommunication을 위한 bus protocol  

AMBA (Advanced Microcontroller Bus Architecture): Bus들을 어떻게 연결하고, IP끼리 서로 어떻게 통신할 것이냐를 약속한 것  
+) SoC Target In-chip bus protocol  
+) ARM에서 주도  
Bus Interface: Bus 위에 Data를 어떻게 전송할거냐, 어떻게 받을 거냐를 잘 Control 해주는 Interface  
+) AMBA에서는 3가지 종류
+) AHB(Advanced High Performce Bus): 빠른속도, Processor, RAM, NAND등 Burst Mode의 Data 전송에 이용  
+) ASB(Advanced System Bus): APB보다 이전에 쓰이던 것으로, 주소,제어,데이터 라인이 모두 분리 및 양방형, 주소와 데이터 번갈아 쏴 burst 어려움  
+) APB(Advanced Peripheral Bus): 나름 빠르지 않은 전송 속도를 필요로하는 주변장치, Multiplex Bus 기반 (주소라인, 제어라인, 데이터 라인 모두 공유하고 주소 쏜 후 데이터 쏴주는 Burst 데이터 전송 기법 활용)  
+) 이들간의 연결이 필요하면 BRIDGE 활용: 속도가 다른 bus끼리 연결  
Master: Slave에게 Read나 Write 요청, Data를 Bus에 흘리고자 하는 주체  
Slave: 요청을 받아 실행, 성공/실패/wait을 master에게 report  
Arbter: bus를 누가 쓸 건지에 대한 결정권 가짐  
Device Control 순서  
+) Master가 뭔가 하고 싶을 때 Arbiter에게 Bus 사용 요청 (HBUSREQ)  
+) 사용 가능하면 HGRANT 신호를 통해 허가  
+) Master가 사용 의미로 HLOCKx 신호를 Arbiter에 전송 (x는 0~15가 가능한 Master의 번호)  
+) 허가를 얻은 Master는 접근하고자 하는 Slave 주소를 (HADDR)을 날림  
+) Decoder는 그것을 보고 HSELx 신호를 통해 Slave에 날려 활성화 (x는 0~15가 가능한 Slave의 번호)  
+) Master에서 write/read를 수행하겠다는 의미로 HWRITE 신호를 High/Low하게 slave에 날림  
+) 활성화된 Slave는 준비가 되었다는 의미로 HREADY 신호 날림  
+) Write면 Master가 HWDATA선에다 Data를 날림, Read면 Slave가 HRDATA에 data를 날림  
Master와 Slave 사이에 HTRANS, HBURST등의 여러 control pin 존재  

Little Endian과 Big Endian: processor가 memory에 데이터를 저장하는 방식  
+) Little Endian은 상위 bit (MSB)를 상위 주소 (더 큰 값) 에 저장  
+) Big Endian은 상위 bit을 하위 주소(낮은 값)에 저장  
+) bit endian이 사람이 읽기 편한 순서  

Compile: 프로세서가 해석할 수 있는 명령어를 만드는 것  
+) 기계어(Native Code, 명령어): 프로세서가 해석할 수 있도록 약속된 특정한 bit pattern  
+) 프로그램: 일련의 기계어 명령의 순차적 집합  
+) Assembly: 기계어와 1:1로 matching이 된 사람이 보기 쉬운 푯기 체제  
+) Assembler: compile의 최초 태동으로, assembly를 기계어로 바꾸어 주는 것, 특정 프로세서에만 통함(프로세서마다 다른 assembler 지원, compatibility 호화성이 떨어짐)   
+) High Level Language compiler: High level lanuage로 coding을 하고, 동작하고자 하는 processor 맞는 compiler를 통해 그 프로세서에 동작 가능한 assembly를 generation 하는 것  
+) C 코딩 -> C compiler -> Arm Assembly -> ARM Assembler -> Bit Pattern  
+) Executable binary image: 이러한 bit battern을 한 덩어리로 뭉쳐 놓은 것  
Cross Compile: 실제 target에서 돌아갈 binary imange를 PC 상에서 comppile 할 수 있게 해주는 환경  
+) 일반적으로 PC에서 컴파일 한 것은 PC에서 실행되는 것이 정상  
+) embeeded system에서는 target에서 컴파일 수행에 어려움이 있어, 이런 환경에서 binary image를 PC 상에서 만듬  

대략적 컴파일 과정  
+) Source file (.c .s)를 C compiler (armcc, tcc)를 이용하여 Assembly로 만들고 (.s), Assembler(armasm)를 이용하여 실행 가능하고 symbol 정보를 가진 elf object 파일 (Elf - Excutable and linkable format, .o)으로 만든 뒤, 이 object file을 linker (armlink)가 엮어 실행 파일인 .elf을 생성함. 이 실행 파일을 실행하면 그 안에 있는 Native Code (기계어)인 binary가 추출되어 메모리에 로드되고, 프로세서가 이를 실행함  
+) link: 여러 개의 .c file을 Assembly로 만든 후 Assembler를 이용하여 Object라는 새로운 기계어 형태로 만들어내고 이런 object들을 연결 하는 것  
+) object는 elf와 같은 형태로 각기 다른 .c를 컴파일한 object와 연결할 수 있는 정보와 디버깅이 가능한 symbol 정보를 담고 있음  
+) armcc, tcc는 .c를 .s로 만들기 전에 preprocessor 일도 진행 (macro나 define을 compile 전 바꿔치기하거나, syntax error등 점검, 컴파일 하는 단계까지 만들어 두는 일)  
+) lib file: source code를 제공하고 싶지 않은 개발자가 object 형식으로 미리 컴파일하여 제공하고, lbi은 다른 컴파일 된 object들과 link되어 같이 물려 들어가는 형식을 취함  
+) link 시에 scl (scatter loading)이 들어가는데, binary를 만들 때의 구조를 설정하는 script file  
+) map, sym file은 compile된 binary의 메모리 구성을 나타내주는 text file  

spaghetti.h, spaghetti.c를 구현  
preprocess를 하면 #inlcude를 처리하여 한 파일로 만들고 #define 메크로 정리 및 compile error 검사하여 spaghetti.i 생성  
+) #include <>는 compiler predefine path, #include "" 는 .c가 있는 directory  

Library: 미리 컴파일 해 놓은 Object fiole 모음  
+) 자주 사용하는 함수들을 굳이 Compile 할 때마다 새로 할 것이 아니라, 미리 만들어 Link할 때만 연결  
+) source 코드 공개 안해도 됨  
+) compile 된 object file들을 압축해서 한 개의 file로 관리하는 것 
+) libaray를 archive file이라고도 부름  
+) arm에서 archive(file을 압축하는 것)는 armar을 사용  
+) 이것을 활용하여 여러 object 파일을 libaray로 한들거나 하나만 빼거나 하나만 추가하거나 등 할 수 있음  

변수의 scope에 따른 유형  
+) auto: 일반적 local 정의 변수  
+) extern: 일반적 global 정의 변수, 다른 file에서도 extern하면 사용 가능  
+) static: local에 사용 시 생존 기간 (함수 등)이 동료되더라도 그 값을 유지, global에 사용 시 다른 file에서 사용하지 못하게함 (접근성을 선언한 곳으로 국한, local도 마찬가지)  
+) volatile: optimization하지말라는 의미

Symbol: Compier/Linker가 알아볼 수 있는 기본 단위로 link 후 자신만의 주소를 갖게 되는 특별한 단위  
+) 전역변수 함수이름 등  
+) Linker를 위하여 ELF object file내에 symbol table을 만들어둠, source code에 의하여 참조되는 symbol들의 이름가 위치 정보
+) 실제 메모리에 적재되는 내용은 아님, Linker 만이 이 symbol을 참조하며, 실제로 Linker는 이른 symbol들을 모두 주소로 변환해서 binary로 만듬  
Symbol을 필자는 global이라 부름  
+) 함수, 전역변수, static 변수, 등을 포함하며 필자는 global이라 부름 => 다른 파일의 함수들도 직접 access 가능  
+) symbol 같이 자기 자신만의 주소를 가지지 못한느 것은 local  
Symbol (절대 주소를 가지는 변수)을 세가지로 나누고 memory에 자리 잡는 것을 살펴보면,  
+) RW: read write로 초기값이 있는 전역 변수 (.data), 초기값이 존재하고 변화 가능임으로 ROM과 RAM 모두  
+) ZI: zero initialized로 초기값이 0인 전역 변수 (.bss), 변화 가능임으로 RAM에 만 있어도 됨  
+) RO: read only로 수정이 불가능한 const 전역 변수와 text인 code를 의미 (.text, .constdata), 변화 불가로 ROM에만 있으면 됨  
절대 주소가 아니고 그때 그때 사용하고 버리는 변수(temporary)는 
+) local 변수는 stack에 자리 잡음  
+) Dynamic Memory Allocation 변수는 heap에 자리 잡음  

ELF: executable and linking format, 실행 가능한 그리고 링크를 하는 형식  
ELF object file: relocatable 파일이락도 하며, 실행은 불가하지만 link가 가능한 object file, table 형태



