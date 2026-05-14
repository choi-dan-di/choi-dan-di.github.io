---
title: "[Operating System] #3주차 - 퀴즈 풀이"
excerpt: "운영체제 공룡책 Synchronization Tools (1) 퀴즈 풀이 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, synchronization, tools]

permalink: /operating-system/os-lecture-3-1-quiz/

toc: true
toc_sticky: true

date: 2023-06-21 17:47:21+0900
last_modified_at: 2023-06-21 17:47:25+0900
published: true
---

## 👻 퀴즈 풀이

***

### 🌱 1
- **문제**

> 프로세스 동기화(Synchronization)에 대한 설명으로 가장 옳은 것은?   
>
> 1) 여러 프로세스가 공유 자원에 접근할 때, 병렬적(Parallel)인 처리를 할 때는 항상 동기화가 필요하지만, 병행적(Concurrent)인 처리를 할 때는 항상 동기화가 필요하지는 않다.   
2) 생산자-소비자 문제를 두 개의 프로세스가 Shared Memory로 처리할 때는 동기화를 해 주어야 하지만, 두 개의 스레드가 같은 주소 공간에서 Buffer를 저장할 때는 동기화가 필요하지 않다.   
3) 여러 스레드가 어떤 공유하는 변수에 접근하여 값을 변경하지 않고 읽기만 하는 경우에는 경쟁 상황이 발생하지 않으므로 동기화를 해 줄 필요가 없다.   
4) 프로세스 동기화(Synchronization)는 프로세스가 공유하는 자원에 접근하는 일련의 순서를 정하도록 한다. 따라서 공유하는 프로세스들이 항상 같은 순서로 순차적으로 자원에 접근할 수 있게 하여 경쟁 상황을 방지한다.

- **풀이**

정답은 **③**.

1) 데이터 불일치는 Concurrently한 상황에서도 발생할 수 있다.   
2) 스레드 간에도 같은 부분을 공유하는 상황이라면 동기화가 필요하다.   
4) 항상 같은 순서가 아닐 수도 있다.

***

### 🌱 2
- **문제**

> 임계 영역(Critical Section)에 대한 설명으로 가장 틀린 것은?   
>
> 1) critical-section은 어떤 프로세스의 코드 영역 중에서 여러 프로세스가 공유하는 자원에 접근하는 코드 영역을 말하고, entry-section과 exit-section 사이에 위치한다.    
2) entry-section은 임계 영역에 진입하기 위한 권한을 획득하는 코드 영역을 말하고, 항상 critical-section 이전에 위치해야 한다.   
3) exit-section은 임계 영역을 빠져나와서 권한을 반환하는 코드 영역을 말하고, 항상 critical-section 이후에 위치해야 한다.   
4) remainder-section은 entry-, exit-, critical-section이 아닌 코드 영역을 말하고, 항상 exit-section 이후에 와야 한다.

- **풀이**

정답은 **④**.

remainder-section은 나머지 영역이므로 entry-section 이전에 위치할 수도 있다.

***

### 🌱 3
- **문제**

> 임계 영역 문제(Critical-Section Problem)에 대한 솔루션이 해결해야 할 요구사항에 포함하지 않아도 되는 것은?   
>
> 1) Mutual Exclusion   
2) Progress (No Deadlock)   
3) Bounded Waiting (No Starvation)   
4) Scalability

- **풀이**

정답은 **④**.

CSP에 대한 솔루션이 해결해야 할 요구사항은 Mutual Exclusion, Progress, Bounded Waiting이다. 확장성은 해당하지 않는 부분이다.

***

### 🌱 4
- **문제**

> 임계 영역 문제(Critical Section Problem)의 해결책에 대한 설명으로 가장 옳은 것은?
>
> 1) Single-core 시스템에서는 단순하게 인터럽트를 방지하는 것만으로 임계 영역 문제를 해결할 수 없다.   
2) 비선점형(Non-Preemptive) 커널에서는 경쟁 상황이 발생하지 않으므로 임계 영역 문제를 고려할 필요가 없다.   
3) 피터슨 알고리즘은 상호 배제 문제를 확실하게 해결했지만, 데드락과 기아 문제를 해결하기 위한 하드웨어적인 지원이 필요하다.   
4) 원자적 변수(Atomic Variable)는 공유 변수에 대한 접근을 하는 명령어(Instruction)를 하드웨어적으로 만들어 지원해 주는 임계 영역 문제 해결책이다.

- **풀이**

정답은 **②**.

1) Single-core 시스템에서는 인터럽트를 방지하는 것만으로 해당 문제를 해결할 수 있다.   
3) 피터슨 알고리즘은 세 요구사항을 모두 만족한다.   
4) Atomic Variable은 CSP를 해결해주는 명령어에 제공되는 변수이다. 직접적으로 CSP를 해결하는 것은 아니다. (제대로 이해한 게 맞을까요?)

***

### 🌱 5
- **문제**

> 아래와 같이 피터슨 알고리즘을 구현했을 때, A, B, C, D에 들어갈 값으로 잘못 짝지어진 것을 모두 고르시오.   
>
> ```c++
> while (true) {
>     /* entry section */
>     flag[0] = true;
>     turn = ⓐ;
>     while (flag[ⓑ] && turn == ⓒ)
>         ;
> 
>         /* critical section */
>         sum++;
> 
>     /* exit section */
>     flag[ⓓ] = false;
> 
>         /* remainder section */
> }
> ```
>
> 1) A = 0   
2) B = 1   
3) C = 0   
4) D = 1

- **풀이**

정답은 **①, ③, ④**.

```c++
while (true) {
    /* entry section */
    flag[0] = true;
    turn = 1;
    while (flag[1] && turn == 1)
        ;

        /* critical section */
        sum++;

    /* exit section */
    flag[0] = false;

        /* remainder section */
}
```

***

### 🌱 6
- **문제**

> 수업 시간에 다룬 Producer-Consumer 예제에서, Producer가 두 개의 스레드로 실행되고, Consumer가 두 개의 스레드로 실행되었다고 가정해 보자. 만약 count의 값이 현재 5인 상태에서, 네 개의 스레드가 한 번씩 Concurrent하게 실행되었다면, 다음 중 최종 count의 값을 가능한 값을 모두 고르시오.
>
> 1) 3   
2) 4   
3) 5   
4) 6   
5) 7

- **풀이**

정답은 **①, ②, ③, ④, ⑤**.

count가 Atomicity하면 동기화가 되겠지만 문제의 경우 Data Inconsistency를 피할 수 없기 때문에 count의 범위는 5 ± 4가 된다.

***

### 🌱 7
- **문제**

> 하드웨어 솔루션으로 임계 영역 문제를 해결할 때, 이에 대한 설명으로 가장 틀린 것은?   
>
> 1) 임계 영역 문제를 해결하기 위한 하드웨어 솔루션은 Atomicity(원자성)을 보장하는 명령어를 제공한다.   
2) test_and_set 명령어는 Boolean 변수인 lock을 이용한다.   
3) compare_and_swap 명령어는 전역 변수인 lock을 이용한다.   
4) atomic_variable 명령어는 변수에 대한 접근을 제어하는 하드웨어 명령어이다.

- **풀이**

정답은 **④**.

atomic_variable은 존재하지 않는 것으로 판단...

***

### 🌱 8
- **문제**

> 임계 영역 문제에 대한 설명으로 가장 틀린 것은?   
>
> 1) 경쟁 상황(Race Condition)이 발생할 수 있는 코드 영역을 임계 영역(Critical Section)이라 한다.   
2) 피터슨 알고리즘은 임계 영역 문제에 대한 소프트웨어 해결책이고, compare_and_swap은 하드웨어 해결책이다.   
3) Atomic Variable을 이용하여 생산자-소비자 문제의 동기화를 해결할 수 있다.   
4) Java에서 피터슨 알고리즘을 그대로 구현하면 Data Inconsistency가 발생하지 않는다.

- **풀이**

정답은 **④**.

Java에서 피터슨 알고리즘을 그대로 구현하면 일반적으로 Data Inconsistency가 발생하진 않지만 보장할 수는 없다. 데이터 일관성을 **보장**하기 위해선 추가적으로 동기화 메커니즘을 사용해야 한다. ~~라고 ChatGPT가 말합니다..~~

***

## 👻 글을 마치며
퀴즈 풀이는 즐거웟. 복습도 되고 좋은 것 같다. 다만 풀이가 없어서 맞는 풀이인지는 검수가 필요할 것 같다 😂

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   