---
title: "[Operating System] #4주차 - Synchronization Examples"
excerpt: "운영체제 공룡책 강의 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, synchronization, examples]

permalink: /operating-system/os-lecture-4-1/

toc: true
toc_sticky: true

date: 2023-06-26 18:08:23+0900
last_modified_at: 2023-06-26 18:08:27+0900
published: true
---

## 👻 동시성 제어의 고전적 문제들
동시성을 제어할 때 발생하는 문제는 다음과 같다.

- **Bounded-Buffer Problem**
    - Producer-Consumer Problem
- **Readers-Writers Problem**
- **Dining-Philosophers Problem**
    - 철학자들의 저녁 식사

***

### 🌱 Bounded-Buffer Problem
Producer-Consumer 사이에서 한정된 버퍼를 사용했을 때 발생하는 문제이다.

생산자는 버퍼를 가득 채우는 것이 목표이고, 소비자는 버퍼를 모두 비우는 것이 목표이다.

```c++
int n;
semaphore mutex = 1;
semaphore empty = n;
semaphore full = 0;
```

해결 방법 중 **Binary Semaphore(Mutex)**는 초기화를 1로두며 n개의 세마포어 변수를 사용하는 **Counting Semaphore**는 초기화를 위와 같이 empty는 n, full은 0으로 두어 작업을 수행한다.

- **생산자 프로세스의 구조**

```c++
while (true) {
    ...
    /* produce an item in next_produced */
    ...
    wait(empty);
    wait(mutex);
    ...
    /* add next_produced to the buffer */
    ...
    signal(mutex);
    signal(full);
}
```

- **소비자 프로세스의 구조**

```c++
while (true) {
    wait(full);
    wait(mutex);
    ...
    /* remove an item from buffer to next_consumed */
    ...
    signal(mutex);
    signal(empty);
    ...
    /* consume the item in next_consumed */
    ...
}
```

위의 대칭구조가 이루어지지 않으면 동기화 문제가 발생할 수 있다.

***

### 🌱 Readers-Writers Problem
대부분의 동기화는 기본적으로 공유된 데이터에 대해 Read와 Write가 수행되는 Race Condition을 가정한다. 해당 문제도 이 부분까지는 동일하나 어떤 프로세스가 데이터를 읽기만 하거나 쓰기만 하는 것을 의미한다.

> 💡 Database의 경우 해당 문제가 발생할 수 있다.   
- **Reader** : SELECT를 수행하는 프로세스
- **Writer** : INSERT, UPDATE, DELETE 등을 수행하는 프로세스

읽기만 하는 리더와 읽고 쓰는 라이터가 동시성을 가지고 작업을 수행하면 어떻게 될까?   
👉 리더는 Data Inconsistency를 일으키지 않음.   
👉 라이터는? 발생 가능성 다수~

아래는 두 가지의 문제 상황과 해결법이다.

- The **First** Readers-Writers Problem
    - 어떠한 리더들도 라이터가 대기 상태에 있다고 해서 대기하면 안 된다.
    - 공평하게 락을 받은 프로세스가 먼저 작업을 수행할 수 있도록 해야함
    - 해결법
        - 아래의 세마포어 구조는 리더 프로세스에 대한 것이다.
        - ``` rw_mutex ```는 리더, 라이터 공용
        - ``` read_count ```가 0이면 라이터가 진입 가능   
    ```c++
    semaphore rw_mutex = 1;
    semaphore mutex = 1;    // 임계 영역 보호
    int read_count = 0;     // 리더의 개수
    ```

```c++
// The structure of a writer process
while (true) {
    wait(rw_mutex);
    ...
    /* writing is performed */
    ...
    signal(rw_mutex);
}

// The structure of a reader process
while (true) {
    wait(mutex);
    read_count++;
    if (read_count == 1)
        wait(rw_mutex);
    signal(mutex);
    ...
    /* reading is performed */
    ...
    wait(mutex);
    read_count--;
    if (read_count == 0)
        signal(rw_mutex);
    signal(mutex);
}
```

- The **Second** Readers-Writers Problem
    - 라이터가 액세스 중일 때 리더는 접근할 수 없다.

위의 두 문제 상황은 기아 현상을 일으킬 수 있다.

- **해결법**
    - 하나의 라이터가 임계 영역에 있을 때, n개의 리더는 기다리게 되는데,
        - 하나의 리더는 ``` rw_mutex ```에서 대기
        - n - 1개의 리더는 ``` mutex ```에서 대기
    - 각 해결법은 스케줄러가 정한다.
    - 기본적으로 **Reader-Writer Locks**를 제공해 줌
        - 이를 이용해 해결
        - 여러 개의 프로세스가 락을 획득하면 Read mode
        - 하나의 프로세스가 락을 획득하면 Write mode

***

## 👻 철학자들은 왜 굶어 죽었을까?
마지막 동시성 문제인 **Dining-Philosophers Problem**의 개념은 다음과 같다.

- 다섯명의 철학자가 Thinking과 Eating을 수행하며 평생을 보낸다.
- 젓가락은 총 다섯개이며, 이를 모두 공유하게 된다.
- 각 철학자들은 자신의 가까이에 있는 두 개의 젓가락에만 접근할 수 있다.
- 모두 굶어 죽지 않고 평화롭게 먹으려면 어떻게 해야할까?

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-4-1/dining.PNG)   

***

### 🌱 Semaphore Solution
각 젓가락에 세마포어를 걸어주면 간단하게 해결할 수 있다.

- 한 철학자가 젓가락을 획득 (``` wait() ```)
- 젓가락을 내려놓음 (``` signal() ```)

```c++
semaphore chopstick[5];

while (true) {
    wait(chopstick[i]);
    wait(chopstick[(i+1) % 5]);
    ...
    /* eat for a while */
    ...
    signal(chopstick[i]);
    signal(chopstick[(i+1) % 5]);
    ...
    /* think for a while */
    ...
}
```

위 해결법은 **상호 배제를 보장**하지만, **데드락**과 **기아 현상**이 쉽게 발생한다는 단점이 있다.   
👉 한 명이 젓가락을 놓지 않거나 잡지 않을 때를 생각할 수 있음

***

#### 🪐 데드락을 해결하는 방법
- 젓가락 수는 그대로일 때, 철학자들의 숫자를 **네 명으로 제한**하면 해결할 수 있을 것이다.
- **양쪽 젓가락이 놓여있을 때만** 잡을 수 있도록 하면 될 것이다.
- Asymmetric Solution
    - 홀수 번호를 부여받은 철학자들은 먼저 왼쪽을 집은 후 오른쪽을 집도록 한다.
    - 짝수 번호를 부여받은 철학자들은 먼저 오른쪽을 집은 후 왼쪽을 집도록 한다.

위 해결법으로 데드락은 해결되겠지만 기아 현상은 보장할 수 없음... 대신 발생하면 최대한 빨리 피하는 식으로 구현을 하도록...

***

### 🌱 Monitor Solution
양쪽의 젓가락이 놓여져 있을 때만 철학자가 집을 수 있도록 한다면?

- 세 개의 상태로 나누자
    - Thinking
    - Hungry (중간 단계)
    - Eating

Hungry 상태에서 젓가락을 집으면 Eating으로 넘어가고, 집지 못하면 Waiting하게 된다.

철학자들은 자기 상태를 Eating 상태로 변경할 수 있는데, 두 이웃 철학자들이 Eating 상태가 아닐 때만 가능하다.

모니터로 위의 상황을 구현하려면 **Condition Variable**이 필요하다.

***

#### 🪐 해결법
- 젓가락의 배포를 담당하는 모니터를 구현
    - **DiningPhilosopher**
- 젓가락을 집는 행위
    - pickup()
- 젓가락을 내려놓는 행위
    - putdown()

> 💡 이 해결법 또한 상호 배제와 데드락은 보장되지만 기아 현상은 여전히 일어날 수 있다.

```c++
monitor DiningPhilosophers
{
    enum {
        THINKING, 
        HUNGRY, 
        EATING 
    } state[5];

    void pickup(int i) {
        /* critical section */
        state[i] = HUNGRY;
        test(i);
        if (state[i] != EATING)
            self[i].wait();
    }

    void putdown(int i) {
        /* critical section */
        state[i] = THINKING;
        test((i + 4) % 5);  // 왼쪽
        test((i + 1) % 5);  // 오른쪽
    }

    // If I'm hungry and my neighbors are not eating,
    // then let me eat.
    void test(int i) {
        if ((state[(i + 4) % 5] != EATING) && (state[i] == HUNGRY) && (state[(i + 1) % 5] != EATING)) {
            state[i] = EATING;
            self[i].signal();
        }
    }

    initialization_code() {
        for (int i = 0; i < 5; i++)
            state[i] = THINKING;
    }
}
```

***

## 👻 대안적 접근법
- **Thread-Safe Concurrent Applications**
    - 동시성을 가진 어플리케이션은 뮤텍스 락, 세마포어, 모니터 등을 사용한 멀티코어 시스템에서 아주 좋은 퍼포먼스를 가진다.
    - 하지만, 경쟁 상태와 데드락 같은 Liveness 위험도가 증가하게 된다.
    - 대안으로 Thread-Safe 디자인된 어플리케이션이 있다.

1. **Transactional Memory** : Atomic Operation을 이용한 단위 메모리
2. **OpenMP** : ``` #pragma omp parallel ```을 이용, 임계 영역 지정
3. **Functional Programming Language** : 함수형 프로그래밍

> 💡 **Imperative Programming Language(명령형 프로그래밍)**   
👉 함수형 프로그래밍과 반대되는 의미의 프로그래밍. 명령에 따라 CPU를 할당하여 작업을 수행하는 방식. 독립적이지 않아 문제 발생 확률이 높다.

***

## 👻 글을 마치며
이번 시간에는 다양한 동시성 문제에 대한 총 복습을 해보았다. 생산자-소비자 문제에 이어 Readers-Writers 문제, 그리고 철학자들의 저녁식사 문제를 포함해 총정리 해보는 시간이었다. 확실히 예시를 드니까 이해가 잘 됐고 앞에서 배웠던 개념들을 복습하다보니 개념 정리가 잘 됐던 것 같다.

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   