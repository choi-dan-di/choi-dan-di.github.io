---
title: "[Operating System] #2주차 - 퀴즈 풀이"
excerpt: "운영체제 공룡책 CPU Scheduling 퀴즈 풀이 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, cpu, cpu scheduling, quiz]

permalink: /operating-system/os-lecture-2-2-quiz/

toc: true
toc_sticky: true

date: 2023-06-15 17:57:39+0900
last_modified_at: 2023-06-15 17:57:43+0900
published: true
---

## 👻 퀴즈 풀이

***

### 🌱 1
- **문제**

> The ________ is a module that gives control of the CPU's core to the process selected by the CPU scheduler.   
>
> 1) dispatcher   
2) distributor   
3) deployer   
4) dissipator

- **풀이**

정답은 **①**.

해석 : 이것은 CPU 스케줄러에 의해 프로세스를 선택할 수 있도록 CPU 코어에게 컨트롤을 넘겨주는 모듈이다. 👉 Dispatcher(디스패처)

***

### 🌱 2
- **문제**

> 다음 중 CPU 스케줄러를 설계할 때 목표로 삼기에 가장 어색한 것은?   
>
> 1) CPU의 사용효율(Utilization)을 높이겠다.   
2) 단위시간당 처리하는 프로세스의 개수(Throughput)을 늘리겠다.   
3) 프로세스가 대기하는 시간(Waiting Time)을 줄이겠다.   
4) 프로세스의 응답시간(Response Time)을 늘리겠다.

- **풀이**

정답은 **④**.

프로세스의 응답시간을 줄이는 것이 CPU Scheduling의 큰 목표 중 하나이다.

***

### 🌱 3
- **문제**

> 선점형(Preemptive), 비선점형(Non-Preemptive) 스케줄러에 대한 설명으로 가장 옳은 것은?   
>
> 1) Shortest-Job-First(SJF)는 짧은 CPU burst를 가진 프로세스를 먼저 처리하는 Preemptive 스케줄러다.   
2) First-Come, First-Served(FCFS)는 먼저 도착한 프로세스를 먼저 처리하는 Preemptive 스케줄러다.   
3) CPU를 점유한 프로세스가 Waiting 상태에서 Ready 상태로 갈 때는 반드시 Non-Preemptive 스케줄링을 해야한다.   
4) Round-Robin 스케줄러는 Time Quantum이 지나면 CPU를 점유한 프로세스를 내보내는 Preemptive 스케줄러다.

- **풀이**

정답은 **④**.

1, 2번의 경우 SJF와 FCFS는 Non-Preemptive(비선점형) 스케줄러이며, 3번의 경우 프로세스의 상태가 Waiting이나 Running에서 Ready로 스위치 될 때는 선점형과 비선점형 중에 택할 수 있다.

***

### 🌱 4
- **문제**

> CPU Scheduler에 대한 설명으로 가장 틀린 것은?   
>
> 1) FCFS를 구현하기 위해서는 FIFO Queue를 Ready-Queue의 자료구조로 사용할 수 있다.   
2) Shortest-Remaining-Time-First 스케줄러는 Preemptive SJF라고 할 수 있다.   
3) Round-Robin 스케줄러는 Preemptive FCFS라고 할 수 있다.   
4) Multi-Level Feedback Queue 스케줄러는 여러 개의 Ready-Queue에 우선순위에 따라 영구적으로 한 개의 큐에 프로세스를 배정한다.   
5) Soft-Real-Time 요구사항을 만족하는 Real-Time OS의 스케줄러는 Priority-based CPU 스케줄러를 사용한다.

- **풀이**

정답은 **④**.

해당 보기의 설명은 MLQ에 관한 것이다.

***

### 🌱 5
- **문제**

> |Process|Arrival Time|CPU Burst|   
> |P1|0|5|   
> |P2|1|7|   
> |P3|3|4|   
>
> 위와 같이 P1, P2, P3 프로세스의 도착시간과 CPU Burst가 주어졌다. FCFS와 RR 스케줄러를 사용하면 프로세스의 완료 순서가 각각 어떻게 될까? (RR 스케줄러의 Time Quantum은 2를 사용한다.)   
>
> 1) FCFS : P1, P2, P3   
&nbsp;&nbsp;&nbsp;&nbsp;RR2 : P1, P2, P3   
2) FCFS : P1, P3, P2   
&nbsp;&nbsp;&nbsp;&nbsp;RR2 : P1, P3, P2   
3) FCFS : P1, P2, P3   
&nbsp;&nbsp;&nbsp;&nbsp;RR2 : P1, P3, P2   
4) FCFS : P1, P3, P2   
&nbsp;&nbsp;&nbsp;&nbsp;RR2 : P1, P2, P3   

- **풀이**

정답은 **③**.

FCFS는 도착한 순서대로 프로세스가 CPU를 선점하기 때문에 Arrival Time 순인 P1, P2, P3이 되고, RR2의 경우 P1, P2, P3, P1, P2, P3, P1(완료), P2, P3(완료), P2(완료) 순으로 작업이 처리된다. RR2 정책의 간트 차트는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2-quiz/5.jpg)   

***

### 🌱 6
- **문제**

> |Process|Arrival Time|CPU Burst|   
> |P1|0 ms|9 ms|   
> |P2|1 ms|4 ms|   
> |P3|2 ms|9 ms|   
>
> 위와 같이 프로세스 P1, P2, P3의 도착시간과 CPU Burst가 주어졌다.   
만약 Preemptive SJF 스케줄러를 사용한다면 평균 대기시간(Average Waiting Time)은 얼마인가?   
단, 스케줄러는 프로세스가 도착할 때와 프로세스가 완료할 때만 동작한다고 가정한다.
>
> 1) 5.00 ms   
2) 4.33 ms   
3) 6.33 ms   
4) 7.33 ms

- **풀이**

정답은 **①**.

Preemptive SJF는 SRTF이며, 이는 남은 Burst가 가장 적은 순으로 실행되는 알고리즘이다. 해당 정책의 간트 차트는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2-quiz/6.jpg)   

***

### 🌱 7
- **문제**

> |Process|Arrival Time|CPU Burst|   
> |P1|0|10|   
> |P2|3|6|   
> |P3|7|8|   
> |P4|8|3|   
>
> 위와 같이 P1, P2, P3, P4 프로세스의 도착시간과 CPU Burst가 주어졌다.   
Preemptive SRTF(Shortest-Remaining-Time-First) 알고리즘을 사용한다고 했을 때, 평균 총처리시간 (Average Turnaround Time)은 얼마인가?   
>
> 1) 10.25   
2) 11.25   
3) 12.25   
4) 13.25   
5) 14.25

- **풀이**

정답은 **③**.

Preemptive SRTF는 해당 시점에 남아있는 Burst가 가장 작은 순으로 CPU를 선점하여 작업을 수행한다. 해당 정책의 간트 차트는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2-quiz/7.jpg)   

***

### 🌱 8
- **문제**

> |Process|CPU Burst|Priority|   
> |P1|10|3|   
> |P2|1|1|   
> |P3|2|3|   
> |P4|1|4|   
> |P5|5|2|   
>
> 위와 같이 5개의 프로세스에 대한 CPU Burst와 우선순위가 주어졌다.   
적용하는 스케줄러별로 Total Waiting Time이 잘못 짝지어진 것은?   
>
> 1) Non-Preemptive FCFS = 47   
2) Non-Preemptive SJF = 16   
3) RR (Time Quantum = 1) = 26   
4) Non-Preemptive Priority-based (Smaller Number, Higher Priority) = 41

- **풀이**

정답은 **①, ③**.

우선순위를 사용하는 것은 4번뿐이다. 그 외에는 모두 우선순위를 제외하고 도착 시간과 Burst만을 사용하여 값을 구한다. 참고로 모든 프로세스의 도착시간은 0으로 동일하고 1, 2, 3, 4, 5 순으로 도착했다고 가정하고 풀었다. 모든 보기의 간트 차트는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2-quiz/8.jpg)   

***

## 👻 글을 마치며
이번 시간에는 CPU 스케줄링과 관련된 문제를 풀어보았다. 제대로 푼 게 맞는지 확실치가 않은 것 같다 ㅠㅠ 아직까지는 선점과 비선점이 헷갈리고 SJF와 SRTF가 좀 헷갈리는 것 같다. 이 부분에 대해선 시간을 더 내서 개념을 확립해야할 것 같다. (정리하는 것도 잊지 말기!!!)

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   