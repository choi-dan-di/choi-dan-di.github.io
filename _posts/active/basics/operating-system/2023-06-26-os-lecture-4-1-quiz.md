---
title: "[Operating System] #4주차 - 퀴즈 풀이"
excerpt: "운영체제 공룡책 Synchronization Examples 퀴즈 풀이 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, synchronization, examples, quiz]

permalink: /operating-system/os-lecture-4-1-quiz/

toc: true
toc_sticky: true

date: 2023-06-26 20:04:17+0900
last_modified_at: 2023-06-26 20:04:21+0900
published: true
---

## 👻 퀴즈 풀이

***

### 🌱 1
- **문제**

> Readers-Writers Problem에서 Reader 프로세스가 다음과 같은 자료 구조를 가지고 있다.
>
> ```c++
> semaphore rw_mutex = 1;
> semaphore mutex = 1;
> int read_count = 0;
> ```
>
> 다음 사례들 중에서 rw_mutex 세마포어를 사용할 필요가 없는 경우로 가장 적절한 것은?
>
> 1) 첫 번째 Reader 프로세스가 임계 구역에 진입할 때.   
2) 마지막 Reader 프로세스가 임계 구역에 진입할 때.   
3) Writer 프로세스가 쓰기 작업을 완료할 때.   
4) 두 번째 Reader 프로세스가 임계 구역에 진입할 때.

- **풀이**

정답은 **④**.

해당 세마포어는 Reader와 Writer 프로세스 모두 사용하는 것으로, 어떠한 모드인지를 나타낸다. 고로 두 번째 Reader는 사용할 필요가 없다.

***

### 🌱 2
- **문제**

> Bounded Buffer Problem의 Consumer Process 구조를 아래와 같이 구현했다.
>
> ```c++
> while (true) {
>     (A) wait(full);
>     (B) wait(mutex);
>     ...
>     /* remove an item from buffer to next_consumed */
>     ...
>     (C) signal(mutex);
>     (D) signal(empty);
> }
> ```
>
> 위 슈도 코드에 대한 설명으로 가장 적절한 것은?
>
> 1) (A)에서 wait(full)을 했을 때 signal(full)을 해 주지 않으므로 Deadlock이 발생할 것이다.   
2) (B)에서 mutex를 wait() 해 주는 것은 Counting Semaphore를 사용했기 때문이다.   
3) (C)에서 signal(mutex)를 호출하는 것은 다른 Consumer 프로세스가 임계 구역에 진입할 수 있게 해 준다.   
4) (D)에서 signal(empty)를 호출했으므로 Producer 프로세스가 임계 구역에 진입할 수 있을 것이다.

- **풀이**

정답은 **④**.

1) signal(full)은 Producer Process에서 해준다.   
2) Binary Semaphore를 사용하였다.   
3) 음..?

***

### 🌱 3
- **문제**

> \<잠자는 TA 문제\>   
주니온 교수는 운영체제 과목을 어려워하는 학생들을 도와줄 수 있는 TA를 한 명 배정하기로 했다.   
TA가 대기하는 IT융복합관 999호는 좁아서 한 명의 TA와 한 명의 학생만 들어갈 수 있다.   
999호 바깥 복도에는 3개의 의자가 있으므로 TA가 한 명을 돕고 있을 때는 여기서 기다릴 수 있다.   
도움을 요청하는 학생들이 없으면 잠이 많은 TA는 항상 졸고 있다.   
TA의 도움이 필요한 학생들은 999호에 도착해서 TA가 졸고 있으면 깨워서 도움을 받을 수 있다.   
만약 이미 도움을 받는 학생이 있으면 빈 의자에서 대기하고, 빈 의자가 없으면 돌아갔다가 다시 와야한다. 
>
> 위 문제에 대해서 동기화 방법을 조율하는 해결책에 대한 설명으로 옳다고 할 수 있는 것을 모두 고르시오.
>
> 1) TA 1명과 도움이 필요한 N명의 학생들을 프로세스, 혹은 스레드로 구현하면 좋겠다.   
2) TA가 졸고 있을 때 도착한 학생은 조교를 깨우기 위해 뮤텍스 락을 이용해 깨우면 좋겠다.   
3) 빈 의자에 앉을 때는 의자의 수로 초기화된 카운팅 세마포어를 이용하면 좋겠다.   
4) 빈 의자가 가득찼을 때, 학번이 더 높은 학생이 도착하면 학번이 낮은 학생을 선점(Premption)하도록 하면 Deadlock 문제가 발생할 수 있다.   
5) TA의 도움을 받은 학생이 나가면서 바깥 의자에 대기하는 학생들에게 끝났다고 알려주지 않으면 Starvation 문제가 발생할 수 있다.

- **풀이**

정답은 **④, ⑤**.

각각 Starvation, Deadlock이 발생할 수 있다.

***

### 🌱 4
- **문제**

> 강의자료에 제공된 Dining Philosophers Problem에 대한 Pthread / Java 솔루션에 대한 설명으로 옳다고 할 수 없는 것을 모두 고르시오.
>
> 1) 철학자들의 저녁 식사 문제의 Pthread 솔루션은 Pthread에서 제공하는 Monitor Lock을 사용하고 있다.   
2) pthread_cond_wait() 함수가 호출되기 전에 Condition 변수와 연계된 Mutex 락이 먼저 잠겨져야 한다.   
3) 공유 데이터를 변경하는 스레드는 pthread_cond_signal() 함수를 호출하여 Condition 변수가 참이 되기를 기다리는 스레드에게 신호를 보낸다.   
4) Java 솔루션에서는 DiningPhilosopherMonitor 클래스가 Monitor 기능을 제공하고 있으며, Philosopher 스레드의 개수만큼의 Monitor를 생성하여 각 Philosopher 객체가 Monitor Instance 하나씩을 가지고 있다.   
5) Java 솔루션에서는 n명의 철학자 스레드를 생성하기 위해서 카운팅 세마포어를 정의하고 초기값으로 n을 설정해주었다.

- **풀이**

정답은 **①, ④, ⑤**.

모니터 락은 자바 솔루션에서 제공하는 기능이며 모니터 인스턴스는 하나만 가진다. 또한 자바 솔루션에서 n명의 철학자 스레드를 생성할 땐 Runnable 인터페이스를 구현한 클래스를 이용하여 생성한다.

***

### 🌱 5
- **문제**

> Thread-safe Concurrent Application을 개발하기 위한 여러 가지 대안에 대한 설명으로 가장 틀린 것은?
>
> 1) Transactional Memory를 통해 메모리 읽기와 쓰기 연산을 원자적 연산(Atomic Operation)으로 만들 수 있다.   
2) OpenMP에서 #pragma omp critical 컴파일러 디렉티브로 임계 구역을 지정해 줄 수 있다.   
3) 함수형 프로그래밍 언어들은 명령형 프로그래밍 언어와 달리 상태를 유지하지 않으므로 경쟁 조건이나 교착 상태가 아예 발생하지 않는다.   
4) 여러 개의 CPU가 존재하는 다중 코어 시스템에서는 각 코어에서 별도의 프로세스(혹은 스레드)가 독립적으로 실행되기 때문에 동기화 문제는 크게 문제가 되지 않는다.

- **풀이**

정답은 **④**.

멀티코어 시스템일수록 동기화 문제에 신경써야한다.

***

## 👻 글을 마치며
자바 쪽은 제대로 보지 않아서 문제 푸는 데 헷갈렸던 부분이 많았던 것 같다. ~~자바를 ㅠㅠ 꼭 ㅠㅠ 알아야 할까요? ㅠㅠㅠ~~ 그거 제외하곤 큰 어려움은 없었던 것 같다.

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   