---
title: "[ASM] 사칙 연산, 시프트 연산, 논리 연산"
excerpt: "어셈블리의 다양한 연산에 대해 알아보기"

categories:
  - Assembly Language
tags:
  - [study, asm, assembly, calculation, arithmetic, shift, logical]

permalink: /asm/calculation/

toc: true
toc_sticky: true

date: 2022-11-13 23:54:30+0900
last_modified_at: 2022-11-13 23:54:32+0900
---

## 👻 헬퍼 매크로
**헬퍼 매크로**는 공식 어셈블리어는 아니고 복잡한 기능을 모아서 간편하게 사용할 수 있도록 만들어둔 것이다. SASM에서 제공해주는 것이라 크게 중요하진 않지만 이번 시간에 각종 연산을 공부하기 위해서는 아래의 매크로들을 알아둘 필요가 있다.

> - **GET_DEC** : 10진수로 숫자를 입력받음   
cf) ``` GET_DEC 1, al ``` : al에서 1바이트만큼의 수를 받아라   
- **PRINT_DEC** : 10진수로 숫자를 출력함   
cf) ``` PRINT_DEC 1, al ``` : al에서 1바이트만큼의 수를 가져와서 10진수로 나타내라   
- **NEWLINE** : 한 줄 띄우기

***

## 👻 사칙 연산
사칙 연산은 말 그대로 **덧셈, 뺄셈, 곱셈, 나눗셈**을 의미한다. 각각 ``` add ```, ``` sub ```, ``` mul ```, ``` div ``` 명령어를 사용한다.

***

### 🌱 덧셈과 뺄셈
``` add ```와 ``` sub ```를 사용하고 입력값 두 개를 뒤에 받는다. 더해진 값은 첫 번째 인수에 저장되며 각각의 인수에 들어와야 할 타입은 정해져있다.

```
add a, b  ; 덧셈
sub a, b  ; 뺄셈
```

> 💡   
- a는 레지스터 or 메모리
- b는 레지스터 or 메모리 or 상수
- **단, a, b 모두 메모리는 될 수 없다.**

- 응용

```
; 레지스터 + 상수
add al, 1
sub al, 1

; 레지스터 + 메모리
add al, [num]
sub al, [num]

; 레지스터 + 레지스터
add al, bl
sub al, bl

; 메모리 + 레지스터
add [num], al
sub [num], al

; 메모리 + 상수
add [num], byte 1
sub [num], byte 1

; 메모리 + 메모리는 올 수 없음!
```

여기서 주의해서 봐야 할 조합은 모두 **메모리**와 관련된 식들이다.   

우선 메모리는 이름만 쓰게되면 메모리의 값을 나타내는 게 아닌 **주소값**을 나타내게 되는데, 그 상태로 연산을 하게되면 에러가 나게된다. 그래서 ``` 대괄호([]) ```로 감싸주어서 주소값이 아닌 데이터 그 자체를 피연산자로 가져와야한다.

메모리와 상수 연산 후, 메모리에 저장시킬 때에는 **상수의 크기를 반드시 지정**해주어야 한다. 메모리의 영역을 얼마큼 지정하여 사용할지 정해주지 않으면 에러가 나게된다.

***

### 🌱 곱셈과 나눗셈
곱셈과 나눗셈은 덧셈과 뺄셈보다 회로도가 더 복잡하고 생각보다 구현하기 어렵다.

```
mul reg ; 곱셈
div reg ; 나눗셈
```

> 💡   
- **mul bl** 👉 **al * bl**
    - 연산 결과를 **ax**에 저장한다.
- **mul bx** 👉 **ax * bx**
    - 연산 결과를 **dx(상위 16비트), ax(하위 16비트)**에 저장한다.
- a 레지스터는 기본적으로 사용이 고정되어있다. 아마 결과값을 받는 용도인 듯 하다.
- **div bl** 👉 **ax / bl**
    - 연산 결과를 **al(몫), ah(나머지)**에 저장한다.


- 곱셈 응용(1바이트)   

```
; 5 * 8 = ?

mov ax, 0
mov al, 5
mov bl, 8
mul bl
PRINT_DEC 2, ax
NEWLINE
```

- 결과   
![Alt Text](/assets/images/posts_img/basics/asm/calculation/mul-58.PNG)   

2바이트 연산이 잘 이해가 가지 않아 하나 더 해보았다.

- 곱셈 응용(2바이트)

```
; 24000 * 628 = ?

mov dx, 0
mov ax, 24000
mov bx, 628
mul bx
PRINT_DEC 2, dx
NEWLINE
PRINT_DEC 2, ax
```

- 결과   
![Alt Text](/assets/images/posts_img/basics/asm/calculation/mul-result2.PNG)   

- 이유   
![Alt Text](/assets/images/posts_img/basics/asm/calculation/mul-result.PNG)   

상위 16비트를 10진수로 변환하면 229(0b 0000 0000 1110 0101)이고, 하위 16비트를 10진수로 변환하면 -1280(0b 1111 1011 0000 0000)이다. ~~신기하네...~~

![Alt Text](/assets/images/posts_img/basics/asm/calculation/save-dx.PNG)   

![Alt Text](/assets/images/posts_img/basics/asm/calculation/save-ax.PNG)   

- 나눗셈 응용

```
; 100 / 3 = ?

mov ax, 100
mov bl, 3
div bl
PRINT_DEC 1, al ; 몫
NEWLINE
mov cl, ah
PRINT_DEC 1, cl

; PRINT_DEC 1, ah ; ah는 PRINT_DEC로 출력 불가
```

- 결과   
![Alt Text](/assets/images/posts_img/basics/asm/calculation/div-result.PNG)   
  
***

## 👻 시프트 연산
왼쪽 혹은 오른쪽으로 1비트씩 자리이동을 해주는 연산이다. 좌측 시프트, 우측 시프트는 각각 ``` shl ```, ``` shr ``` 명령어를 사용한다.

```
shl eax, 8  ; 왼쪽으로 8칸 이동
shr eax, 8  ; 오른쪽으로 8칸 이동
```

> - 좌측 시프트의 경우 최상위 8비트는 분실되고 최하위 비트가 0으로 채워진다.
- 우측 시프트의 경우 최하위 8비트는 분실되고 최상위 비트는 0으로 채워진다.
- **단, 최상위 비트가 부호를 나타내고 있을 경우 부호 비트는 1로써 고정된다.**
    - 부호 비트 고정은 **우측 시프트**만 적용된다.

> 💡 **시프트 연산을 하는 이유?**   
- 좌측 시프트는 2배, 우측 시프트는 나누기 2의 값이 나오므로 곱셈, 나눗셈 연산을 진행하는 것보다 편리하다.
- 게임 서버에서 ObjectID를 만들어줄 때 각 다양한 값을 ID에 넣을 수 있다.   
👉 **BitFlag**

***

## 👻 논리 연산
두 가지 이상의 조건을 판별할 때 주로 논리 연산을 사용한다. ``` not ```, ``` and ```, ``` or ```, ``` xor ``` 등이 있다.

```
not al        ; 0이면 1, 1이면 0
and al, bl    ; 둘 다 1이면 1, 아니면 0
or al, bl     ; 둘 중 하나라도 1이면 1, 아니면 0
xor al, bl    ; 둘의 값이 다르면 1, 같으면 0
```

두개의 숫자로도 논리 연산이 가능한데, 이를 함으로써 원하는 정보를 추출해낼 수 있다.

> _[여기서](/cpp/calculation/#-비트-플래그bitflag)_ 비트 플래그에 관해 더 자세하게 알 수 있다.

***

## 👻 글을 마치며
이번 시간에는 각 연산을 어셈블리어로 익혀볼 수 있었다. 문법만 약간 달라졌지 계산 방식은 다 비슷했다. 아예 레지스터 자체로 계산을 하다보니 곱셈, 나눗셈이 약간 어렵게 느껴졌는데 계산 결과를 저장하는 곳이 고정되어있다는 사실만 알면 나름의 규칙이 보여서 이해할 수 있었다. 이번 포스팅을 함으로써 기초란 기초는 완전 탄탄히 다진 것 같다. ~~연산만 세번째.. 네번짼가..~~ 그래도 잊을만 할 때 어셈블리 복습을 하니 각성 효과도 있고 잘 새겨진 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_assembly/blob/main/calculation.asm)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   