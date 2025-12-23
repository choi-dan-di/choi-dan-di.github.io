---
title: "[ASM] 함수 기초"
excerpt: "어셈블리어의 함수 생성과 선언 방법에 대해 알아보기"

categories:
  - Assembly Language
tags:
  - [study, asm, assembly, function]

permalink: /asm/function/

toc: true
toc_sticky: true

date: 2022-11-16 17:28:26+0900
last_modified_at: 2022-11-16 17:28:28+0900
---

## 👻 함수(Function)
**함수**란 코드가 길어짐에 따라 쉽게 관리하기 위해 **자주 사용하는 부분**이나 **공통된 기능**을 따로 **분리하여 사용하는 것**을 의미한다. 어셈블리에선 ``` call ``` 명령어를 사용한다.

```
    call PRINT_MSG

    xor rax, rax
    ret

PRINT_MSG:
    PRINT_STRING msg
    NEWLINE
    ret     ; return

section .data
    msg db 'Hello World', 0x00
```

앞서 알아보았던 라벨과 만드는 형식이 동일하다.

***

### 🌱 함수 만들어보기
> 두 값 중 더 큰 값을 반환하는 MAX 함수 만들기

분기문을 이용하여 두 값을 비교한 다음 큰 값을 반환하는 방식으로 만들어보자. ``` eax ```와 ``` ebx ```를 입력값으로 받고 ``` ecx ```를 출력하는 형식이다.

```
MAX:
    cmp eax, ebx
    jg L1
    mov ecx, ebx
    jmp L2
L1:
    mov ecx, eax
L2:
    ret
```

**L1**과 **L2**는 라벨이다. 우선 eax, ebx 값을 비교해 eax가 더 크면 L1으로 점프, eax의 값을 ecx에 넣고 함수가 종료된다. L1으로 점프하지 않았으면 ``` mov ecx, ebx ```와 ``` jmp L2 ```가 실행된다. 함수 호출 부분은 다음과 같다.

```
; MAX 함수 호출 부분
mov eax, 10
mov ebx, 15
call MAX
PRINT_DEC 4, ecx
NEWLINE
```

> 하지만 이렇게 레지스터를 사용하게 되면 약간의 문제점이 발생한다. **인자가 10개 이상으로 많을 경우**, **eax, ebx에 이미 중요한 값이 있는 경우** 등이 있다. 고로 레지스터만 이용할 경우 약간의 위험 요소가 있다. 변수를 이용해 값을 저장해도 마찬가지이다.   
> 
> 영구적으로 사용되는 메모리에 모든 인자값을 할당하게되면 **메모리 낭비**가 생기게 되며, 이러한 방식으로 하는 **이중 함수 호출**은 데이터 관리에 좋지 않다.
>
> 이러한 문제점을 방지하기 위해서는 **다른 메모리 구조**인 **스택(Stack)**이라는 메모리 영역을 사용하면 된다.

***

## 👻 글을 마치며
이번 시간에는 어셈블리어로 함수를 생성하고 선언하는 방법에 대해 알아보았다. 생성하고 선언하는 게 간단했지만 그만큼 간단한 기능밖에 못 쓸 것 같다. 점프 범위도 한 눈에 알아보긴 힘들어서 여러번 반복 학습하면서 이해해야 할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_assembly/blob/main/function.asm)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   