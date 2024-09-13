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



###### 출처: https://ww1.microchip.com/downloads/en/DeviceDoc/DDI0029G_7TDMI_R3_trm.pdf
