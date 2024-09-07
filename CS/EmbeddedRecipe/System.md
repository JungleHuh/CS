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
  - 플립플롭: 1bit의 정보를 저장할 수 있는 기억소자
  - 플립플롭의 하나인 RS(Reset-Set) 플립플롭은 NOR gate 2개의 Output을 서로의 Input으로 교차해 피드백하는 구조(이해안됨...2024-09-07)
  - 핵심은 플립플롭이 1bit의 정보를 저장할 수 있다는 것
  - Write 신호를 인가해서 플립플롭을 제어하는 것을 Level Trigger Flip Flop/ Latch라고 함
  - Clock신호를 인가해서 CPU 동작 타이밍에 맞춰 제어하는 것을 Edge Trigger Flip Flop이라고 부름
  - 16bit 레지스터는 플립플롭이 16개 엮인 것을 의미
 
  #### 2-2. 레지스터의 종류
  - General Purpose Register
  - -  
