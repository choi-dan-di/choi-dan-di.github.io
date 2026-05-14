---
title: "[Operating System] #5주차 - 퀴즈 풀이"
excerpt: "운영체제 공룡책 Virtual Memory 퀴즈 풀이 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, virtual memory, memory, quiz]

permalink: /operating-system/os-lecture-5-2-quiz/

toc: true
toc_sticky: true

date: 2023-07-07 16:08:31+0900
last_modified_at: 2023-07-07 16:08:34+0900
published: true
---

## 👻 퀴즈 풀이

***

### 🌱 1
- **문제**

> 다음 페이지 교체 알고리즘들 중에서 Belady's Anomaly 현상을 겪지 않는 알고리즘으로 묶인 것은?
>
> a. LRU replacement   
b. FIFO replacement   
c. Optimal replacement   
d. Second-chance replacement
>
> 1) a, b   
2) a, c   
3) b, c   
4) b, d

- **풀이**

정답은 **②**.

FIFO 알고리즘을 이용하면 Belady's Anomaly 현상이 나타난다. Second-chance도 FIFO 알고리즘을 기반으로 하므로 마찬가지로 동일한 현상이 나타난다.

***

### 🌱 2
- **문제**

> Consider the following page reference string:
>
> 1, 2, 3, 4, 5, 3, 4, 1, 6, 7, 8, 7, 8, 9, 7, 8, 9, 5, 4, 5, 4, 2.
>
> How many page faults would occur for the following replacement algorithms, assuming four frames? Remember that all frames are initially empty, so your first unique pages will cost one fault each.
>
> a. LRU replacement   
b. FIFO replacement   
c. Optimal replacement
>
> 위 문제에 대한 적절한 해답으로 묶인 것은?
>
> 1) a = 11, b = 12, c = 12   
2) a = 12, b = 13, c = 15   
3) a = 11, b = 13, c = 13   
4) a = 11, b = 12, c = 13   
5) a = 13, b = 13, c = 11

- **풀이**

정답은 **⑤**.

프레임에 해당 페이지가 없을 때마다 Page Faults가 발생한다. 교체 기준은 다음과 같다.

- LRU : 이전 참조 페이지 중 가장 오래전에 사용했던 페이지를 Victim으로 선택
- FIFO : 가장 먼저 프레임에 올라왔던 페이지 순으로 Victim 선택
- Optimal : 미래에 올 페이지들을 파악해 늦게 참조되는 페이지를 Victim으로 선택

***

### 🌱 3
- **문제**

> Consider the following page reference string:
>
> 7, 2, 3, 1, 2, 5, 3, 4, 6, 7, 7, 1, 0, 5, 4, 6, 2, 3, 0, 1.
>
> Assuming demand paging with three frames, how many page faults would occur for the following replacement algorithms?
>
> a. LRU replacement   
b. FIFO replacement   
c. Optimal replacement
>
> 위 문제에 대한 올바른 답으로 묶인 것은?
>
> 1) a = 17, b = 17, c = 15   
2) a = 17, b = 18, c = 15   
3) a = 18, b = 17, c = 14   
4) a = 18, b = 17, c = 13   
5) a = 18, b = 18, c = 13

- **풀이**

정답은 **④**.

풀이는 문제 2번과 같음.

***

### 🌱 4
- **문제**

> 어떤 시스템이 Demand Paging을 도입했는데, 페이징 디스크의 평균 액세스 시간이 20 millisecond라고 하자.   
페이지 테이블은 메인 메모리에 저장이 되어 있고, 메인 메모리 액세스 시간은 1 microsecond라고 하자.   
페이지 테이블을 통한 각 메모리의 참조는 2 microsecond가 걸리게 될 것이다.   
여기에 TLB를 추가했고, TLB hit ratio는 80%로 측정되었다.   
TLB miss가 발생하는 20%의 10% (즉, 전체의 2%)는 페이지 폴트가 발생한다.
>
> 다음 중 이 시스템에서 유효 메모리 접근 시간(EAT)과 가장 근사한 값은 얼마인가?
>
> 1) 0.1 millisecond   
2) 0.2 millisecond   
3) 0.3 millisecond   
4) 0.4 millisecond   
5) 0.5 millisecond

- **풀이**

정답은 **④**.

**EAT = (1 - p) * ma + p * (page fault time)**

- ``` ma ``` = memory access time = 2 microseconds
- ``` page fault time ``` = 20 milliseconds

> 💡 **단위 변경**   
1 second = 1,000 millisecond   
**1 millisecond = 1,000 microseconds**   
1 microsecond = 1,000 nanosecond   
>
> 1 second = 1,000,000,000 nonasecond

∴ EAT = (1 - p) * 2 + p * 20,000 = 2 + 19998 * p

여기서 p = 0.02 이므로 이를 대입하면

EAT = 2 + 19998 * 0.02 = 401.96 microseconds ≒ **0.4 milliseconds**

***

### 🌱 5
- **문제**

> Consider a demand-paging system with the following time-measured utilizations:
>
> CPU utilization: 20%   
Paging disk: 97.7%   
Other I/O devices: 5%
>
> For each of the following, indicate whether it will (or is likely to) improve CPU utilization.
>
> 디맨드 페이징 시스템에서 시스템 측정 결과가 위와 같이 나올 때, 아래의 전략 중 CPU utilization을 향상시킬 수 있을 것 같은 전략으로만 묶인 것은?
>
> a. Install a faster CPU.   
b. Install more main memory.   
c. Install a bigger paging disk.   
d. Increase the page size.   
e. Decrease the degree of multiprogramming.
>
> 1) a, b, c, d, e   
2) a, b, c   
3) a, c, e   
4) b, e   
5) c, d, e

- **풀이**

정답은 **④**.

위 문제는 Thrashing 현상을 나타내고 있다. 이를 해결하기 위해선 메인 메모리의 용량을 늘리거나 멀티프로그래밍 정도를 줄여야한다.

***

### 🌱 6
- **문제**

> 페이지 폴트가 계속적으로 너무 자주 발생하게 되어 프로세스의 실행 시간보다 페이지 교체를 하는 시간이 더 많아지는 현상과 관련이 높은 것은?
>
> 1) 외부 단편화 (External Fragmentation)   
2) 프로그램의 국부성 (The Locality of Programs)   
3) 스레싱 (Thrashing)   
4) 역전 페이지 테이블 (Inverted Page Table)

- **풀이**

정답은 **③**.

***

### 🌱 7
- **문제**

> 다음 중 가상 메모리(Virtual Memory)에 대한 설명으로 가장 틀린 것은?
>
> 1) 가상 메모리를 사용하면 물리적 주소 공간의 크기는 가상 주소 공간의 크기보다 더 작아도 문제가 없다.   
2) 가상 주소 공간에서의 주소와 물리적 주소 공간에서의 주소 공간은 서로 독립적이다.   
3) 디맨드 페이징을 적용하여 여러 페이지를 분산하여 메모리에 적재하므로 실행 속도가 훨씬 빠르다.   
4) 가상 메모리를 도입하면 하나의 프로세스의 크기가 물리적인 메모리 용량보다 더 커도 문제가 없다.

- **풀이**

정답은 **③**.

디맨드 페이징은 페이지를 요구할 때마다 메모리에 로드하는 방식이므로 설명이 맞지 않다.

***

### 🌱 8
- **문제**

> 프로그램의 국부성(The Locality of Program)에 대한 설명으로 가장 옳지 않은 것은?
>
> 1) 반복 횟수가 많은 for문을 자주 사용하면 국부성이 높아진다고 할 수 있다.   
2) 프로그램의 국부성이 높을수록 디맨드 페이징을 사용하는 시스템의 성능은 좋아진다.   
3) 자주 사용하는 전역 변수들은 heap 영역에서 가급적이면 넓게 분포하도록 하면 프로그램의 효율을 올릴 수 있다.   
4) 가상 메모리를 사용하지 않고 Contiguous Memory Allocation을 하는 시스템이라면 프로그램의 국부성은 성능과의 관련이 많지 않다.

- **풀이**

정답은 **③**.

***

## 👻 글을 마치며
문제를 풀어보면서 각 교체 알고리즘이 어떠한 방식으로 돌아가는지 알 수 있었고, 특성들에 대해 다시 한 번 더 정리할 수 있는 시간이었다🤗 다만 페이징 디스크는 처음 들어보는 것이라 또 추가로 공부했다.. 운영체제는 확실히 어려운 단어가 많은 것 같다 ㅠㅠ

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   