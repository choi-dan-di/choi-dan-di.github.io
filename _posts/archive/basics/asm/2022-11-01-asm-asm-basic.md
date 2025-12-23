---
title: "[ASM] 어셈블리 언어(Assembly Language) 입문 & SASM 설치 및 세팅 방법"
excerpt: "어셈블리 언어의 개념을 이해하고 SASM을 통해 컴퓨터가 어떻게 데이터를 관리하는 지에 대해 알아보기"

categories:
  - Assembly Language
tags:
  - [study, asm, assembly, sasm]

permalink: /asm/asm-basic/

toc: true
toc_sticky: true

date: 2022-11-01 00:01:01+0900
last_modified_at: 2022-11-01 00:01:01+0900
---

## 👻 어셈블리 언어란?
**어셈블리어(assembly language)** 또는 **어셈블러 언어(assembler language)**는 기계어와 일대일 대응이 되는 컴퓨터 프로그래밍의 저급(low level) 언어이다.   
컴퓨터 구조에 따라 사용하는 기계어가 달라지며, 따라서 기계어에 대응되어 만들어지는 어셈블리어도 각각 다르게 된다.

여기서 기계어는 CPU가 직접 해독하고 실행할 수 있는 비트 단위로 쓰인 컴퓨터 언어를 통틀어 일컫는 말인데, 프로그램을 나타내는 가장 낮은 단계의 개념이다.
_<u>0과 1, 이진수</u>_ 로 프로그램을 하는 기계어는 컴퓨터가 바로 읽을 수 있다는 점만 빼면 장점이 없는 언어이기 때문에 이를 보완하기 위해 나온 언어가 어셈블리어이다.
컴퓨터 프로그래밍에서 기계어는 대부분 어셈블리어를 거쳐 짜여지게 된다.

고급 언어는 컴파일하는 시간이 오래 걸리는 단점이 있는 반면, 저급 언어는 컴퓨터와 가까운 언어이기 때문에 컴파일을 해도 간단한 명령으로 실행돼서 실행 속도가 굉장히 빠르다.
하지만 저급 언어는 배우기가 어렵고 유지보수가 힘들다는 이유로 특수한 경우를 제외하고는 사용되지 않고 있다.

그래도 임베디드 시스템이나 커널 프로그래밍, 컴퓨터 보안 등은 어셈블리어를 알아야 하고, <u>디버깅이 불가능한 라이브 서비스</u>를 진행하고 있는 환경에서 어느정도 디버깅을 할 수 있게 해주니 어셈블리어에 대한 전반적인 지식을 배움으로써 컴퓨터의 구조를 더 자세히 알 수 있기 때문에 어셈블리어로 코딩을 할 정도는 아니어도 해석 정도는 할 수 있으면 좋을 것 같다.

***

## 👻 SASM이란?
SimpleASM의 줄임말로 4개의 어셈블리어(NASM, MASM, GAS 및 FASM)를 위한 무료 오픈 소스 크로스 플랫폼 통합 개발 환경이다.
드미트리 마누신(Dman95)이 작성했으며 간단한 입출력 매크로 라이브러리가 있다.

***

## 👻 SASM 설치 및 기본 세팅
[👉 SASM을 설치 할 수 있는 곳](https://dman95.github.io/SASM/english.html)   
해당 링크로 들어가서 각 환경에 맞게 다운받아 설치하면 간단하다. :)

![Alt text](/assets/images/posts_img/basics/asm/asm-basic/sasm.PNG "SASM 초기화면")
👆 SASM 초기 화면

***

### 🌱 기본 세팅
우선 SASM을 다운 받았으면 각 개발환경에 맞게 설정이 약간 필요하다.   
![Alt text](/assets/images/posts_img/basics/asm/asm-basic/sasm-setting.PNG "SASM 세팅하기")   
> 1. 좌측 상단 Settings 👉 Settings를 클릭
> 2. Build 탭으로 이동
> 3. Mode를 개발 환경에 맞게 변경
나는 64비트 환경이라 x64로 설정해두었다. 참고로 x86은 32비트, x64는 64비트다.

***

### 🌱 "Hello World!" 출력해보기
다음으로 **create new project**를 클릭하면 기본 세팅된 코드로 새 파일이 생성된다.
![Alt text](/assets/images/posts_img/basics/asm/asm-basic/sasm-basic-code.PNG)   
지금은 뭐가 무슨 의미를 뜻하는지 하나도 모르겠지만 앞으로 진행하다보면 자연스레 코드를 해석할 수 있게 된다.   
일단 코딩의 가장 처음 스텝인 ```Hello World!``` 를 출력하는 코드를 작성하고 출력까지 시켜보자.   
<img src="/assets/images/posts_img/basics/asm/asm-basic/helloworld.PNG" width="60%">
<img src="/assets/images/posts_img/basics/asm/asm-basic/helloworld-output.PNG" width="35%">   
우선 위의 사진처럼 코드는 간단하다!   
대충 코드에 대한 설명을 붙이자면   
> **section .data**는 특정된 값이 들어있는 변수들이 저장되는 곳, **PRINT_STRING**은 SASM이 제공하는 문자열 출력 매크로 정도로 알아두면 된다.   

이렇게 듣고 보면 되게 쉬운 것 같은데 막상 백지 상태에서 시작하려면 손가락이 안 움직여지기는 한다. 😂

아무튼, 상단에 있는 **실행(<span style="color: green;">▶</span>)**을 클릭하게되면 SASM이 알아서 빌드하고 실행까지 시켜준다.   
출력값이 잘 나오는 걸 확인한 후, 실행 파일(Save .exe)로도 저장해서 파일을 직접 실행시켜봤다.   
![Alt text](/assets/images/posts_img/basics/asm/asm-basic/save-file.PNG)   
실행 파일로 실행시키게 되면 실행이 끝난 후 파일창이 자동으로 닫히며 SASM 내의 Output에 결과값이 출력된다.   
cmd 창에서 확인하고 싶다면 명령어를 사용해서 파일을 실행시키면 된다.   
![Alt text](/assets/images/posts_img/basics/asm/asm-basic/helloworld-exe.PNG)   
무야호! 아주 잘 된다. 이로써 SASM 세팅과 기본 사용법을 익히게 되었다.

> 💡 <br>Save .exe의 단축키는 **Ctrl + Shife + E**, <br>Build and run은 **F9**, <br>Debug는 **F5**이다.

***

## 👻 컴퓨터의 데이터 처리 방식
우선 실행 파일이 어떤 구조를 가지고 실행이 되는 지 간단하게 알아보자.   

### 🌱 실행 파일 구조
![Alt text](/assets/images/posts_img/basics/asm/asm-basic/exe-file-structure.png)   
**실행 파일(.exe)의 구조**와 파일이 실행될 때 어떤 식으로 데이터가 전송되는 지 나타낸 그림이다.   
File 부분은 말 그대로 실행 파일 구조라고 생각하면 된다.  
그림에서 보이듯이 파일 내엔 여러가지 정보들이 들어있다.  
**text 섹션**엔 우리가 작성한 코드가 들어있고 **data 섹션**엔 우리가 입력했던 ```Hello World!```같이 특정 값을 가지는 데이터가 들어가게된다.   
이 파일을 실행시키면 그제야 _<u>메모리</u>_ 에 정보들이 올라가게 된다.

***

### 🌱 컴퓨터 구조
![Alt text](/assets/images/posts_img/basics/asm/asm-basic/computer-structure.png)   
**컴퓨터 구조**를 보기 편하게 나타낸 그림이다.   
컴퓨터는 크게 **CPU, 메모리, 하드디스크** 이렇게 세 부분으로 나뉜다.   
하드디스크보다 메모리가 물리적으로 CPU와 더 가까워 접근이 빨라 전송 속도가 빠르지만 데이터가 **휘발성**으로 저장되어 컴퓨터 전원이 끊기면 해당 데이터는 사라져버린다.   
반대로 하드디스크는 전원이 끊겨도 데이터가 날아가진 않지만 물리적으로 CPU와 거리가 멀어 전송 속도가 느리다.   
> 롤을 실행시킨다고 가정했을 때, <br>1. 하드디스크에 저장되어 있는 롤을 실행<br>2. 롤 실행 파일이 메모리로 복사되어 돌아감<br>3. CPU와 메모리 사이에서 데이터가 오고가며 롤이 실행됨<br>4. 롤이 종료되면 메모리에서도 사라짐

이러한 과정을 본다면 하드디스크의 데이터는 입·출력 시 한 번 밖에 오고가지 않지만 메모리에서는 수없이 CPU와 데이터를 주고 받을 것이라는 걸 알 게 된다.   
앞으로 어셈블리어를 배우면서 **CPU와 메모리, 그리고 그 사이에 있는 레지스터까지 포함하여** 데이터가 어떻게 오고가는 지를 알 수 있게 될 것이다.

***

## 👻 글을 마치며
나는 이번에 SASM이라는 툴을 사용하여 어셈블리어에 대해 전반적으로 개념을 익히고 실행 파일 구조부터 컴퓨터 구조까지 자료를 통해 눈으로 확인을 하며 대략적인 데이터 전송 방식을 알 수 있게 되었다. 학부생이던 시절, 어쩌면 20살부터 꾸준히 이러한 정보에 노출되어 왔었을텐데 이제서야 점차 이해를 한다는 게 조금 부끄러운 일이지만 늦은 만큼 더 집중하고 끈기있게 파고들어야겠다는 생각을 했다. 물론 이해가 점점 되니까 더 재미있는 것도 있고 😉

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_assembly/tree/master/HelloWorld)_

***

_출처_   
_[어셈블리어 개념-위키피디아](https://ko.wikipedia.org/wiki/%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC%EC%96%B4)_   
_[어셈블리어 개념2-코딩팩토리](https://coding-factory.tistory.com/651)_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   