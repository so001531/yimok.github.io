---
layout: post
title: "[Cortex-M4]마이크로 프로세서 기초지식"
description: "[Cortex-M4]마이크로 프로세서 기초지식" 
modified: 2016-09-25
tags: [blog]
comments: true
image:
  feature: back1.jpg
---

Arm 마이크로 프로세서 종류와 , 클락, 레지스터, RCC 에 대해서 알아보자
 

<section id="table-of-contents" class="toc">
  <header>
    <h3>Overview</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->



## ARM 프로세서

### 소개
<figure>
	<p style="text-align: center;">	
		<img src="/images/arm.PNG" alt="">
	</p>
</figure>

- ARM 이란 프로세서의 코어를 설계하는 회사이다 즉 아키텍처를 설계하는 회사이다 
- 이러한 코어 아키텍처를 삼성, LG 등과같은 기업이 사서 ARM 프로세서를 제작한다.
- 클래식 코어버전 : ARM 7 -> ARM 9 -> ARM 11
- ARM 의 버전은 2씩 올라가며 홀수 를 사용한다. 짝수 ARM 버전은 ARM 직원들이 사용하는 버전이며 시판되기전에 버그를 잡는 용도로 사용된다.
- 최근에는 ARM Cortex-M , Cortex-R, Cortex-A 버전으로 버전업이되어 출시되었다. 클래식 버전보다 성능이 많이 뛰어나 cortex(피질) 라는 단어를 붙이게 되었다.
- M버전(저전력)은 자동차에 사용되며, R버전은 영상처리 카메라, A버전(고성능)은 스마트폰에 사용된다.
 
## ARM-Cortex-M4 

### STM32F4

- 실제 사용할 보드는 STM32F4 라는 보드이며 과거 STM 사에서 제작한 32bit 보드로 라이센스를 팔았지만 이름 그대로 STM을 사용하고 있다.
- STM32F4 ARM-cortex-M4 프로세서를 사용한다. 최대클럭 168MHZ, DSP, 32bit의 특징을 가지고 있다.
- 마이프로세서에서 가장 중요한 것은 +와 GND를 물려서 밥을 주고, 타이머를 동작시켜서 레지스터를 사용하여 데이터를 이동시키는 것이 마이크로 프로세서에서 하는 가장 중요한 동작이다.
- 마이크로프로세서는 시간, 즉 펄스(Pulse) 에 따라 일을 한다.
 

### 주파수

- 주파수란 1초에 몇번을 진동하느냐를를 말한다.
- 4Hz는 1초에 4번을 진동 -> 1/4(s) -> 0.25(s) -> 250ms 
- 10Hz는 1초에 10번 진동-> 1/10(s) -> 0.1(s) -> 100ms 
- Hz가 높을수록 시간은 낮아진다. 반대로 Hz가 낮을수록 시간은 증가한다.

<figure>
<p style="text-align: center;">	
	<img src="/images/pulse.png" width="400">
</p>
</figure>

- 위그림은 5Hz 주파수와 7Hz 주파수를 보여준다.
- 마이크로프로세서는 엣지에서 동작을한다. 엣지는 Falling edge 와 Rising edge 가 있으며 Falling으로 할지 Rising으로 할지는 설정이 가능하다.
- 일반적으로 마이크로프로세서는 Falling edge를 사용한다.

### 클럭(Clock)

- 마이크로 프로세서에서 시간을 가한다라는 말은 = 동작속도를 설정한다는 말이다.
- 클럭 이 작으면 전력소모가 낮아지고, 클럭이 크면 속도가 증가한다, 따라서 사용자에 알맞게 클럭을 동작시켜야한다. 이때 사용자에게 알맞게라는 것은 사용자가 느끼기에 배터리 소모속도도 적당하고 디바이스의 속도도 빠른것을 말한다.
- STM32F4의 최대 클럭은 168MHz이지만 위와 같은 이유로 클럭을 분할해서 알맞게 사용한다. 
- 분할을 할때 마이크로 프로세서 옆에붙은 오실레이터 or 크리스탈 이 클럭의 분할을 도와준다.

<figure>
<p style="text-align: center;">	
	<img src="/images/stm32f4.PNG" alt="">
</p>
</figure>

- 위사진에서 보면 보드 중앙의 stm32f4 마이크로프로세서 위에 쇠뭉치같은게 붙어있는데 이것이 오실레이터 또는 크리스탈이다.

### 레지스터(Register)

<figure>
	<img src="/images/register.png" alt="">
</figure>

- 레지스터는 쉽게말해 기억장치이다. 다수의 플립플롭으로 구성된 메모리 이다.
- 왜 레지스터라는 명칭을 쓸까? 바로 CPU 내에있는 메모리로 CPU에서 가장 가까운 위치에 있기때문이다. 실제로 이 레지스터는 버스를 타지 않기때문에 빠른연산이 가능하다.
- 레지스터는 빠른속도를 같는만큼 전력소모는 증가한다.
- 메모리인 레지스터는 사용자의 명령을 받고 , 명령에 따른 답을 받아서 사용자에게 주는 인터페이스 역할을한다.
- 레지스터에 명령을 내리면 레지스터가 알아서 프로세서1, 프로세서2, ... 동작가능한 프로세서를 선택하여 활성화시키고 명령을 처리한뒤 완료된 결과를 다시 레지스터에 놓는다. 그러면 유저는 레지스터에온 결과를 확인한다.
 
### AMBA(Advanced Microcontroller Bus Architecture)

- 예전에는 마이크로 프로세서의 버스구조가 체계화 되어있지 않았다.
- 최근에 체계적이고 잘 구조화된 버스를 도입하였는데 그것이 바로 AMBA 이다.
- Advaced High-Performance Bus(AHB)
- Advaced Peripharal Bus(APB)
- 위 2가지 버스이외에 2가지 버스가 더있지만 가장많이 쓰이는 버스가 위의 2가지 버스이다.

### STM32F4의 메모리

- GPIO를 쓰려면 레지스터를 통해 GPIO를 초기화하고 사용해야한다. 초기화를 하려면 그 주소로 가서 Enable(1) 을 시켜 주어야한다.
- 마이크로프로세서의 GPIO, DAC, ADC 등.. 여러 기능들인 메모리에 각각 배정되어있다.(자세히 보려면 메모리맵을 살펴보자)
- 메모리상에서 GPIOF 의 주소가 0x40021400 ~ 0x400217FF 일때 실제로 GPIOF 포트 활성화를 어떻게 시키는지 C코드로 간단히 구현해보자

```c

	int *a = (int *)0x40021402 // 명시적 형변환으로 int * 형으로 변환하여 주소를 포인터변수 a에 저장
		*a = 1; //Enable(1) 주소로 가서 값을 1로 변경
		*a = 0; //Disable(0) 주소로 가서 값을 0으 변경
	
```

### RCC(Runtime Clock Control)

- RCC란 클럭을 사용할때 첫번째로 사용하는 레지스터를 말한다.
- 클럭을 초기화 시킬때는 비트를 사용하여 초기화를 한다.
- RCC 역시 메모리상에 있으며 아래 그림과같이 32bit로 블록화 되어있다.

<figure>
	<p style="text-align: center;">	
		<img src="/images/rcc1.png" alt="">
	</p>
</figure>

- 가장낮은 비트를 LSB , 가장 높은 비트를 MSB 라고 부른다. 이 32개의 bit를 이용하여 RCC를 세팅한다.
- RCC의 비트맵을 보면 클럭을 2개 보유하고있다(MCO1,MCO2)
- MCO2 는 2bit로 동작되며 각 bit에 따른 상태는 다음과같다 00(SYSCLK) 01(PLLI2S CLK) 10(HSE oscillator CLK) 11(PLL clock) 실제로 많이사용되는 클럭은 시스템클럭이며 나머지 3개의클럭은 외부클럭이다.
- MCO2PRE 는 클럭을 나눠쓸때 사용된다 즉 Division 용도로 쓰인다. 0xx(no division) 100(2) 110(4) 111(5)  각 비트설정에 따라 나누지않거나, 2, 4, 5로 나눈다.
- 그럼 division 5 로 클럭2 SYSCLK 을 활성화 해보자.

<figure>
	<p style="text-align: center;">	
		<img src="/images/rcc2.png" alt="">
	</p>
</figure>

- 위 그림과같이 주소를 4bit씩 묶어서 16진수로 주소의 한자리를 표현한다.
- 따라서 SYSCLK(00) 비트를 MCO2에 설정해주고 , division 5(111) 을 MCO2 PRE 에 설정해 준다.
- 결과값은 0x38000000 이 된다.
- 이를 C코드로 구현해 보자 RCC의 주소는 0x10000 이라 가정

```c

int *a = (int*a) 0x10000  // RCC의 블럭주소를 찿아간다.
	*a = 0x38000000  //설정할 값으로 세팅한다.

```


Reference: Sang Youn Kim - Micro processor , Korea university of technology & education
