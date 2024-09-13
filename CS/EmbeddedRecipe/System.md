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

- 















