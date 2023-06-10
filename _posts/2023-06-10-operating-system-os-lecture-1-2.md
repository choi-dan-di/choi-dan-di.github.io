---
title: "[Operating System] #1주차 - 프로세스(Process)"
excerpt: "운영체제 공룡책 강의 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, process]

permalink: /operating-system/os-lecture-1-2/

toc: true
toc_sticky: true

date: 2023-06-10 22:05:05+0900
last_modified_at: 2023-06-10 22:05:09+0900
published: true
---

## 👻 프로세스
**프로그램을 실행시켜주는 단위**를 **프로세스(Process)**라고 한다. 필요 자원으로는 CPU, Memory, File, I/O Device 등이 있다.

운영체제는 이러한 프로세스를 관리하는 일을 해야하며 프로세스의 메모리는 여러 섹션으로 나뉜다.

> - Text : 소스 코드 저장
- Data : 초기화 된 정적, 전역 변수
- BSS : 초기화 되지 않은 정적, 전역 변수
- Heap : 동적 메모리 할당
- Stack : 함수 파라미터, 리턴 주소, 지역 변수 등

- **어떻게 관리할 것인가?**   
5개의 **State(생명 주기, Life Cycle)**를 갖고 있다.

- **New** : 프로세스가 막 생성됨
- **Running** : CPU 점령. 프로세스의 명령어를 호출해 실행
- **Waiting** : 진행중이지 않은 프로세스의 상태
    - 예: I/O 하면 자발적으로 대기상태
- **Ready** : CPU를 받을 준비 상태
- **Terminated** : 모든 것을 다 끝내고 종료된 상태

각 상태의 전환 조건을 잘 봐야한다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-1-2/state.PNG)   

- **PCB(Process Control Block)** or TCB(Task Control Block)
    - 이 구조체 안에 프로세스가 가져야 할 모든 정보를 저장한다.
    - OS 입장에서 이 PCB를 가지고 프로세스를 컨트롤하게 된다.
    - 여러 정보를 담고 있음
        - Process State
        - Program Counter : 여기에 있는 메모리 주소를 사용하여 명령어를 가져옴
        - CPU Registers : IR, DR
        - CPU-Scheduling Information
        - Memory-Management Information
        - Accounting Information
        - I/O Status Information

프로세스는 **싱글 스레드**로 실행되는 게 기본적이다. 이는 한 타임에 하나의 Task만 수항하며 어떠한 프로세스는 여러개의 스레드를 통해 이러한 단점을 보완한다. 이를 **스레드(Thread)**라 부른다. (프로세스 내부의 처리 단위인 스레드와는 같은 단어지만 다른 의미를 가진다.)

> 스레드는 **LightWeight Process**라고도 불린다.

***

## 👻 프로세스 스케줄링
- **MultiProgramming**
    - CPU Utilization(사용량)을 최대로 늘리기 위해 **멀티 프로그래밍**이 지향된다. 

- **Time Sharing**
    - CPU Core를 프로세스 간에 자주 스위치하여 유저 입장에선 각 프로그램이 동시에 돌고 있는것처럼 보여지게 하자는 게 Time Sharing의 목적이다.
    - 이를 위해서 CPU 스케줄링이 필요하며 스케줄링 큐를 사용하여 구현한다. (연결 리스트로도 구현 가능)

> 💡 **Queueing Diagram**   
![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-1-2/diagram.PNG)   

- **Context Switch**
    - 직역하면 문맥 교체
    - 프로세스가 사용되고 있는 상태(PCB의 상태)를 Context라고 한다.
    - 어떤 Task가 CPU Core를 다른 Task에게 넘겨주는 것
    - 현재 프로세스를 저장하고 다른 프로세스를 복원하는 것

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-1-2/context.PNG)   

***

## 👻 프로세스 운영
운영체제는 프로세스의 생성 및 소멸을 제공한다. 또한, 하나의 프로세스가 또 다른 프로세스를 생성할 수 있다. 이렇게 되면 두 프로세스는 **부모-자식 관계**가 된다. 두 프로세스 간의 상태에 따라 아래와 같은 두 가지 상태로 변할 수 있다.

- **좀비 프로세스(Zombie Process)**
    - 부모가 있지만 wait 호출을 하지 않고 부모는 부모 일만 하는 프로세스
- **고아 프로세스(Orphan Process)**
    - 부모가 없는 자식 프로세스

```
fork(); // 프로세스 생성
exit(); // 프로세스 소멸
```

> **fork()** 사용 시 자식 프로세스가 생성되며 해당 자식 프로세스의 ID를 리턴한다. 자식 프로세스에겐 0을 리턴한다.

- **사용 예**   

```c++
#include <stdio.h>
#include <unistd.h>
int main()
{
    pid_t pid;
    pid = fork();
    printf("Hello, Process! %d\n", pid);
}
```

프로세스를 생성하고 ``` wait() ``` 명령어를 사용하면 부모 프로세스는 자식 프로세스가 종료될 때까지 대기상태로 변환된다.

***

## 👻 Interprocess Communication
프로세스 간의 통신(IPC)는 두 가지 방식이 있다.

- **Independent**
    - 프로세스 간 독립적으로 움직이며 데이터를 공유하지 않는 방식
- **Cooperating**
    - 프로세스 간 데이터를 공유하는 방식

데이터를 주고 받기 위해서는 IPC 메커니즘이 필요하다.

- **Shared Memory**
- **Message Passing**

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-1-2/ipc.PNG)   

위 이미지에서 (a)는 Shared Memory, (b)는 Message Passing 방법을 보여준다.

***

### 🌱 Shared Memory
이 방법은 버퍼를 이용하는데, 사용할 땐 **Producer-Consumer Problem**을 고려해야한다.

> 💡 **Producer-Consumer Problem**   
생산자는 버퍼를 채우고 소비자는 버퍼를 비우는 작업을 수행한다. 버퍼가 가득차면 생산자가 기다린다거나, 반대로도 버퍼가 비었으면 소비자가 기다리는 문제를 말한다.

이 방식은 명시적으로 버퍼를 쓰고 관리하는 것을 Application Programmer(응용 프로그래머)가 직접 설정해줘야한다는 문제가 있다.

***

### 🌱 Message Passing
Message Passing은 Shared Memory와 다르게 OS가 알아서 해주기 때문에 명시적으로 설정할 필요가 없다.

```
send(message);  // Message 발신
receive(message)    // Message 수신
```

- **Communication Links**
    - 두 프로세스의 통신 방법
    - 구현 방식은 여러개가 있다.

- **Comm. Link의 구현 방식**   
- **Direct**
    - Comm. Link가 자동으로 생성된다.
    - 두 개의 프로세스 간에 **하나의 링크**밖에 존재하지 않는다.
- **Indirect**
    - Ports라는 이름의 **메일 박스(Object)**를 설정한다.
    - 두 개의 프로세스가 **하나의 메일 박스**를 공유할 때 성립된다.
    - 여러개의 프로세스 간에 연결이 가능하다.
- **Synchronous**
    - Blocking Send, Receive
- **ASynchronous**
    - Non-Blocking Send, Receive
- Automatic or Explicit Buffering

***

### 🌱 실제 통신
Shared Memory는 **POSIX Shared Memory**를 사용한다.

> 💡 **POSIX** : Portable Operating System Interface (for uniX)

Message Passing은 **Pipes**를 사용한다.

> 💡 Network에서 쓰는 파이프를 **소켓(Socket)**이라고 한다.   
- **Socket** : 소통을 위한 양종단(EndPoints)을 의미.
- 양종단은 IP Address와 Port를 의미한다.
- 서로 다른 환경이라면 RPCs이용
    - RPCs : Remote Procedure Calls
    - 이는 그 쪽(원격) 컴퓨터에 있는 함수를 사용(호출)하는 것(자바에서 제공)
    - Stub, Skeleton 정의, 객체 직렬화 위해 Marshals the Parameters
- Socket Class : Connection-Oriented (TCP)
- DatagramSocket Class : Connectionless (UDP)
- MulticastSocket Class : Multiple Recipients

***

## 👻 글을 마치며
이번 시간에는 프로세스에 대해 알아보았다. 강의가 아주 기초적인 개념을 알려주는 것 같아서 개인적으로 조금 더 깊은 공부를 할 수 있도록 시간을 내야할 것 같다. ~~너무 어려워 ㅠ.ㅠ~~

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   