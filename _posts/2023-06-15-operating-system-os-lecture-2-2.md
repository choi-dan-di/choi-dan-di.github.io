---
title: "[Operating System] #2주차 - CPU Scheduling"
excerpt: "운영체제 공룡책 강의 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, cpu, cpu scheduling]

permalink: /operating-system/os-lecture-2-2/

toc: true
toc_sticky: true

date: 2023-06-15 16:16:37+0900
last_modified_at: 2023-06-15 16:16:41+0900
published: true
---

## 👻 CPU Scheduling
CPU Scheduling은 멀티프로그래밍에서 필수이다. 멀티프로그래밍의 목적은 몇몇의 프로세스가 모든 시간 동안 작업을 수행하게 하고 CPU Utilization(점유화)을 높이기 위함이다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/burst.PNG)   

- **CPU burst**
    - CPU가 명령어를 연속적으로 수행하는 단계
    - CPU의 처리 시간이 길다.
- **I/O burst**
    - I/O 작업을 연속적으로 수행하는 단계
    - CPU의 처리 시간이 짧다.

위 이미지 우측의 그래프를 보면, CPU burst가 짧을수록 많이 나타나는 것을 알 수 있다. 주로 I/O 요청 작업을 처리한다. 반대로 CPU burst가 길수록 CPU 연산이 많이 요구되는 것으로 볼 수 있다.

> 💡 **bound**는 burst와 같은 의미로 사용되나 조금 더 많은 작업을 처리하는 부분을 의미한다.   
- 상대적으로 CPU burst가 I/O burst보다 많으면 **CPU bound**
- 상대적으로 CPU burst가 I/O burst보다 적으면 **I/O bound**

***

### 🌱 CPU Scheduler
프로그램은 CPU burst와 I/O burst가 번갈아가며 반복되는 방식으로 실행된다. 이러한 작업 과정에서 효율적으로 CPU를 사용하려면 스케줄러를 이용하여 최선의 작업 순서를 만들어야한다.

- 스케줄러를 만들 때 고민해야할 것
    - 어느 프로세스에 할당해야할까?
        - 이는 ready queue를 사용하여 CPU를 할당받을 프로세스를 지정할 수 있다.
    - 어떻게 다음 프로세스를 선택할까?
        - 아래와 같이 여러 방법이 있다.
        - Linked List? or Binary Tree?
        - FIFO Queue
        - Priority Queue

- **Preemptive(선점형)** vs **Non-preemptive(비선점형)**
    - 쉽게 말해 쫓아낼 수 있는 것(선점형)과 쫓아낼 수 없는 것(비선점형)
    - **Non-preemptive Scheduling**
        - 어느 프로세스가 CPU를 선점하면 Terminating되거나 Switching될 때까지 대기
    - **Preemptive Scheduling**
        - 비선점형과 반대
        - 스케줄러에 의해 중간에 CPU를 선점할 수 있다.

> 💡 현대적은 대부분 **선점형**을 사용한다.

- CPU Scheduling을 설정할 수 있는 부분
    1. 프로세스가 running에서 waiting으로 스위치될 때
    2. 프로세스가 running에서 ready로 스위치될 때
    3. 프로세스가 waiting에서 ready로 스위치될 때
    4. 프로세스가 terminate될 때

1번과 4번은 고려할 필요가 없다. 👉 무조건 비선점형

2번과 3번은 고려할 필요가 있다. 👉 선점형 or 비선점형

- **Dispatcher**
    - **CPU 코어에 컨트롤을 넘겨주는 모듈**이다. 그렇게 되면 CPU 스케줄러에 의해 프로세스가 선택된다.

- **Dispatcher의 기능**
    - 프로세서의 Context Switching
    - User mode로의 스위칭
    - 프로그램을 적절한 위치로 점핑시켜 Resume(재개) 해줌

디스패처는 가능한한 **빨라야** 하며, 디스패치 되는 시간을 **Dispatcher Latency**라고 한다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/dispatcher.PNG)   

***

## 👻 Scheduling Criteria
**Scheduling의 목표**는 다음과 같다.

1. **CPU Utilization**
2. **Throughput** : 단위 시간 내에 프로세스의 완결 수를 높이는 것(완료되는 프로세스의 수)
3. **Turnaround Time** : 프로세스 실행에서 종료까지의 시간을 최소화
4. **Waiting Time** : 프로세스의 대기 시간을 최소화
5. **Response Time** : UI 등의 반응 시간 줄이기

- **CPU Scheduling의 문제**
    - Ready Queue에 있는 프로세스 중 어느 프로세스를 고를 것이냐?

이 문제는 **다양한 알고리즘**으로 해결할 수 있다.

- **FCFS** : First-Come, First-Served
- **SJF** : Shortest Job First
    - **SRTF** : Shortest Remaining Time First
        - 남은 시간이 가장 짧은 프로세스 먼저
- **RR** : Round-Robin
    - 시분할해서 정해진 시간 만큼만 실행 후 돌리는 방식(Interleaving)
- **Priority-based**
    - 남은 아이들 중 우선순위를 부여
- **MLQ** : Multi-Level Queue
- **MLFQ** : Multi-Level Feedback Queue

***

### 🌱 FCFS
- First-Come, First-Served
- 가장 간단한 스케줄링 알고리즘
- CPU에 가장 먼저 요청한 프로세스에 CPU를 할당
- CPU burst time에 따라 평균 대기 시간이 달라짐
- **Non-preemptive**
- Convoy Effect(호송 효과)
    - CPU burst time이 큰 프로세스가 먼저 들어오면 뒤에는 계속 대기 상태가 됨

- **FCFS 정책의 간트 차트(Gantt Chart)**

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/gantt-fcfs.PNG)   

P<sub>1</sub>, P<sub>2</sub>, P<sub>3</sub>는 각각 CPU burst time이 24, 3, 3이다. 이에 대해 Waiting Time을 구하면 다음과 같다.

- Waiting Time for Processes
    - P<sub>1</sub> = 0
    - P<sub>2</sub> = 24
    - P<sub>3</sub> = 27
- _Total_ Waiting Time
    - (0 + 24 + 27) = 51
- _Average_ Waiting Time
    - 51 / 3 = 17

> 💡 프로세스가 2, 3, 1 순으로 들어왔다면 간트 차트는 다음과 같다.   
![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/gantt-fcfs-2.PNG)   
또한 대기 시간도 각각 6, 0, 3으로 줄어들고 Total은 9, Average는 3이 될 것이다.

Turnaround Time의 간트 차트는 아래와 같다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/gantt-fcfs-turn.PNG)   

- Turnaround Time for Processes
    - P<sub>1</sub> = 24
    - P<sub>2</sub> = 27
    - P<sub>3</sub> = 30
- _Total_ Turnaround Time
    - (24 + 27 + 30) = 81
- _Average_ Turnaround Time
    - 81 / 3 = 27

해당 알고리즘은 간단한 만큼 프로세스 입력 순서에 많은 의존성을 보이기 때문에 최적의 결과를 보장할 수 없는 등 문제가 많다.

***

### 🌱 SJF
- Shortest-Job-First : Shortest-Next-CPU-Burst-First
- next CPU burst time이 가장 적은 프로세스부터 처리하자.
- 그럴려면 next CPU burst time을 알고 있어야 한다.
- burst time이 동일하면 FCFS
- 최적이 증명가능하다. (Provably Optimal)
- 하지만 **구현은 불가능**.
    - next CPU burst time을 알 방법이 없다.
- 엄격하게 적용할 순 없고 근사적으로 구할 수 있다.
    - next CPU burst time을 예측하는 방법
- **Preemptive** or **Non-preemptive**
    - 남은 burst time을 알 수 없기 때문에 선점형도 구현하긴 쉽지 않다.

**[next CPU burst time의 예측은 어떻게 할까?]**

- 과거의 CPU burst를 가지고 측정하면 Exponential Average를 구할 수 있다.
- 이론적으론 최적을 보장하나 실제로 사용하진 않음

> **τ<sub>n + 1</sub> = αt<sub>n</sub> + (1 - α)τ<sub>n</sub>**   

- τ<sub>n</sub>은 n번째 CPU burst의 길이이다.
- τ<sub>n + 1</sub>은 next CPU burst의 예측값이다.
- 0 ≤ α ≤ 1

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/sjf.PNG)   

위 그래프에서 검은선은 실제 CPU burst time을 의미하고 파란선은 이전 CPU burst를 이용한 next CPU burst의 예측값을 나타낸다.

***

### 🌱 SRTF
- Shortest-Remaining-Time-First
    - **Preemptive SJF** Scheduling
- SJF와 비슷하나 burst time이 더 적은 프로세스를 선점하여 작업을 수행한다.

- **SRTF 정책의 간트 차트**

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/gantt-srtf.PNG)   

위와 같이 프로세스의 정보가 있고, 해당 정보를 바탕으로 간트 차트를 만들었을 때 이들의 Waiting Time은 다음과 같다.

- _Total_ Waiting Time
    - [(10 - 1) + (1 - 1) + (17 - 2 + (5 - 3)] = 26
    - 프로세스 작업 시작 시간 - Arrival Time
- _Average_ Waiting Time
    - 26 / 4 = 6.5

***

### 🌱 RR
- Round-Robin
    - **Preemptive FCFS**(선점형 FCFS) with a **time quantum**
- Time Quantum이라는 일종의 **수행 시간 단위**를 이용해 들어온 프로세스 순으로 처리한다.
- Time Quantum만큼 수행한 후 Next Process로 CPU가 넘어간다.
- Time Quantum보다 burst가 더 **짧으면** 자발적으로 빠져나오며, 더 **길면** 인터럽트로 빠져나온다.
- 대기 시간은 더 길어질 수 있지만 다른 알고리즘과 섞어쓰면 유용하기 때문에 RR은 반드시 사용된다고 볼 수 있다.
- **Preemptive**
- Time Quantum의 크기에 따라 성능이 달라진다.
    - Time Quantum에 많이 의존적이다.
- Time Quantum이 **무한대**면 **FCFS**
- Ready Queue는 _Circular Queue_ 로 처리된다.

- **RR 정책의 간트 차트**

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/gantt-rr-1.PNG)   

위와 같이 프로세스의 정보가 있고 Time Quantum이 4ms일 때, RR 정책의 간트 차트는 아래와 같다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/gantt-rr-2.PNG)   

The **Waiting Time**:

- Waiting Time for Processes
    - P<sub>1</sub> = 10 - 4 = 6
    - P<sub>2</sub> = 4
    - P<sub>3</sub> = 7
- _Total_ Waiting Time
    - (6 + 4 + 7) = 17
- _Average_ Waiting Time
    - 17 / 3 = 5.66

***

### 🌱 Priority-base
- 프로세스에 우선순위를 주고 높은 순으로 할당하는 방식
- 동일한 우선순위는 FCFS를 따름
- SJF의 경우, next CPU burst의 Inverse(역순)가 우선순위가 된다.
    - 즉, next CPU burst가 클수록 우선순위가 낮아짐
- 기본적으로 낮은 숫자가 **높은 우선순위**를 가진다.
- **Preemptive** or **Non-Preemptive**

해당 알고리즘의 대표적인 문제로 **Starvation(기아; indefinite blocking)**가 있다.

- 우선순위가 낮은 프로세스의 경우 **무한 대기상태**가 될 수 있음

이러한 문제를 해결하는 방법은 **Aging**이 있다.

- 마냥 같은 우선순위를 가지는 게 아닌, 점진적으로 우선순위를 높여주는 방법

***

### 🌱 MLQ
- Multi-Level Queue
- 분리 된 Ready Queue에 각각의 우선순위를 배정

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/mlq.PNG)   

- 실시간 프로세스의 경우 우선순위가 높으며, 그 다음으로 시스템, 인터랙티브, 배치 순으로 낮아진다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/mlq2.PNG)   

해당 알고리즘 또한 Starvation 발생 위험이 있다.

***

### 🌱 MLFQ
- Multi-Level Feedback Queue
- MLQ에서 발생할 수 있는 Starvation을 해결하기 위한 알고리즘
    - 각각의 Ready Queue에 Time Quantum을 다르게 설정하거나, 다른 스케줄링 알고리즘을 사용하면 Starvation을 해결할 수 있다.
- 실전 OS의 CPU Scheduling 알고리즘이라고 볼 수 있다.
    - 여기에 Multicore가 더해지면 효율 UP

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-2/mlfq.PNG)   

***

## 👻 Thread Scheduling
실질적으로는 프로세스보다 스레드 스케줄링을 더 많이 한다.

**user thread**는 스레드 라이브러리가 관리하므로 스케줄링은 **kernel thread**만 해주면 된다.

> 지금까지 봐왔던 스케줄링은 모두 kernel thread에 적용된다고 볼 수 있음!

***

## 👻 Real-Time CPU Scheduling
- **Real-Time Operating System**
    - 주어진 시간 내에 완료할 수 있는 것

Real-Time OS에서의 스케줄링은 두 가지로 나뉜다.

- **Soft Realtime**
    - 빨리 제공하는 게 목적
    - 중간에 정보의 일부를 살짝 잃어도 괜찮음(전화 같은 것)
- **Hard Realtime**
    - 어떤 Task가 반드시 데드라인 안에 실행되어야 한다.
    - 정보 손실을 용납하지 않는다(로켓 발사같이 모든 수치가 정확해야 하는 것)

두 방법 모두 **Priority-based** 알고리즘을 사용한다.

***

## 👻 글을 마치며
이번 시간에는 CPU 스케줄링에 대해 알아보았다. 기억하고 있었던 게 라운드 로빈 밖에 없었는데 다시 보다보니 기억이 새록새록 떠오르는 것 같다. 정확히 어떻게 동작하는지 알 수 있었고 아직 좀 헷갈리는 부분이 많은데, 아마 상반되는 단어들이 많이 나와서 그런 것 같다. 이쪽의 개념 비교는 따로 정리를 해야할 것 같다(선점형과 비선점형, 동기와 비동기, 동시성과 병렬성 등..?).

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   