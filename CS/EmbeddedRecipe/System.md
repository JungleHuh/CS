### 1. 논리회로
- 논리회로란 디지털 신호를 Input으로 넣었을 때 원하는 Output을 만들어내기 위해 논리적 순서에 의해 데이터를 Manipulation하는 회로
- ![image](https://github.com/user-attachments/assets/f9f9b932-4c9c-4de3-9a21-5abfd52ff692)
- 논리회로는 트랜지스터의 조합으로 만들어짐

##### IC
- ![image](https://github.com/user-attachments/assets/decd84ca-22f1-4e12-9b5e-469396c6c31d)
- 수많은 논리회로가 합쳐져 하나의 패키지로 만든 칩을 IC라고 부름
- IC의 datasheet를 읽고 Pin번호를 이해할 수 있어야 함
- NC(No Connection)은 사용하지 않는 Pin을 의미
- CLK는 Clock trigger Pin
- GDN(GROUND)
- VCC(전원)
- Data in/out은 I/O를 위한 Pin

### 2. 레지스터

  #### 2-1. 레지스터의 정의와 플립플롭(Flip-Flop)
  - 레지스터는 CPU가 데이터를 처리하는 동안의 중간 결과를 일시적으로 저장하기 위해 사용하는 고속의 저장 공간, 플립플롭의 집합
  - ![image](https://github.com/user-attachments/assets/ddc8ce5b-59d3-4153-be8b-402c06d3fe3d)
  - 플립플롭: 1bit의 정보를 저장할 수 있는 기억소자
  - 플립플롭의 하나인 RS(Reset-Set) 플립플롭은 NOR gate 2개의 Output을 서로의 Input으로 교차해 피드백하는 구조(이해안됨...2024-09-07)
  - 핵심은 플립플롭이 1bit의 정보를 저장할 수 있다는 것
  - Write 신호를 인가해서 플립플롭을 제어하는 것을 Level Trigger Flip Flop/ Latch라고 함
  - Clock신호를 인가해서 CPU 동작 타이밍에 맞춰 제어하는 것을 Edge Trigger Flip Flop이라고 부름
  - 16bit 레지스터는 플립플롭이 16개 엮인 것을 의미

 - GPT 부연설명
 - RS 플립플롭은 두 개의 NOR 게이트를 이용해 구성되며, 이 게이트들은 서로의 출력을 자신의 입력으로 피드백 받는 구조
 - 즉, 하나의 NOR 게이트의 출력은 다른 NOR 게이트의 입력에 영향을 주어 상호 연결된 상태를 유지
 - #### 동작방식
 - 1. Reset 상태 (R = 1, S = 0):
NOR 게이트 1의 출력이 0이고, NOR 게이트 2의 출력이 1이 되며, 플립플롭의 상태는 Q = 0으로 초기화됩니다.

  2. Set 상태 (R = 0, S = 1):
NOR 게이트 1의 출력이 1이고, NOR 게이트 2의 출력이 0이 되어, 플립플롭의 상태는 Q = 1로 설정됩니다.

  3. 유지 상태 (R = 0, S = 0):
이전의 상태를 유지합니다. 입력이 둘 다 0일 때, NOR 게이트의 특성에 따라 이전 상태의 출력이 계속 유지됩니다.

  4. 금지된 상태 (R = 1, S = 1):
출력이 불안정한 상태가 되며, NOR 게이트의 논리 특성상 정상적인 동작이 이루어지지 않습니다. 이 상태는 피해야 합니다.

- 위 경우의 수의 핵심은 플립플롭이 1-bit의 정보를 ‘저장할 수 있는 능력’이 있다는 점이다.
- 좌측 그림처럼 write 신호를 인가해서 플립플롭을 제어하는 걸 ‘level trigger flip flop’ 또는 ‘latch’라고 부르고 우측 - 그림처럼 clock 신호를 인가해서 CPU 동작 타이밍에 맞춰 제어하는 걸 ‘edge trigger flip flop’ 또는 ‘flip flop’이라고 부른다.
- 따라서 16-bit 레지스터는 위와 같은 플립플롭이 16개가 엮인 것을 말한

  #### 2-2. 레지스터의 종류
  - [General Purpose Register]
  - Address Register: 메모리에 Read/Write할 때 데이터가 들어있는 주소를 임시 저장
  - Data Register: 메모리에 Read/Write할 때 쓰려는 값, 또는 읽은 값을 임시 저장
  - Instruction Register: 메모리에서 읽어온 명령어 저장

  - [Special Purpose Register]
  - PC: 현재 실행되고 있는 주소를 가리킴
  - SP: 사용중인 Stack의 최상단 주소를 가리킴
  - LR: 복귀할 주소를 가리킴
  - SR: 시스템/서브시스템의 현재 상태를 저장하고 알려줌

  - [I/O Register]
 

### 3. Clcok과 Timing Diagram
- 디지털 회로는 논리회로글의 집합인 'Combinational 회로'와 플립플롭같은 기억소자들의 집합인 'Sequential 회로'로 구성
- 어느 하나만 사용하는 것은 비효율적임
- EX) 5를 20번 더할 때 adder 논리회로 20개를 연달아 쓰는 것보다 adder 논리회로 1개와 레지스터 1개로 20번 피드백하면서 Loop 돌리는게 더 효율적
- 동기화는 1/박자를 맞추다 2/순서를 맞추다 : 두 가지 뜻을 의미
- Clock은 디지털 시스템 속 심장박동과 같으며 모든 것이 CPU의 Clock에 맞춰 박자도 맞추고, 순서도 맞춰 진행됨
- Clock은 빠르면 좋지만, Input과 Output으로 나가기까지 아주 약간의 Delay가 존재
- 신호가 0에서 1로, 1에서 0으로 변할 때 한 번에 변하는 것이 아님.
- 신호가 스위칭될 때, 완전한 Low, High 신호가 되기까지의 10~90%까지 소요되는 시간을 Delay(전달소요시간)라고 하며, 이에 관한 전기적 특성을 스위칭 특성이라고 함
- 신호가 변하기까지 신호가 소요되므로, CLock 속도는 무작정 빨라질 수 없음
- 또한 회로로 연결되어 있기에, 전체 시스템의 속도는 가장 느린 소자의 속도에 맞춰야 전체 시스템이 오작동 없이 동작한다.

- ![image](https://github.com/user-attachments/assets/08eda820-06c3-4fad-a239-4de3cf1410b0)

### 4. BUS
- 버스란 장치들이 정보 공유를 위해서 공유하는 선들의 집합
- 디지털 신호들이 버스를 통해서 이동한다고 생각하기 보다, 어떤 특정 시점에 버스를 바라봤을 때, 그 시점에서 버스를 점유하고 있는 어떤 장치의 신호만이 보인다고 이해하는게 더 정확함(2024.09.13 : 이해안됨)
- 하나의 버스에는 여러 장치가 연결돼 어떤 시점에 버스를 점유해 사용한다
- 버스는 점유권을 중재하며, 버스 사용권을 결정하는 것이 CPU의 CU(Control Unit) 또는 Arbiter이다.

### 5. 메모리

- ![image](https://github.com/user-attachments/assets/d9bf950b-aca7-4299-a3a6-de90c6de555b)
- 메모리는 RAM(휘발성), ROM(비휘발성)으로 나뉨
- 예전에 ROM에는 EPROM, EEPROM 등이 있었지만, 기술이 발전하면서 임베디드 시스템에 탑재되는 MCP(Multi-Chip-Package)도 PSRAM + NOR 조합, SDRAM + NAND 조합으로 가고 있음
- SRAM(Static RAM)은 1개의 셀이 트랜지스터 6개로 구성된 가장 비싼 RAM
- DRAM(Dynamic RAM)은 1개의 셀이 트랜지스터 1개에 캐패시터 1개로 구성된 값싼 RAM
- DRAM은 SRAM에 비해 회로가 단순하고 집적도가 높아 용량 뻥튀기가 가능함
- 하지만 캐패시터에서 전하가 방전(누설)되며 PL172규격에 따라 일정시간마다 Refresh가 필요하고, Read할 때마다 PreCharge도 필요
- 그렇게 하지 않으면 데이터가 사라진다는 단점이 있음
- DDR SDRAM(Double Data Rate Synchronous DRAM)은 CPU 동작에 박자와 순서를 맞춰가며 CPU성능을 최대한으로 활용하는 DRAM
- 특히 DDR은 Clock의 Rising Edgedhk Falling Edge 둘 다 사용해서 Data를 2배로 빨리 전송할 수 있는 메모리
- PSRAM(Pseudo SRAM)은 SRAM + DRAM 같은 느낌으로, 구조적으로는 DRAM이지만 HW적으로 Refresh + Precharge 회로가 내장되어 있어, 따로 Charge Control을 할 필요없이 SRAM처럼 쓸 수 있는 RAM이다.
- NOR FLASH는 셀이 병렬로 연결되어 있고, Address Line과 Data Line을 모두 갖고 있어 Byte 단위로 Random Access가능
- NOR은 Read가 빠르지만 Write/Erase는 느리다
- NAND Flash는 셀이 String(직렬)로 연결되어 있고, Address Line과 Data Linedl 없어 직적도가 높고 Page단위로만 Read/Write가 가능하다.
- NAND는 Read는 느리지만, Write/Erase가 빠르다.
- XIP(Excute-in-place)는 메모리 상에서 직접 프로그램을 실행할 수 있는 기술을 의미하며, Byte/word 등의 크기를 Random Access가 가능해야 한다. 모든 RAM과 Nor Flash는 이를 충족


### 5.2 RAM의 물리적 동작
-![image](https://github.com/user-attachments/assets/40c397d6-7bb7-4e9f-a5d3-691b59b527c7)

- RAM은 Address Pin(A0 ~ A7)과 Data Pin(D0 ~ D7) 그리고 Control Pin(RD, WR)으로 이루어졌다.
- Data Pin은 8개니, 한번에 8-bit씩 읽을 수 있음.
- Address Pin이 8개니까 0x00부터 0xFF까지를 나타낼 수 있고, 한 주소가 8-bit의 데이터를 가지고 있으니 이 메모리의 크기는 256-Byte다.
- 이 RAM에 데이터를 쓰는 방법: 0xAB번지에 0x7C를 쓴다고 가정
  1. WR Pin에 High 신호를 주고, RD Pin에 Low 신호를 준다
  2. A[7:0] = 0xAB = 10101011, D[7:0] = 0x7C = 01111100 신호를 주면 된다.

- RAM을 읽는 방법: WR Pin에 Low, RD Pin에 High 신호를 준 뒤 주소 지정(RAM을 읽기 모드로 설정하는 단계)해주면 D[7:0]에 Data가 나온다.


##### 간략한 16진수 변환법
- 1. 10진수 -> 16진수 변환: 16으로 나누어 나머지를 구하는 방법 사용


     255 % 16 = 15(몫) -> 나머지 15 -> 15는 16진수로 F
     15 % 16 = 0(몫) -> 나머지 15 -> 15는 16진수로 F
     결과: 0xFF : 255는 16진수로 FF


  2. 16진수 -> 10진수 변환: 각 자리에 16의 거듭제곱을 곱해 더함

     0x2A(16진수) -> 10진수 변환
     2 * 16^1 = 2 * 16 =  32
     A(10) * 16^0 = 10
     32 + 10 = 42(10진수)

  3. 2진수 -> 16진수 변환: 4자리씩 끊어서 변환

     10110111(2진수) -> 16진수

     1. 1011 = B(16진수)
     2. 0111 = 7(16진수)
     결과: 0xB7

  4. 16진수 -> 2진수 변환: 16진수 각 자리를 4자리 2진수로 변환

     0xC5(16진수 ->2진수)
     1. C = 1100 (2진수)
     2. 5 = 0101 (2진수)
     결과: 11000101(2진수)


### 6.CPU

#### 6.1 CPU 구조 및 동작 원리

- ![image](https://github.com/user-attachments/assets/91332cae-3890-4941-8c5e-7231bf831200)
- CPU는 논리회로의 집합체이며, 약속된 신호를 주면 약속된 동작을 수행함
- CPU 내부는 CU(Control Unit), Decoder, ALU(Arithmetic & Logical Unit), Register Set 등으로 구성됨
- CPU의 동작원리: Decoder에서 명령어를 읽고 해석한 뒤 CU로 각종 제어 신호를 발생시켜 ALU 등에게 동작 명령
- 동작하는 과정에서 임시로 빠르게 결과를 저장하고 참조하는 용도로 레지스터 활용
- CPU 이외에 여러 가지 기능(Flash, UART, I/O) 등이 한 개의 Chip에 내장된 것을 MCU(Micro Controller Unit)이라고 부름

#### 6.2 Pipeline

- ARM Core CPU의 동작구조를 단순하게 표현하면 Fetch -> Decode -> Execute의 Cycle로 되어있음










     































