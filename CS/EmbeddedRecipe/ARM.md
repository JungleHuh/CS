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


###### 출처: https://ww1.microchip.com/downloads/en/DeviceDoc/DDI0029G_7TDMI_R3_trm.pdf
