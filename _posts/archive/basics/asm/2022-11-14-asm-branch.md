---
title: "[ASM] 분기문"
excerpt: "어셈블리어에서 분기문을 나타내는 CMP, Jump 시리즈에 대해 알아보기"

categories:
  - Assembly Language
tags:
  - [study, asm, assembly, branch, if, switch, cmp, jmp]

permalink: /asm/branch/

toc: true
toc_sticky: true

date: 2022-11-14 23:14:36+0900
last_modified_at: 2022-11-14 23:14:38+0900
---

## 👻 분기문
**특정 조건에 따라 코드 흐름을 제어하는 명령어**를 의미한다.

> - 스킬 버튼을 눌렀는가?
  - YES 👉 스킬 사용
- 제한 시간 내에 던전 입장 수락 버튼을 눌렀는가?
  - YES 👉 입장
  - NO 👉 던전 취소

***

### 🌱 CMP
어셈블리에선 ``` CMP(Compare) ```라는 명령어로 분기를 나눈다.

```
CMP dst, src
```

> - Compare destination, source
- dst가 기준이다.
- 비교를 한 결과물은 **Flag Register**에 저장된다.

> 💡 **Flag Register**
결과물에 따라 **특수 목적**으로 저장해주는 레지스터를 의미한다.   
cf) 연산 결과, 비교 결과, 상태 저장 등에 쓰인다.

***

#### 🪐 분기 명령어 (Jump 시리즈)
CMP로 조건을 걸고, 그 결과에 따라 분기를 나뉘어주는 **Jump 시리즈**가 존재한다. 말 그대로 Jump(이동)한다는 의미이고 이동하는 곳은 다른 코드의 위치이다.

> - **JMP** : 무조건 점프
- **JE** : JumpEquals. 같으면 점프
- **JNE** : JumpNotEquals. 다르면 점프
- **JG** : JumpGreater. dst가 src보다 크면 점프
- **JGE** : JumpGreaterEquals. dst가 src보다 크거나 같으면 점프
- **JL** : JumpLess. dst가 src보다 작으면 점프
- **JLE** : JumpLessEquals. dst가 src보다 작거나 같으면 점프

```
; ex) 두 숫자가 같으면 1, 아니면 0을 출력하는 프로그램
    mov rax, 10
    mov rbx, 20
    
    cmp rax, rbx
    
    je LABEL_EQUAL
    
    mov rcx, 0
    
    jmp LABEL_EQUAL_END
    
LABEL_EQUAL:
    mov rcx, 1
    
LABEL_EQUAL_END:
    PRINT_HEX 1, rcx
    NEWLINE
```

두 개의 값이 같지 않으므로 ``` LABEL_EQUAL ```로 점프하지 않고 ``` mov rcx, 0 ```이 실행된 후 ``` LABEL_EQUAL_END ```로 무조건 점프를 하게된다. 결과값은 **0**이 나온다.

> 💡 **LABEL**   
코드를 묶어주는 기능을 한다. 어셈블리에서만 사용되며 결과물이 나올 때는 없어진다.

***

### 🌱 연습 문제
> 어떤 숫자(1~100)가 짝수면 1, 홀수면 0을 출력하라.

```
    mov ax, 100 ; 짝수
    mov bl, 2
    
    div bl
    
    cmp ah, 0
    
    je LABEL_EQUAL2
    
    mov rcx, 0
    jmp LABEL_EQUAL_END2
    
LABEL_EQUAL2:
    mov rcx, 1
    
LABEL_EQUAL_END2:
    PRINT_HEX 1, rcx
    NEWLINE
```

결과값은 **1**이 출력된다. ax의 값을 홀수로 변경하면 결과값은 **0**이 출력된다.

***

## 👻 글을 마치며
이번 시간에는 어셈블리어로 분기문에 대해 공부해보았다. 다른 코드들은 if나 switch를 사용할텐데 어셈블리어는 비교할 두 수를 먼저 지정한 다음 각 조건에 따라 코드를 점프하는 방식으로 진행된다. 처음 접했을 땐 어색해서 어려웠지만 하다보니 코드의 흐름을 자연스레 이해할 수 있게 되었다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_assembly/blob/main/branch.asm)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   