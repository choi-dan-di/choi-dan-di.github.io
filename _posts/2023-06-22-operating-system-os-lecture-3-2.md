---
title: "[Operating System] #3주차 - Synchronization Tools (2)"
excerpt: "운영체제 공룡책 강의 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, synchronization, tools, mutex, semaphore, monitor, liveness]

permalink: /operating-system/os-lecture-3-2/

toc: true
toc_sticky: true

date: 2023-06-22 16:55:30+0900
last_modified_at: 2023-06-22 16:55:33+0900
published: true
---

## 👻 Higher-level Software tools
CSP를 해결하는 더 고급 레벨의 소프트웨어 솔루션은 다음과 같다.

- **Mutex Locks**
    - Mutual Exclusion에 대해 조금 더 강화된 방법
    - 가장 간단한 Synchronization Tool
    - 2개의 프로세스(혹은 스레드) 제어가 가능하다.
- **Semaphore**
    - N개의 프로세스(혹은 스레드) 제어가 가능하다.
- **Monitors**
    - Mutex와 Semaphore의 단점을 보완한 방법
- **Liveness**
    - Deadlock을 해결할 수 있다.

***

### 🌱 Mutex Locks
> **Mutex** : **Mut**ual **Ex**clusion(상호 배제)

임계 영역을 **보호**하여 경쟁 상태를 방지한다. **lock**을 사용하며 lock이 걸린 상태의 프로세스는 임계 영역에 들어가기 전 **열쇠를 획득하는 과정**인 **Acquire**이 필요하다. 마찬가지로 임계 영역을 빠져나올 때 **Release** 과정 또한 필요하다.

Mutex Locks는 두 함수와 하나의 변수를 이용한다.

- **Functions**
    - ``` acquire() ```
    - ``` release() ```
- **Variable**
    - ``` available ```(Boolean)이라는 것을 이용하여 상태를 확인

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-3-2/mutex-locks.PNG)   

- ``` acquire() ```과 ``` release() ```의 정의

```c++
// acquire
acquire() {
    while (!available)
        ;   /* busy wait */

    available = false;
}

// release
release() {
    available = true;
}
```

이 방법은 간단하지만 Busy Waiting이 생긴다는 단점이 있다.

- **Busy Waiting**
    - 어떤 프로세스가 임계 영역에 들어가기 위해 **무한 루프(**``` acquire() ```**)** 상태가 되는 것
    - Single-core일 때 CPU 사이클을 쓸데 없이 소모한다는 심각한 단점이 발생할 수 있다.

Busy Waiting 하면서 Mutex Lock을 하는 것을 **Spinlock**이라고 한다.

- **Spinlock**
    - 언뜻 보면 자원 소모가 커보이지만 Context Switch가 일어나지 않아 유용하다.
    - 스핀락을 하지 않으려면 Wait Queue를 이용해야함(Context Switching)

***

### 🌱 Semaphore
> **Semaphore** : 신호장치, 신호기

N개짜리 프로세스(혹은 스레드)가 임계 영역을 공유할 때 사용하는 동기화 방법 중 하나이다. 세마포어(S)는 **Integer Variable**이며 초기화를 어떻게 해주느냐에 따라 값이 달라진다.

두 개의 표준 아토믹 연산을 사용하는데 ``` wait() ```과 ``` signal() ```, 혹은 ``` P() ```와 ``` V() ```를 사용한다.

> P()와 V()는 다익스트라에 의해 처음 소개되었다.   
👉 **P**roberen(to test) and **V**erhogen(to increment)

- ``` wait() ```과 ``` signal() ```의 정의

```c++
// 여기서 S는 세마포어를 의미한다.

// wait
wait(S) {
    while (S <= 0)
        ;   /* busy wait */

    S--;
}

// signal
signal(S) {
    S++;
}
```

마찬가지로 여기서 두 함수 내의 수정 가능한 세마포어의 값은 **원자성**을 가져야 한다.

- **Binary and Counting Semaphores**
    - **Binary Semaphore**
        - 파라미터 S가 0과 1로만 이루어져있음
        - 뮤텍스와 비슷하다. (2개의 프로세스에 한해 동작)
    - **Counting Semaphore**
        - 여러개의 인스턴스를 가진 자원에 사용 가능
        - 카운팅 세마포어는 어떻게 쓸까? 👉 리소스의 개수만큼 세마포어를 초기화한다.
        - 사용할 때 ``` wait() ``` : 카운트 감소
        - 안할 때 ``` signal() ``` : 카운트 증가

> 💡 카운팅 세마포어의 경우 열쇠 꾸러미로 비유할 수 있다. N개만큼의 열쇠가 하나의 열쇠 꾸러미에 있다고 가정할 때,   
- ``` wait() ```은 열쇠 꾸러미의 열쇠를 빼가는 느낌
- ``` signal() ```은 열쇠 꾸러미에 열쇠를 추가하는 느낌

세마포어도 마찬가지로 Busy Waiting 문제가 발생한다. 이는 프로세스가 ``` wait() ``` 혹은 ``` signal() ```을 실행할 때, while문을 도는 대신 자기 자신을 **Waiting Queue** 혹은 **Ready Queue**로 이동 시킴으로써 해결할 수 있다.

Waiting Queue로 가면 ``` sleep() ```, Ready Queue로 가면 ``` wakeup(process) ```을 실행하여 프로세스를 재우거나 깨울 수 있다.

- ``` sleep() ```과 ``` wakeup(process) ```를 이용하여 구현한 세마포어

```c++
typedef struct {
    int value;
    struct process *list;
} semaphore;

// wait
wait(semaphore *S) {
    S->value--;
    if (S->value < 0) {
        /* add this process to */ S->list;
        sleep();
    }
}

// signal
signal(semaphore *S) {
    S->value++;
    if (S->value <= 0) {
        /* remove a process P from */ S->list;
        wakeup(P);
    }
}
```

***

### 🌱 Monitors
세마포어는 편리하고 효과적이긴 하지만 **타이밍 에러**가 많이 발생한다. ``` wait() ``` 👉 ``` signal() ``` 순서를 지키지 않으면 임계 영역에 프로세스(혹은 스레드) 여러 개가 들어가는 문제가 발생할 수 있다. (같은 걸 쓴다거나, 안 쓴다거나 등의 문제들)

- **여러가지 문제 상황들**
    - **wait과 signal의 순서를 반대로 구현하였을 경우 : Mutual Exclusion 보장 X**   
    ```c++
    signal(mutex);
    ...
    /* critical section */
    ...
    wait(mutex);
    ```
    - **signal을 wait으로 대체하였을 경우 : Bounded Waiting 보장 X**   
    ```c++
    wait(mutex);
    ...
    /* critical */
    ...
    wait(mutex);
    ```

이렇게 뮤텍스나 세마포어를 잘못 써서 발생한 문제를 해결하기 위해 간단한 동기화 툴을 사용해야한다. 이는 좀 더 하이레벨 언어 구조를 사용하거나, **모니터 타입**을 사용하는 것으로 대체할 수 있다.

> 하이레벨 일수록 사람 친화적이라 구현의 난이도가 줄어들겠지요?

**Monitor(모니터)**는 하나의 주요한 하이레벨 동기화 구조이다.

- **ADT**
    - **Abstract Data Type**
    - 데이터와 그 데이터에 대한 연산을 결합한 추상화된 데이터 타입
    - 캡슐화
    - C++의 클래스 개념과 비슷

모니터도 ADT 중 하나이며 이는 프로그래머가 정의한 연산자도 포함한다. 변수를 정의하고 이를 포함한 함수를 이용해 모니터를 만들며 각 프로세스의 상태를 알 수 있는 **상태 변수(Condition Variables)**도 추가해줘야한다.

> 💡 **Condition Variables**   
프로세스(혹은 스레드) 간의 동기화를 위해 사용된다. ``` wait() ```과 ``` signal() ```같은 상태를 담고 있는 변수이다.   
>
> - **상태 변수 사용 예**   
>
> ```c++
> condition x, y;
> 
> x.wait();
> 
> x.signal();
> ```

- **Monitor Sudocode**

```c++
monitor
{
    /* shared variable declarations */
    function P1 () {}
    function P2 () {}
    ...
    function Pn () {}
    initialization_code () {}
}
```

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-3-2/monitor.PNG)   

모니터는 자바의 동기화와 형식이 비슷하다. 참고로 자바는 모니터와 비슷한 메커니즘을 제공하며 **monitor-lock** 혹은 **intrinsic-lock**이라고 부른다.

기본 언어로 구현하려면 다음과 같은 것들을 사용한다.

- ``` synchronized ``` 키워드
    - 임계 영역에 해당하는 코드 블록을 선언할 때 사용하는 자바 키워드
    - 해당 임계 영역에는 **모니터락**을 획득해야 진입 가능
    - 모니터락을 가진 객체 인스턴스를 지정할 수 있음
    - 메소드에 선언하면 메소드 코드 블록 전체가 임계 영역으로 지정됨
        - 이 때, 모니터락을 가진 객체 인스턴스는 ``` this ``` 객체 인스턴스임   

    ```java
    synchronized (object) {
        // critical section
    }

    public synchronized void add() {
        // critical section
    }
    ```

- ``` wait() ```과 ``` notify() ``` 메소드
    - ``` java.lang.Object ``` 클래스에 선언됨 : 모든 자바 객체가 가진 메소드
    - 스레드가 어떤 객체의 ``` wait() ``` 메소드를 호출하면
        - 해당 객체의 모니터락을 획득하기 위해 **대기 상태로 진입**함
    - 스레드가 어떤 객체의 ``` notify() ``` 메소드를 호출하면
        - 해당 객체 모니터에 대기중인 스레드 **하나**를 깨움
    - ``` notify() ``` 대신에 ``` notifyAll() ``` 메소드를 호출하면
        - 해당 객체 모니터에 대기중인 스레드 **전부**를 깨움

***

### 🌱 Liveness
뮤텍스와 세마포어는 데드락과 기아 문제를 해결할 수 없다. 오히려 세마포어는 증가한다. 이 두 문제를 해결하는 방식이 바로 **Liveness**이다. Liveness는 프로세스가 실행 사이클 동안의 진행을 보장해줘야 하는 특성을 말한다. 단, Liveness Failure라면 **데드락**과 **Priority Inversion(우선순위 역전)** 상황이 발생할 수 있다.

***

#### 🪐 Deadlock
**교착 상태**를 의미한다. 다른 프로세스가 영향을 줘서 발생하는 이벤트로, 한 프로세스가 Waiting 상태에서 영영 상태를 바꾸지 못 하는 상황을 말한다.

- **데드락 발생의 한 예시**

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-3-2/deadlock.PNG)   

***

#### 🪐 Priority Inversion
**Priority Inversion(우선순위 역전)** 상황은 높은 우선 순위를 가진 프로세스가 낮은 프로세스에게 밀리는 현상을 말한다.

높은 우선순위의 프로세스가 커널 데이터를 읽거나 수정하려는 요청을 했을 때, 낮은 우선순위의 프로세스가 실행 중이라면 작업이 끝날 때까지 기다려야한다. 이 부분에서 우선순위 역전이 발생한다.

- **해결 방법**
    - **Priority-Inheritance(우선순위 상속)**
        - 점유하고 있는 자원에 대한 우선순위를 상속받아 대기 상태에 빠지지 않고 자원에 접근이 가능하도록 해줌
        - [👉 정리 잘 된 블로그로 이해합쉬다 ... 👈](https://j-i-y-u.tistory.com/21)

***

## 👻 글을 마치며
이번 시간에는 더 고급 레벨의 소프트웨어 솔루션들에 대해 알아보았다. 운영체제 공부하면서 처음 들어보는 단어들이었다. 뮤텍스, 세마포어 등.. (아니면 들었는데 까먹었는지 ㅠ) 개념이 다들 비슷비슷한 것 같은데, 한 방식을 개선하기 위해 나온 것들이라 생각하면 나름의 특징이 보이는 것 같기도 하다. 이런 차이점 찾는 재미가 쏠쏠한 것 같음!

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   