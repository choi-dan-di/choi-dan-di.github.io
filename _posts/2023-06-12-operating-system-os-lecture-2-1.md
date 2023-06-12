---
title: "[Operating System] #2주차 - Thread & Concurrency"
excerpt: "운영체제 공룡책 강의 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, thread, concurrency]

permalink: /operating-system/os-lecture-2-1/

toc: true
toc_sticky: true

date: 2023-06-12 18:34:48+0900
last_modified_at: 2023-06-12 18:34:52+0900
published: true
---

## 👻 Thread
스레드는 **LWP(Lightweight Process)**라고도 하며, CPU Utilization(점령화)의 기본적인 단위이다.

> 스레드는 프로세스 내부에 존재함!

프로세스 ID인 pid 안의 스레드 ID인 tid가 CPU를 점령한다고 볼 수 있고, **PC(Program Counter), Register Set, Stack가 스레드 단위로 생긴다.**

하나의 프로세스는 하나 이상의 스레드를 가지는데, 둘 이상의 스레드를 가지면 **멀티스레드(Multithread)** 프로그래밍이 가능하다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-1/thread.PNG)   

- **멀티스레드 프로그래밍의 이점**
    1. **Responsiveness(응답성)**
        - 여러 스레드를 사용하기 때문에 Non-Blocking으로 작업 처리가 가능하다.
    2. **Resource Sharing(리소스 공유)**
        - 이미 Code, Data를 공유하기 때문에 Shared Memory나 Message Passing 방식보다 리소스 공유가 쉽다.
    3. **Economy(경제적)**
        - 프로세스를 생성하는 것보다 경제적이다.
        - 스레드 교환은 Context Switching보다 오버헤드(Overhead)가 적다.
    4. **Scalability(확장성)**
        - 하나의 프로세스 생성으로 여러개의 스레드 생성이 가능하다.

> 💡 **Overhead** : 어떤 처리를 하기 위해 들어가는 간접적인 처리 시간·메모리 등을 의미한다.

**Client-Server System**을 생각해보면 멀티스레딩이 어떠한 방식으로 동작되는지 이해하기 쉽다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-1/thread2.PNG)   

1. 클라이언트의 요청
2. 서버는 하나의 스레드를 생성해 요청된 작업을 수행
3. 동시에 클라이언트의 추가적인 요구를 받을 준비를 재개

***

### 🌱 Thread Library
자바에서의 스레드는 프로그램 실행의 기능적인 모델을 의미한다. 자바는 스레드의 생성 및 관리를 위해 다양한 기능을 제공한다.

또한, 스레드는 세 가지 방법으로 생성이 가능하다.

**1. Thread 클래스 상속받기**

```java
class MyThread1 extends Thread {
    public void run() {
        try {
            while (true) {
                System.out.println("Hello, Thread!");
                Thread.sleep(500);
            }
        }
        catch (InterruptedException ie) {
            System.out.println("I'm interrupted");
        }
    }
}

public class ThreadExample1 {
    public static final void main(String[] args) {
        MyThread1 thread = new MyThread1();
        thread.start();
        System.out.println("Hello, My Child!");
    }
}
```

**2. Runnable 인터페이스 구현하기**

```java
class MyThread2 implements Runnable {
    public void run() {
        try {
            while (true) {
                System.out.println("Hello, Runnable!");
                Threadm.sleep(500);
            }
        }
        catch (InterruptedException ie) {
            System.out.println("I'm interrupted");
        }
    }
}

public class ThreadExample2 {
    public static final void main(String[] args) {
        Thread thread = new Thread(new MyThread2());
        thread.start();
        System.out.println("Hello, My Runnable Child!");
    }
}
```

**3. Runnable 람다 표현식 사용하기**

사실상 이 방법은 Runnable 인터페이스를 구현하는 것과 (람다를 사용했다는) 구현 방법만 다를 뿐 같은 의미이다.

```java
public class ThreadExample3 {
    public static final void main(String[] args) {
        Runnable task = () -> {
            try {
                while (true) {
                    System.out.println("Hello, Lambda Runnable!");
                    Thread.sleep(500);
                }
            }
            catch (InterruptedException ie) {
                System.out.println("I'm interrupted");
            }
        };
        Thread thread = new Thread(task);
        thread.start();
        Sytem.out.println("Hello, My Lambda Child!");
    }
}
```

**부모 스레드의 대기**는 ``` join() ```, **스레드의 종료**는 ``` interrupt() ```이다.

> ``` wait() ``` = ``` join() ```   
``` stop() ``` = ``` interrupt() ```

***

## 👻 Multicore Programming
CPU의 core가 하나일 때(Single-Core), 스레드는 Interleaving될 것이다(**Concurrency**).   

> **Interleaving** : 사이사이 끼워넣음

반대로 core가 여러개인 Multiple-Cores를 사용하게 되면 스레드는 **병렬적(Parallel)**으로 동작한다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-1/single.PNG)   
![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-1/multi.PNG)   

- **프로그래머가 멀티코어를 사용할 때 해야할 일**
    - **Identifying Tasks** : 동시에 실행 가능한 영역이 어딘지 찾기
    - **Balance** : 스레드 일의 균형 맞추기
    - **Data Splitting** : 데이터 쪼개기
    - **Data Dependency** : 데이터의 동기화
    - **Testing and Debugging** : 싱글 스레드보다 테스팅과 디버깅이 어려워진다.

그렇다면, 코어의 수가 무조건 많을수록 좋을까?   
👉 그건 아님!

**암달의 법칙(Amdahl's Law)**은 위 내용을 증명해준다. 암달의 법칙은 컴퓨터 시스템의 일부를 개선할 때 전체적으로 얼마만큼의 최대 성능 향상이 있는지 계산하는 데 사용된다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-1/amdahl.png)   

가로축은 프로세서 코어의 개수, 세로축은 실질적으로 향상된 속도를 보여준다. 이상적인 속도 향상은 빨간색 그래프를 따라가지만, 실질적인 수치는 나머지 그래프를 그리며 코어의 개수에 속도 향상이 비례하지 않다는 것을 보여준다.

Speedup의 계산 방법은 아래와 같다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-1/speedup.PNG)   

- **S** : 시스템에서 연속적으로 실행되는 어플리케이션의 부분(병렬이 아님)
- **N** : 프로세서 코어의 개수

초록색은 어플리케이션의 5%가 연속적으로 실행될 때, 파란색과 보라색은 각각 10%, 50%가 연속적으로 실행될 때를 의미하며, 이러한 그래프에서 아무리 연산에 사용되는 코어를 무한대로 추가한다고 해도 성능 향상의 한계가 존재한다는 것을 알 수 있다.

> 어플리케이션의 연속적인 부분은 연산 코어를 추가하여 얻는 성능 향상과 **반비례**하다.

***

## 👻 Multithreading Models
멀티스레딩 모델도 두 가지가 존재한다.

- **User** threads
    - Java 같이 사용자가 직접 제어할 수 있는 스레드
    - OS의 CPU Core에 직접 접근, 제어할 수 없음
- **Kernel** threads
    - OS에 의해 CPU Core에 직접 접근, 제어할 수 있음

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-1/thread-model.PNG)   

위 두 스레드 사이의 관계는 세 가지 모델이 있다.

- Many-to-One Model

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-1/many-to-one.PNG)   

- One-to-One Model

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-1/one-to-one.PNG)   

- Many-to-Many Model

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-2-1/many-to-many.PNG)   

***

## 👻 Thread Libraries
스레드 라이브러리는 스레드의 생성과 관리를 제공하며 아래와 같이 세 개의 메인 스레드 라이브러리가 있다.

- **POSIX Pthreads**
    - POSIX 표준(IEEE 1003.1c) 제공
    - 스레드 구현이 아닌 **행동**을 명세
    - ``` pthread_attr_init(&thread_attributes); ``` : 초기화
    - ``` pthread_create(&tid, &thread_attributes, thread_runner_func, argv[1]); ``` : 생성
    - ``` pthread_join(tid, NULL); ``` : 대기
    - ``` pthread_exit(0); ``` : 종료
- **Windows thread**
    - 윈도우에서 제공하는 스레드
- **Java thread**(OS에 종속적)
    - 자바에서 제공하는 스레드

***

## 👻 Implicit Threading
Concurrent and Parallel한 앱은 만들기 힘들다. 이 복잡한 부분을 알아서 처리해주는 게 **Implicit Threading**이다.

Implicit Threading을 사용하는 네 가지 방법

- **Thread Pools** 이용
    - 여러개의 스레드를 스레드 풀을 만들어 저장해둔 다음 거기서 꺼내 사용한다.
    - ``` ThreadPool.getThreads ```
    - 풀은 Maximum을 설정해줘야함
- **Fork & Join(Wait)**
    - Explicit(명백한) Threading을 Implicit(함축적인) Threading으로 할 수 있음
- **OpenMP(Open Multi-Processing)**
    - 컴파일러 지시어를 이용해 C/C++에서 병렬처리가 가능하도록 지원하는 API
    - _[위키 보러가기...](https://ko.wikipedia.org/wiki/OpenMP)_
- **Grand Central Dispatch (GCD)**
    - Apple의 maxOS와 iOS에서 사용하는 방법

> 💡 **OpenMP**   
**병렬 처리 지역**만 지정해주면 코드 블럭을 알아서 병렬적으로 실행시켜주도록 만든다. 이전까지의 방법은 라이브러리에게 일을 시키는 방식이었다면 이 방식은 컴파일러에게 일을 다 시킨다.   
>
> c에서는 ``` omp.h ``` 파일 Include 후 ``` #pragma omp parallel ```을 사용하면 병렬 처리가 가능하다. ``` #pragma ``` 블럭 안에 있는 코드는 병렬처리 해달라는 의미이다.

- **OpenMP**의 예시

```c
#include <stdio.h>
#include <omp.h>

int main(int argc, char *argv[])
{
    #pragma omp parallel
    {
        printf("i am a parallel region.\n");
    }

    return 0;
}

// 실행 명령어
$gcc -fopenmp File_title.c
```

***

## 👻 글을 마치며
이번 시간에는 스레드에 대해 정리해보았다. 진짜 너무 어렵고 이해가 잘 안 되는 것 같다 ㅠㅠ(그래도 그래픽스는 이해라도 됐는데...) 아무래도 눈에 보이는 부분이 아니라서 확실히 적응하기 힘든 부분도 없지 않아 있는 것 같다. 단어도 많고 너무 추상적이라 힘들지만.. 어떻게든 이해해보려고 열심히 노력 중..😞

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   