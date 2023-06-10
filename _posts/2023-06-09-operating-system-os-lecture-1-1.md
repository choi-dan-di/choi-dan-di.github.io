---
title: "[Operating System] #1주차 - 운영체제 개요"
excerpt: "운영체제 공룡책 강의 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, process]

permalink: /operating-system/os-lecture-1-1/

toc: true
toc_sticky: true

date: 2023-06-09 16:55:03+0900
last_modified_at: 2023-06-09 16:55:06+0900
published: true
---

## 👻 운영체제가 뭐길래?
컴퓨터 시스템을 운영하는 **소프트웨어**를 **운영체제**라고 한다.

컴퓨터는 정보를 처리하는 기계이며, 정보는 불확실성을 측정해서 수치화로 표현한 것이며 최소 단위를 **bit(binary digit)**라고 한다. (8 bits = 1 byte)

> 💡 I(x) = -log<sub>2</sub>P(x)

정보의 처리는 정보의 상태 변환으로 표현할 수 있다. (0에서 1로, 1에서 0으로) 이는 **부울 대수(NOT, AND, OR)**, **논리 게이트(NOT, AND, OR, XOR, NAND, NOR)**, **논리 회로(IC, LSI VLSI, ULSI, SoC, ...)** 등으로 표현이 가능하다. 부울 대수를 이용해 논리 게이트를 만들고 논리 게이트를 이용해 논리 회로를 만드는 방식이다.

- 컴퓨터의 정보 처리 방법
  - 덧셈
    - 반가산기, 전가산기
  - 뺄셈
    - 2의 보수 표현법
  - 곱셈과 나눗셈
    - 덧셈과 뺄셈의 반복
  - 함수
    - GOTO

- 컴퓨터는 두 가지로 나뉜다.
  - **범용성** : universality
    - NOT, AND, OR 게이트만으로 모든 계산이 가능
    - NAND 게이트만으로 모든 계산이 가능
    - 범용 컴퓨터 : general-purpose computer
  - **계산가능성** : computability
    - Turing-computable : 튜링 머신으로 계산 가능한 것
    - 정지 문제(Halting Problem) : 튜링 머신으로 풀 수 없는 문제

- Alan Turing(앨런 튜링) : 컴퓨터의 할아버지
- John von Neumann(존 폰 노이만) : 컴퓨터의 아버지
  - 프로그램을 저장할 수 있는 컴퓨터를 발명
  - ISA(Instruction Set Architecture)라고도 불림

프로그램은 명령어의 집합이며, 프로그램을 컴파일하게되면 기계어(어셈블리어)가 생긴다. 운영체제도 프로그램 중 하나이며 **프로세스(중요)**, **리소스**, **UI** 등을 관리한다.

***

## 👻 운영체제가 하는 일
운영체제는 소프트웨어와 하드웨어를 연결해주는 중간다리, 즉 인터페이스이다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-1-1/os.PNG)   

컴퓨터에서 항상 돌아가는 하나의 프로그램을 **커널(Kernel)**이라고 한다. 시스템 프로그램과 어플리케이션 프로그램이 있다.

***

## 👻 컴퓨터 시스템의 구성
예전의 컴퓨터는 다음과 같은 구성을 이룬다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-1-1/computer1.PNG)   

하나 혹은 여러개의 CPU와 여러가지의 장치 컨트롤러, 그리고 그 둘을 연결해주는 버스(bus)가 있다.

- **Interrupts**
  - 키보드나 마우스, 모니터 등의 입출력 장치(I/O Device)와 CPU 간의 통신 방법
  - 시스템 버스를 이용해 신호를 CPU로 전송한다.
  - DMA(Directed Memory Access) : 인터럽트의 한 종류(유튜브 같은 경우에 해당). CPU가 별로 할 일이 없으면 이 방법을 사용한다.

- **Bootstrap**
  - 부츠의 뒤쪽에 위치한 끈을 부트스트랩이라 부른다.
  - 이를 이용해 신발에 발을 딱 맞추듯, 컴퓨터의 파워를 켰을 때 가장 처음으로 실행되는 프로그램을 의미한다.

- **von Neumann Architecture**
  - 명령어의 실행 순환을 수행
  - 명령어 레지스터(Instruction Register)에 명령어를 저장하며 메모리에 올린 후 실행시키는 것을 반복한다.
  - 결과는 다시 메모리에 저장

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-1-1/storage.jpg)   

용량과 Access Time에 따라 **스토리지(Storage: 저장소)**는 계층 구조로 설정된다. 위로 갈수록 처리 속도가 빨라진다.

> 💡 **solid-state disk(ssd)**는 메모리 형태의 하드 디스크이다.

***

## 👻 컴퓨터 시스템의 구조와 기능
컴퓨터 시스템을 이루는 요소는 대표적으로 다음과 같다.

- CPU
- Processor
- Core
- Multicore
- Multiprocessor

- **MultiProgramming**
  - 하나의 프로그램 이상이 돌고있는 것
  - 프로세스가 동시에 올라가있으면 CPU 타임을 높일 수 있음

> 💡 Multitasking = Multiprocessing

멀티프로그래밍이 되면 하나의 CPU를 쪼개서(Time-Sharing) 사용할 수 있음(Concurrency)

- **CPU Scheduling**
  - 이후 처리할 프로그램을 선택하는 과정

모드는 두 가지로 나뉜다.
  - **User mode**와 **Kernel mode**

***

## 👻 Virtualization
여러 개의 OS를 실행시키기 위해 가상의 매니저를 설정하는 것

- VMM(Virtual Machine Manager, Monitor)
  - VMware, XEN, WSL 등등이 있다.

기존의 커널이 관리해줬던 부분을 VMM이 관리해준다고 볼 수 있다.

![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-1-1/virtual.PNG)   

<span style="font-size: 0.7rem; color: gray;">Q. 하나의 OS를 관리하는 게 커널인가..?</span>

***

## 👻 컴퓨터 환경
다양한 컴퓨팅 환경의 운영체제는 다음과 같은 것들이 있다.

- Mobile Computing : Android, iOS
- Client-Server : Web
- Peer-to-Peer : 음악 파일 공유 등. 중간 서버 없이 공유 가능
  - P2P의 연장이 비트코인, 블록체인
- Cloud : AWS, Azure, GCP(Google Cloud Platform)
- Real-Time

> 멀티 프로그래밍의 동기화를 제대로 처리 못하면 데드락에 걸린다.

***

## 👻 글을 마치며
이번 시간에는 운영체제에 대한 기본적인 개념과 여러가지 단어에 대해 알아보았다. 강의를 들으면서 학부생 때 배웠던 내용들이 생각났는데 그 때 그 내용이 이제서야 이해가 되었다 😂 이래서 관심이 중요하다는...

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   