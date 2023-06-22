---
title: "[Operating System] #3주차 - 퀴즈 풀이"
excerpt: "운영체제 공룡책 Synchronization Tools (2) 퀴즈 풀이 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, synchronization, tools, mutex, semaphore, monitor, liveness, quiz]

permalink: /operating-system/os-lecture-3-2-quiz/

toc: true
toc_sticky: true

date: 2023-06-22 18:33:26+0900
last_modified_at: 2023-06-22 18:33:30+0900
published: true
---

## 👻 퀴즈 풀이

***

### 🌱 1
- **문제**

> 국수를 아주 좋아하는 철학자 5명이 모였습니다.   
그들은 원형 테이블에 둘러 앉아 "삶이란 무엇인가?"라는 질문에 대한 해답을 고민하기 시작했습니다.   
원형 테이블의 중앙에는 국수가 놓여 있었고, 테이블에는 모두 다섯 개의 젓가락이 놓여 있었습니다.   
즉, 철학자들 한 명의 왼쪽과 오른쪽에 각각 한 개의 젓가락이 놓여 있다는 뜻입니다.   
따라서 철학자들은 왼쪽과 오른쪽 양쪽의 젓가락을 이용하여 국수를 먹을 수 있습니다.   
국수는 무한리필되기 때문에 5명의 철학자들은 해답을 찾을 때까지 생각을 하기로 했습니다.   
하지만 어느날 그들은 모두 굶어 죽었습니다. 왜 그랬을까요?   
>
> 이 문제는 자원(젓가락)을 공유하는 프로세스(철학자)의 동기화 문제로 유명한 메타포어인 "철학자들의 저녁식사" 문제입니다.   
위 문제의 설명으로 적절한 것을 모두 고르시오.   
>
> 1) 시계 방향으로 돌아가면서 먹기로 약속을 정했으면 아무도 굶어죽지 않았을 것이다.   
2) 모두가 왼쪽에 있는 젓가락을 먼저 집고, 오른쪽 젓가락을 나중에 집기로 약속을 한다면, 아무도 굶어죽지 않을 것이다.   
3) 인접한 두 철학자들이 서로 동시에 식사를 하지 않기로 약속을 한다면, 아무도 굶어죽지 않을 것이다.   
4) 홀수 번호의 철학자는 먼저 왼쪽 젓가락을 집고, 다음에 오른쪽 젓가락을 집도록 하고, 반대로 짝수 번호인 철학자는 오른쪽 젓가락을 먼저 집고, 다음에 왼쪽 젓가락을 집도록 하면 아무도 굶어죽지 않을 것이다.

- **풀이**

정답은 **①**.

확실히 하려면 한 명씩 젓가락을 집고 국수를 먹는다는 가정이 성립되어야한다. 1번을 제외한 2, 3, 4번은 보장할 수 없는 상황들이다.

***

### 🌱 2
- **문제**

> Counting Semaphore에 대한 설명으로 가장 틀린 것은?   
>
> 1) 여러 프로세스 간의 동기화 문제를 해결하는 데 사용할 수 있다.   
2) 여러 프로세스의 상호 배제(Mutual Exclusion)를 지원하기 위해서 세마포어의 초기값을 n으로 지정할 수 있다.   
3) 주로 사용 가능한 자원의 인스턴스가 여러 개인 경우에 카운팅 세마포어를 사용할 수 있다.   
4) 카운팅 세마포어를 사용하면 Deadlock과 Starvation 문제도 해결할 수 있다.

- **풀이**

정답은 **④**.

세마포어는 잘못 사용할 경우 데드락 혹은 기아 문제가 발생할 수 있으며 경우에 따라 심해질 수 있다.

***

### 🌱 3
- **문제**

> 다음 중 동기화 문제 해결을 위한 Monitor 방법에 대한 설명으로 가장 옳지 않은 것은?
>
> 1) 모니터 내부의 자원(변수)에 접근하고자 하는 프로세스는 반드시 모니터 내부의 함수를 호출해야 한다.   
2) 모니터 내부의 함수를 사용하는 프로세스는 상호 배제(Mutual Exclusion)가 보장된다.   
3) 모니터에서는 동기화를 위해 wait()과 signal()을 사용하고, signal() 함수를 호출하면 모니터 큐에서 대기한다.   
4) 자바에서는 모니터락을 지원하므로 synchronized 키워드로 간단하게 임계 구역을 지정할 수 있다.

- **풀이**

정답은 **③**.

signal() 함수를 호출하면 대기 상태가 아닌 실행 상태로 전환된다.

***

### 🌱 4
- **문제**

> 주니온은 서울 출장을 가기 위해 KTX 열차표를 온라인으로 발매했는데, 해당 좌석에 가보니 동일한 시간에 동일한 좌석으로 발매된 열차표를 가진 승객이 있었다. 이 경우 KTX 온라인 발권 시스템에 어떤 문제가 발생했다고 보는 것이 가장 합리적일까? 
>
> 1) 좌석표 데이터에 대한 접근을 할 때 Mutual Exclusion을 제대로 보장해 주지 못했을 것이다.   
2) 여러 개의 발권 스레드가 Deadlock에 빠져 Progress 조건을 만족하지 못했을 것이다.   
3) 주니온의 발권 처리 프로세스의 우선순위가 낮아 Starvation이 발생했을 것이다.   
4) 발권 처리 과정에서 Bounded Waiting을 하느라 중복 발행을 하게 되었을 것이다.

- **풀이**

정답은 **①**.

해당 좌석에 동시에 두 프로세스가 접근하여 처리했기 때문에 발생한 문제이다. 상호 배제의 특성을 보장하지 못하면 데이터 불일치가 발생한다.

***

### 🌱 5
- **문제**

> Dijkstra가 제안한 동기화 문제에 대한 소프트웨어 솔루션으로, 경쟁 상황이 발생하는 임계 영역을 가지는 여러 개의 프로세스에 대해 상호 배제를 보장하는 두 개의 연산인 P()와 V()를 제공하는 해결책을 무엇이라고 할까?
>
> 1) 뮤텍스   
2) 세마포어   
3) 모니터   
4) 라이브니스

- **풀이**

정답은 **②**.

여러 개의 프로세스에 대해 상호 배제를 보장하는 해결책은 세마포어이다.

***

### 🌱 6
- **문제**

> Multi-Level Feedback Queue 스케줄링 알고리즘은 여러 개의 우선순위가 다른 Ready Queue를 사용하여 하나의 Ready Queue에서 처리를 마치지 못한 프로세스를 우선순위가 더 높은 Ready Queue에 Feedback 시켜줌으로서 우선순위를 높여준다. 이것을 우리는 노화(Aging) 기법이라고 부르기도 한다. Aging 기법이 필요한 이유와 가장 관련이 높은 것은?
>
> 1) Mutual Exclusion   
2) Progress   
3) Bounded Waiting   
4) Critical Section

- **풀이**

정답은 **③**.

Aging 기법은 우선순위가 낮은 프로세스가 무한한 대기를 하지 않기 위하여 우선순위를 높여주는 것이다. 이와 관련된 것은 기아 문제이며, 이를 피하기 위해 한정된 대기 즉 Bounded Waiting의 특성이 관련이 가장 높다.

***

### 🌱 7
- **문제**

> Process Synchronization에 대한 설명으로 가장 옳은 것은?
>
> 1) 어떤 Scheduler를 사용하더라도 Race Condition은 기피할 수 없으므로 Mutual Exclusion, Progress, Bounded Waiting을 고려해서 Critical Section Problem을 해결해야 한다.   
2) Semaphore는 정수 변수로서 오로지 wait과 signal 연산만을 통해서 접근할 수 있으며 반드시 0으로 초기화를 해 주어야 한다.   
3) Spinlock은 Busy Waiting을 하며 Lock을 획득하는 방식이므로 멀티코어 시스템에서는 비효율적이므로 사용하면 좋지 않다.   
4) Monitor는 모니터 내부에서는 항상 하나의 프로세스만이 활성화되도록 보장해 주므로, 프로그래머가 동기화 제약 조건을 명시적으로 프로그래밍해야 할 필요가 없다는 장점이 있다.

- **풀이**

정답은 **④**.

1) Race Condition은 CPU 스케줄러가 아닌 프로세스(혹은 스레드) 간의 통신 중 나타날 수 있는 문제이므로 관련없다.   
2) 세마포어의 초기화는 다양한 값으로 가능하다.   
3) 스핀락은 오히려 멀티코어 시스템에서 유용하다. 하나의 스레드가 하나의 코어에서 작업을 수행하는 동안 다른 스레드가 하나의 프로세싱 코어에서 회전하며 대기하게 되면 전환 속도가 훨씬 빠르기 때문이다.

***

### 🌱 8
- **문제**

> Semaphore를 사용했을 때 발생할 수 있는 문제점으로 옳은 것을 모두 고르시오.
>
> 1) 반드시 Spinlock(Busy Waiting)이 발생한다.   
2) Deadlock & Starvation이 발생할 수 있다.   
3) wait(), signal()을 순서에 맞게 사용하지 않으면 Mutual Exclusion 문제가 발생할 수 있다.   
4) Mutual Exclusion 문제는 절대로 발생할 수가 없다.

- **풀이**

정답은 **②, ③**.

세마포어는 반드시 스핀락이 발생하는 것은 아니며 wait()과 signal()을 잘못 사용할 경우 상호 배제를 보장할 수 없는 것은 물론 데드락과 기아 현상이 발생할 수 있다.

***

### 🌱 9
- **문제**

> Peterson's algorithm과 같은 Software적인 해결책은 SMP (Symmetric Multiprocessor System) 환경과 같은 Modern Computer System에서는 제대로 작동할 수 있다는 보장이 없다. 그 해결책으로 하드웨어적인 Instruction이나 Mutex, Semaphore와 같은 소프트웨어적인 API를 사용하여 프로세스 간 동기화를 보장하는 방법을 사용한다.
>
> ① 이런 기법들은 모두 이것을 획득하도록 하여 Critical Section을 보호한다. 이것 혹은 이것을 이용하는 기법을 무엇이라 하는가?   
② 동기화를 위한 이런 방법을 구현하는 데 있어서, test_and_set()이나 compare_and_swap()과 같은 하드웨어 Instruction이나 Mutex/Semaphore의 wait(), signal() 구현은 모두 이 성질을 만족해야만 한다. 이 성질을 무엇이라 하는가?
>
> 1) Locking, Atomicity   
2) Busy Waiting, Atomicity   
3) Locking, Integrity   
4) Busy Waiting, Integrity

- **풀이**

정답은 **①**.

1. 임계 영역을 Locking하여 보호한 다음, 열쇠를 획득하는 것과 비슷한 방식으로 권한을 획득하여 임계 영역에 진입할 수 있다.   
2. 모든 하드웨어 Instruction은 더이상 쪼개지지 않는 원자성(Atomicity)을 가져야한다.

***

### 🌱 10
- **문제**

> 다음 Java 코드 중에서 Mutual Exclusion이 보장되기 어려운 코드를 모두 고르시오.
>
> 1)   
> ```java
> class Counter {
>     public static int count = 0;
>     synchronized public static void increment() {
>         Counter.count++;
>     }
> }
> ```
> 2)   
> ```java
> class Counter {
>     public static int count = 0;
>     public static Object obj = new Object();
>     public static void increment() {
>         synchronized (obj) {
>             Counter.count++;
>         }
>     }
> ```
> 3)   
> ```java
> class Counter {
>     public static int count = 0;
>     public static void increment() {
>         synchronized (this) {
>             Counter.count++;
>         }
>     }
> }
> ```
> 4)   
> ```java
>class Counter {
>    public static int count = 0;
>    public static void increment() {
>        synchronized (new String("lock")) {
>            Counter.count++;
>        }
>    }
>}
> ```

- **풀이**

정답은 **③, ④**.

~~설명해 주실 분 구함니다.~~

***

## 👻 글을 마치며
이번 시간에는 동기화 툴 두 번째 시간에 배웠던 내용을 토대로 퀴즈를 풀어보았다. 뮤텍스, 세마포어, 모니터... 너무 헷갈린다 ㅠㅠㅠ 코드를 직접 따라치면서 감을 익히는 실습 시간을 따로 내야 할 것 같다. (스터디 끝나면 천천히 해야짓..)

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   