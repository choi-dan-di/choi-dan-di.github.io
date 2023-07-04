---
title: "[Operating System] #5주차 - 퀴즈 풀이"
excerpt: "운영체제 공룡책 Main Memory 퀴즈 풀이 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, main memory, memory, quiz]

permalink: /operating-system/os-lecture-5-1-quiz/

toc: true
toc_sticky: true

date: 2023-07-04 19:24:19+0900
last_modified_at: 2023-07-04 19:24:23+0900
published: true
---

## 👻 퀴즈 풀이

***

### 🌱 1
- **문제**

> 다음과 같은 메모리 파티션이 순서대로 주어져있다.
>
> 100MB 500MB 200MB 300MB 600MB
>
> 다음과 같은 크기를 가진 네 개의 프로세스가 순서대로 도착했을 때,
>
> 212MB 417MB 112MB 426MB
>
> 다음 중 가장 효율적인 전략은 무엇인가?
>
> 1) First-Fit   
2) Best-Fit   
3) Worst-Fit   
4) Random-Fit

- **풀이**

정답은 **②**.

Best-Fit이 가장 Free Holes 낭비가 적으며 First-Fit, Worst-Fit 사용시 모든 프로세스가 메모리에 들어갈 수 없다. (Random-Fit은 뭐지?)

***

### 🌱 2
- **문제**

> ______ is the area in a region or a page that is not used by the job occupying that region or page.
>
> This space is unavailable for use by the system until that job is finished and the page or the region is released.
>
> ______ 에 들어갈 용어로 가장 알맞은 것은?
>
> 1) external fragmentation   
2) internal fragmentation   
3) segmentation   
4) separated paging

- **풀이**

정답은 **②**.

Job이 끝나거나 페이지 혹은 구간이 Release 될 때까지 사용할 수 없는 공간을 설명하고 있다. 이는 단편화를 의미하며 그 중에서도 페이지 내의 단편화를 의미하므로 내부 단편화(Internal Fragmentation)에 대한 설명이라고 볼 수 있다.

***

### 🌱 3
- **문제**

> 다음 중에서 페이징 기법에 대한 설명으로 가장 틀린 것은?
>
> 1) 페이징 기법은 연속 메모리 할당(Contiguous Memory Allocation) 방법보다 외부 단편화(Fragmentation) 현상이 덜 심하다.   
2) 페이징 기법을 사용하기 위해서는 각 페이지의 크기를 프로그램의 크기에 따라 다양하게 변경할 수 있어야 한다.   
3) 페이징 기법을 적용하기 위해서는 물리적인 메모리 공간을 여러 개의 작은 프레임으로 나누어야 한다.   
4) 페이징 기법을 적용하기 위해서는 페이징 테이블을 정보를 저장하기 위한 작은 캐시 메모리 장치가 필요하다.

- **풀이**

정답은 **②**.

페이징 기법은 고정된 사이즈를 가지는 페이지를 이용한다.

***

### 🌱 4
- **문제**

> 페이지의 크기가 1KB라고 하자. 주소 번지 3085번지가 저장되는 위치의 페이지 번호와 오프셋은 각각 얼마인가?
>
> 이 때 3085는 십진수이고, 페이지 번호는 0번부터 시작한다고 가정한다.
>
> 1) 3, 13   
2) 2, 1013   
3) 2, 13   
4) 3, 1013

- **풀이**

정답은 **①**.

1KB = 1024 bytes   
3085 / 1024 = 3 ··· 13

***

### 🌱 5
- **문제**

> Consider a logical address space of 64 pages of 1,024 words each, mapped onto a physical memory of 32 frames.   
a. How many bits are there in the logical address?   
b. How many bits are there in the physical address?
>
> 위 문제에서 word는 메모리 주소 공간의 기본 단위(byte)의 2배(2 bytes)를 의미한다.   
a, b에서 묻는 것은 각각 최소한 몇 비트가 있으면 이 주소 공간을 표현할 수 있는가를 질문한다.   
즉, 1024 = 2<sup>10</sup> 바이트의 주소 공간의 주소는 10비트를 필요로 한다.   
a, b에 대한 답으로 올바르게 묶인 것은?
>
> 1) 17, 16   
2) 16, 16   
3) 17, 15   
4) 16, 15

- **풀이**

정답은 **①**.

1024 words = 2048 bytes

64 pages * 2048 bytes = 2<sup>6</sup> * 2<sup>11</sup> = 2<sup>17</sup>

32 frames * 2048 bytes = 2<sup>5</sup> * 2<sup>11</sup> = 2<sup>16</sup>

***

### 🌱 6
- **문제**

> Consider a paging system with the page table stored in memory.   
a. If a memory reference takes 50 nanoseconds, how long does a paged memory reference take?   
b. If we add TLBs, and if 75 percent of all page-table references are found in the TLBs, what is the effective memory reference time?
>
> a. 메모리 참조에 50ns가 걸린다고 한다. 페이징 메모리에 접근하는 시간은 얼마인가?   
b. TLB가 있고, hit ratio가 0.75라고 한다. 유효 메모리 접근시간(EAT)은 얼마인가?
>
> 위 문제의 올바른 답으로 묶인 것은?
>
> 1) 50, 62.5   
2) 50, 78.5   
3) 100, 62.5   
4) 100, 78.5

- **풀이**

정답은 **③**.

페이징 메모리는 메모리 참조를 두 번 하기 때문에 50ns * 2 = 100ns가 걸린다.

EAT : 0.75 * 50 + 0.25 * 100 = 62.5

***

## 👻 글을 마치며
강의 들을 땐 어려워서 제대로 이해한 게 맞나 싶었는데 문제는 다 맞췄다! 확실히 풀면서 정리가 되니까 확실하게 공부가 되는 것 같다! 굿!

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   