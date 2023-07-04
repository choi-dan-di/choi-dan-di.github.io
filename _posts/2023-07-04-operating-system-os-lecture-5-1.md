---
title: "[Operating System] #5주차 - Main Memory"
excerpt: "운영체제 공룡책 강의 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, main memory, memory]

permalink: /operating-system/os-lecture-5-1/

toc: true
toc_sticky: true

date: 2023-07-04 16:08:47+0900
last_modified_at: 2023-07-04 16:08:51+0900
published: true
---

## 👻 Background
프로세스는 실행 중인 프로그램이다. 이 말인 즉슨, **메인 메모리**에 해당 프로그램이 로드되어있다는 의미이다.

여기서 **메모리**는 바이트의 큰 배열로 구성되어있으며 각 공간은 각자의 주소값을 가진다. CPU는 PC가 가지고 있는 메모리의 주소를 이용하며 load, store 등 다양한 작업을 수행한다.

- **Memory Space**
    - 프로세스는 분리된 메모리 공간을 가짐
    - 두 개의 레지스터
        - **Base Register**
        - **Limit Register**
        - Base와 Base+Limit 주소값을 이용해 유효한 주소인지 판단한다.
        - 유효 범위를 벗어나면 Segmentation Fault 에러 발생   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/memory-space.PNG)   

- **Protection of Memory Space**
    - 항상 체크해야하므로 하드웨어적으로 구성됨   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/protection.PNG)   

- **Address Binding**
    - 프로그램은 결국 Binary Executable File 형식으로 디스크에 저장됨
    - 프로그램에서 주소를 다루는 방식은 단계별로 다 다름
        - ex. 소스코드의 경우 컴파일러가 주소를 정해줌
        - 컴파일러는 exe 파일 혹은 바이너리 파일(.out)을 생성
        - 변수는 그저 주소를 나타내는 기호일 뿐(Symbolic)
    - 실행시키기 위해서 프로그램을 메모리로 가지고 오면 프로세스가 된다.
    - 프로세스의 주소는 OS Kernel이 정해주고 00000000으로 시작하지 않는다.
    - **컴파일러(Compiler)**는 심볼릭 주소로부터 **Relocatable 주소**로 **바인딩**해준다(위치 재부여).
    - **링커(Linker)** 혹은 **로더(Loader)**는 Relocatable 주소로부터 **Absolute 주소**를 바인딩해준다(절대적인, 물리적 주소로 재부여).   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/multistep.PNG)   

- **Logical vs Physical Address Space**
    - 두 주소를 매핑해주는 단계가 필요하다.
    - **Logical Address**
        - CPU에 의해 생성되는 주소
        - 유저 프로세스에서 접근하려는 주소
        - 물리적 주소와 관계가 없음
    - **Physical Address**
        - 메모리 단위에 의해 보여지는 주소
        - 메모리 주소 레지스터에 매핑시켜줘야 함
    - 결국 위의 두 주소를 각각 관리하는 공간이 분리되어야 한다.
        - **Logical Address Space**
            - 유저 프로그램에 의해 생성되는 모든 논리적 주소의 집합
        - **Phisical Address Space**
            - 이러한 논리적 주소와 대응되는 모든 물리적 주소의 집합

이렇게 논리적 주소와 물리적 주소의 공간을 분리해주는 하드웨어적 방법으로 **MMU**가 있다.

- **MMU (Memory Management Unit)**
    - 논리적 주소로부터 물리적 주소를 연결시켜주는 하드웨어 장치
    - **Relocation Register** : MMU의 Base Register   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/mmu.PNG)   
    - MMU 예시   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/mmu2.PNG)   

- **Dynamic Loading**
    - 파일 전체를 로딩할 필요가 있을까?
    - 메모리 주소 공간을 효율적으로 사용하기 위해 루틴을 필요할 때만 호출해주는 것
    - Relocatable Linking Loader가 필요한 루틴을 호출할 때에만 주소 테이블에 반영

- **Dynamic Linking** and **Shared Libraries**
    - **DLLs** : Dynamically Linked Libraries
        - 시스템 라이브러리를 프로그램이 실행될 때 링크시킴
    - **Static Linking**
        - 시스템 라이브러리를 전부 로더가 링크시킴
    - **Dynamic Linking**
        - 링킹을 실행시까지 연기시켜 실행 중에 DLL 파일을 링크시킴
    - **Shared Library**
        - ``` .so ```(Linux), ``` .dll ```(Windows) 등의 라이브러리

***

## 👻 Contiguous Memory Allocation
프로세스에게 메모리를 할당해주는 방식에 대해 알아보자. 메인 메모리를 할당받을 땐 가능한 가장 효과적인 방법이 필요하다. 메모리는 크게 OS 혹은 유저 프로세스로 구분되는데, 몇몇 유저 프로세스는 메모리 내에서 동시에 상주하기도 한다.

**연속 메모리 할당(Contiguous Memory Allocation)**은 유저 프로세스를 통째로 할당하는 것이다. 각각의 프로세스는 메모리의 Single Section을 얻으며, 여러 개의 프로세스는 연속적으로 메모리를 할당받게 된다.

- **Memory Protection**   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/allocation-protection.PNG)   

- **Memory Allocation**   
    - **Variable-Partition** Scheme
        - 메모리를 할당하는 가장 간단한 방법 중 하나
        - 프로세스가 종료되고 남은 공간에 새 프로세스를 할당
    - **Hole** : Available Memory 블록
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/allocation-partition.PNG)   

***

### 🌱 The Problem of Dynamic Storage Allocation
동적 스토리지 할당을 하게되면 **Free Holes**에서 어떻게 메모리를 할당할건지에 대한 문제가 발생한다. 이 문제의 해결 방법은 다음과 같이 세 가지 타입으로 나뉜다.

- **First-Fit**
    - 충분히 큰 hole 중 첫 번째로 만나는 hole에 할당
    - Linked List
- **Best-Fit**
    - 충분히 큰 hole 중 가장 작은 hole에 할당
    - Priority Queue (Min Heap)
- **Worst-Fit**
    - 가장 큰 hole에 할당
    - Priority Queue (Max Heap)

위의 해결법을 사용하다보면 또 다른 문제 **단편화(Fragmentation)**가 발생한다.

- **Fragmentation**
    - 단편화 문제
    - **External Fragmentation**
        - 외부 단편화
        - Small Holes가 많이 생기는 것
        - Contiguous Memory Allocation 시 발생
    - **Internal Fragmentation**
        - 내부 단편화
        - 하나의 프레임 안에 빈 공간이 생기는 것
        - Paging 시 발생

> 💡 **Segmentation**   
CMA와 Paging 사이. 메모리 할당 공간을 종류별로 분할하는 것. Variable Size기 때문에 외부 단편화 문제는 심해짐!

***

## 👻 Paging
연속 메모리 할당(CMA)과 반대되는 개념. 메모리를 일정 단위로 쪼개어 관리하는 것이다. 즉, 비연속적(Non-Contiguous)으로 메모리를 할당하며, CMA의 두 가지 문제점을 극복할 수 있다.

> 💡 CMA의 단점을 극복한 Paging의 장점   
- 외부 단편화가 거의 발생하지 않는다.
- Free Holes를 모아 압축하는 과정이 필요 없다.
>
> ➕ 크기가 작으니 중간중간 들어가기도 쉽다!

OS와 하드웨어를 조합해서 제공해야한다. (Memory Access는 하드웨어적인 도움없인 안 됨!)

- **Basic Method for Paging**
    - 물리적 메모리를 고정된 사이즈 블록으로 쪼갠다. (**Frames**)
    - 논리적 메모리를 위와 같은 사이즈 블록으로 쪼갠다. (**Pages**)
    - 이러한 방법을 사용하면 두 메모리 공간은 완전히 분리된다.
    - 모든 주소는 CPU에 의해 두 부분으로 분리되어 생성된다.
        - A **Page Number (p)**
        - A **Page Offset (d)**   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/page.PNG)   

- **The Page Number**
    - 페이지 넘버는 각 프로세스의 **페이지 테이블(Page Table)**의 인덱스로 사용된다.   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/page-table.PNG)   

- **The Page Size**(Like the **Frame Size**)
    - 하드웨어에 의해 결정됨
    - 2의 배수여야함
        - 전형적으로 4KB와 1GB 사이의 크기를 가짐
    - 논리적 주소 공간이 2<sup>m</sup>, 페이지 사이즈가 2<sup>n</sup>일 때,
        - High-order ``` m - n ``` 비트가 **Page Number**를 결정
        - Low-order ``` n ``` 비트가 **Page Offset**을 결정

- **Hardware Support**
    - Context Switch가 일어날 때 Page Table도 Switch 되어야한다.
    - Page Table을 관리하는 것도 일임!
        - **PTBR(Page-Table Base Register)**

- **PTBR**
    - 페이지 테이블을 가리키는 레지스터 사용
    - Context Switch가 빠르지만 Memory Access Time은 여전히 느리다.
    - 두 번 Memory Access를 하게 된다는 단점이 있다.
        - Page Table Entry
        - Actual Data

- **TLB(Translation Look-aside Buffer)**
    - 작고 빠른 **캐시 메모리**를 사용   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/tlb.PNG)   
    - **TLB hit** : TLB 내의 정보 사용
    - **TLB miss** : PTBR 사용
    - **Hit Ratio** : TLB에서 페이지 넘버를 찾을 확률
        - 메모리 액세스 시간이 10ns일 때,
        - 80% hit ratio : EAT = 0.80 * 10 + 0.20 * 20 = 12ns
        - 99% hit ratio : EAT = 0.99 * 10 + 0.01 * 20 = 10.1ns

- **Memory Protection with Paging**
    - Protection bits 사용
    - 각 프레임 별로 Valid-Invalid를 체크
    - **Valid**
        - 해당 페이지는 논리적 주소 공간에 포함되어있음(**legal**)
    - **Invalid**
        - 해당 페이지는 논리적 주소 공간에 포함되어있지 않음(**illegal**)
    - Illegal Address는 Valid-Invalid bit에 의해 걸림   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/valid.PNG)   

- **Shared Pages**
    - 공통 코드를 공유 가능하다.
    - **libc** : Standard C Library
        - 위 라이브러리를 각 프로세스가 로드한다면 비효율적
        - 이러한 코드가 **Reentrant Code**라면 공유할 수 있음

> 💡 **Reentrant Code** : non-self-modifying code. 즉, 실행 중 스스로 수정 불가한 코드   
![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/libc.PNG)   

***

## 👻 Structure of the Page Table
논리적 주소 공간이 너무 커지면 문제가 된다. 이렇게 지나치게 큰 페이지 테이블은 페이지 테이블의 구조를 수정하여 관리할 수 있다.

- **Hierarchical Paging**
    - 페이지 테이블을 계층적으로 만들어 관리
    - Multiple Table   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/hierarchical-paging.PNG)   
- **Hashed Page Table**
    - 32비트보다 큰 주소 공간을 핸들링하기 위해 사용
    - O(1)   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/hash-page-table.PNG)   
- **Inverted Page Table**
    - 기존 페이지 테이블과 반대로 어떤 프로세스가 어떤 페이지 테이블을 가지고 있는지 관리
    - 즉, pid를 추가해주어서 프로세스별로 페이지 테이블을 관리한다는 개념   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/inverted-page-table.PNG)   

***

## 👻 Swapping
모든 프로세스의 물리적 주소 공간 크기를 넘어서는 시스템을 실행 가능하게 해주는 방법이다. 한 페이지씩만 올릴 수 있기 때문에 멀티 프로그래밍의 정도가 증가하게 된다.

명령어와 그 명령어에 사용되는 데이터는 실행될 때 반드시 메모리에 존재해야 하므로, 액세스 할 때에만 메모리에 올라오고 사용하지 않으면 메모리 외부로 다시 보내야 한다.

- **swap in** : 메모리 IN
- **swap out** : 메모리 OUT

- **Standard Swapping**
    - 메인 메모리와 Backing Store 사이의 모든 프로세스를 이동시킴
    - 모든 프로세스를 Swapping하는 비용은 부담이 큼   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/standard-swapping.PNG)   

- **Swapping with Paging**
    - 모든 프로세스를 스와핑하는 대신, 프로세스의 **페이지**를 스와핑
    - 물리적 메모리와 논리적 메모리의 분리 목적은 여전히 달성
    - 아주 작은 페이지의 스와핑이 가능
    - 오늘날의 **페이징**이라 함은 Swapping with Paging을 선호함
        - **Page Out** : 메모리로부터 Backing Store로 페이지 이동
        - **Page In** : Backing Store로부터 메모리로 페이지 이동
    - 페이징은 **Virtual Memory**에서 굉장히 큰 위력을 발휘함   
    ![Alt Text](/assets/images/posts_img/basics/operating-system/os-lecture-5-1/swapping-with-paging.PNG)   


***

## 👻 글을 마치며
이번 시간에는 메인 메모리에 관련된 내용에 대해 알아보았다. 메인 메모리를 어떤 식으로 관리하는지 알 수 있었고 단편화, 페이징, CMA 등 다양한 기법과 문제점들에 대해 알 수 있는 좋은 시간이었던 것 같다. 확실히 하드웨어적으로 많이 엮여있어서 애매모호한 부분도 없지않았는데 이런 부분은 개인적인 궁금증으로 메모해두고 찾아보는 것이 좋을 듯 하다😎

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   