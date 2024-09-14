### 1. ARM7TDMI의 내부 구조

- <img width="554" alt="스크린샷 2024-09-13 오후 2 07 51" src="https://github.com/user-attachments/assets/8b63ff3f-82fb-4dc4-ab26-80df352632e6">
- Register Bank: 32-bit 크기의 레지스터 37개를 통칭
- Barrel Shifter: 1-wor를 입력받아 Shift 또는 Rotate 연산을 수행하는 용도로 사용한다

### 2. ARM Modes

#### 2.1 ARM Mode vs Thumb Mode
- Thumb 모드는 ARM Mode의 반쪽짜리 버전이라고 볼 수 있음. 16-bit Instruction Set이다.
- ARM은 32-bit RISC이므로 32-bit를 1-Word로 동작하는게 최고의 성능을 제공할 수 있는데도 불구하고 Thumb모드를 만든 이유는 '비즈니스' 때문
- ARM이 처음 등장했던 시절에는 임베디드 시스템 시장을 장악하던 메모리가 16-bit data line 위주였기 때문
- 물론 ARM mode로도 당시 유행하던 메모리를 사용할 수 있었지만, 하나의 instruction을 사용하기 위해 매번 2회 fetch하므로 성능면에서 굉장히 낭비였을 것이다. 따라서 thumb mode를 만든 것

#### 2.2 ARM 동작 Modes
- ![image](https://github.com/user-attachments/assets/5e5db890-bd80-45f0-b92f-373904ceec67)
- ARM은 7가지 모드로 동작, 모드의 특성을 잘 이해하면 더 적합한 모드를 선택해 SW를 구성할 수 있음
- ARM은 User Mode와 Privileged mode로 나뉜다.
- Privileged Mode는 내부적으로 6개의 모드로 나뉜다.
  1. Privileged Mode는 서로 간에 Mode 변경이 자유롭지만, User Mode -> Privileged Mode는 불가능하다
  2. Privileged Mode는 IRQ/FIQ 등의 인터럽트 사용 가능 유무를 포함해, 다양한 시스템 기능을 직접 설정할 수 있다.
- ARM의 Default 모드는 SVC(Supervisor)모드다. Boot-up 시에 ARM의 모든 기능을 사용하기 위해서는 SVC모드로 진입해야하기 때문이다.

#### 2.3 ARM 레지스터와 Context
- ![image](https://github.com/user-attachments/assets/0dd128b5-4e66-4db3-96fa-4511cd342a50)
- ARM Core는 총 37개의 레지스터를 가지고 있음
- 동작 Mode가 바뀌면 사용하는 레지스터 Set도 바뀐다
- USR와 SYS는 같은 레지스터를 사용한다. R0 ~ R14까지 15개를 사용한다.
- FIQ는 자신만의 R8 ~ R14 레지스터를 갖고 있다. SPSR까지 합쳐 8개를 사용한다.
- 다른 Mode들과 달리 FIQ만 레지스터를 많이 갖고 있는 이유는 빠른 연산을 위해서다.
- FIQ는 빠르게 인터럽트를 처리해줘야 하기 때문에 레지스터를 많이 가지고 있을수록 빠른 처리가 가능하다
- SVC, ABT, IRQ, UND는 자신만의 R13(SP), R14(LR), SPSR을 사용해 총 12개의 레지스터를 사용한다.
- 마지막으로 PC와 CPSR 레지스터를 합치면 총 37개
- 모든 Mode가 공유하는 레자스터는 PC, SPSR, R0 ~ R7이다.
- CPSR(Current Program Status Register): 현재 상태 저장하는 레지스터, SPSR(Saved PSR): 이전 시스템 상태를 백업하는 레지스터
- Thumb Mode를 사용한다면 R8 ~ R12는 사용하지 않는다
- OS에서 나오는 'Context Switching'의 Context가 Register Set을 의미한다.
  - 프로세스 A를 실행할 때 R0 ~ R14, CPSP값을 Stack과 함께 녠ㄲ에 백업한 뒤에 프로세스B의 R0 ~ R14, CPSR값을 Stack에서 꺼내어 덮어 
  - 프로세스B에서 뭘 하려고 했었는지(미래), 무엇을 하고 있었는지(현재), 무엇을 했는지(과거) 파악 가능
  - 특정 순간의 Context를 저장/복원하면 그 특정 순간으로 돌아갈 수 있다는게 Context Switching이다.
  - 이 때 각 프로세스 또는 함수가 어떤 레스터를 사용해서 작업할지는 컴파일 단게에서 결정된다.(그래야 어떤 레지스터에 스택을 저장할지 알 수 있고, 그것에 맞게 기계에를 만들 수 있다)
  -  컴파일러는 프로세스나 함수가 실행될 때, 어떤 값을 어느 레지스터에 저장할지 미리 결정합니다. 예를 들어, 특정 변수가 R0에 저장된다면, 나중에 이 변수를 사용할 때 항상 R0에서 가져오도록 코드가 생성


### AAPCS (ARM Architecture Procedure Call Standard)
![image](https://github.com/user-attachments/assets/bdd32193-a77e-48d9-a331-ec53cb2c43ce)
- ARM은 각 레지스터의 사용법에 대한 표준을 만들었고, 이 표준을 버전에 따라 PCS, APCS, AAPCS라 부른다
- ARM 컴파일러는 이 표준에 맞춰 기계어를 만든다.
- 1. R0 ~ R3(a1 ~ a4)(Argument, Result, Scratch)
     - R0 ~ R3는 함수에서 Argument(Parameter)로 넘길 때 사용한다.
     - 예를 들어, int foo(int a, int b, int c)라고 함수를 정의하고, foo(10,20,30)이라고 call하면 R0에 10, R1에 20, R2에 30이 들어간다.
     - 이 때  Return값은 보통 R0에 들어가는데, Return 값이 여러 개인 경우 R0 ~ R3를 사용한다.
     - 만일 4개를 초과하는 Argument나 Result가 있다면 어쩔 수 없이 Stack에 넣음
     - R0 ~ R3는 Scratch 레지스터라고 부르기도함(연습장이라는 의미로, CPU가 연습장처럼 연산 중간중간에 임시저장 용도로 사용된다는 의미)
     - 따라서 R0 ~ R3는 Callee(호출된 함수)가 마음대로 수정할 권리를 가지기 때문에 여기에는 Caller(호출하는 함수)가 중요한 정보를 넣어두면 안됨
     - R0 ~ R3는 함수에서 Argument(Parameter)로 넘길 때 사용한다.
     - 예를 들어, int foo(int a, int b, int c)라고 함수를 정의하고, foo(10,20,30)이라고 call하면 R0에 10, R1에 20, R2에 30이 들어간다.
     - 이 때  Return값은 보통 R0에 들어가는데, Return 값이 여러 개인 경우 R0 ~ R3를 사용한다.
     - 만일 4개를 초과하는 Argument나 Result가 있다면 어쩔 수 없이 Stack에 넣음
     - R0 ~ R3는 Scratch 레지스터라고 부르기도함(연습장이라는 의미로, CPU가 연습장처럼 연산 중간중간에 임시저장 용도로 사용된다는 의미)
     - 따라서 R0 ~ R3는 Callee(호출된 함수)가 마음대로 수정할 권리를 가지기 때문에 여기에는 Caller(호출하는 함수)가 중요한 정보를 넣어두면 안됨
     - 만약에 중요한 정보를 넣어두었다면, 복귀될 수 있도록 스택에 넣고 복구하는 추가작업을 따로 해줘야 함
  2. R4 ~ R11 (Variable)
     - Local 변수를 저장하는데 사용됨
     - 한정된 개수를 넘어가게 되면 Stack에 저장한다
  3. R12 ~ R15 (Special Puropose Registers)
     - R12: 특수용도 레지스터
     - ARM과 Thumb 모드 간의 상호작용에서 중요한 역할을 함
     - ARM-Thumb interworking 기능에서 Long branch 또는 Vaneer를 사용할 때 주소의 임시저장소로 활용한다.
     - R12 ~ R15도 임시 저장소로 사용할 수 있긴 하지만, 예기치 못한 동작을 할 수 있고, R13, R14 같은 경우 항상 유효한 SP 또는 복귀주소라고 가정하고 실행하는 OS도 있기 떄문에 권장하지 않는다.
     - 모든 레지스터는 사용하기 전에 기존에 들어있는 값을 Stack에 저장한 뒤 사용해야하고, 서브루틴이 끝난 후 복귀할 때는 다시 Stack에서 꺼내어 복구해줘야한다.

### 2.4 ARM Modes와 Exception









###### 출처: https://ww1.microchip.com/downloads/en/DeviceDoc/DDI0029G_7TDMI_R3_trm.pdf
