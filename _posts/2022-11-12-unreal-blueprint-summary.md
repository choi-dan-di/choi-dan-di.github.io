---
title: "[Unreal Engine] BP - 블루프린트 기초"
excerpt: "언리얼 엔진에서 제공하는 블루프린트 기능에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp]

permalink: /unreal/blueprint-summary/

toc: true
toc_sticky: true

date: 2022-11-12 19:03:50+0900
last_modified_at: 2022-11-12 19:03:52+0900
---

## 👻 블루프린트란?
**블루프린트(blueprint)**란 언리얼 엔진에서 제공하는 **비주얼 스크립팅 시스템**이다. 복잡한 C++ 문법 없이 노드 기반으로 그림을 그려서 프로그래밍 하는 방법이다. 프로토타입 같은 간단한 타입들을 만들 때 유용하다. 언젠간 C++로 넘어간다고 해도 블루프린트는 꼭 사용하게 되는 경우가 많으니 참고하면 좋을 것 같다.

***

## 👻 블루프린트 시작하기
우선 언리얼 엔진 프로젝트를 새로 생성하여 블루프린트 화면을 열어보자. 이번 실습부터 모든 실습을 **5버전**으로 진행할 예정이다. ~~이전에 올렸던 포스팅도 앞으로는 5버전으로 진행할 예정이다.~~

***

### 🌱 블루프린트 열기
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/blueprint-summary/open-bp.PNG)   

언리얼 엔진 에디터를 실행한 후 프로젝트를 만들고 해당 위치로 들어가 블루프린트를 열어주자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/blueprint-summary/bp-main.PNG)   

그러면 이렇게 블루프린트 화면이 따로 뜨게되는데 탭을 **클릭&드래그** 해서 창을 따로 뺄 수도 있고 본 프로젝트 탭 옆에 도킹도 가능하다.

언리얼은 레벨이 하나 생성될 때마다 레벨 블루프린트도 같이 생성된다. 레벨 당 하나씩 짝을 맞춰 생성되기 때문에 레벨 별로 구분지어진다.

> 💡   
블루프린트 화면에 **우클릭 한 후 드래그**를 하면 화면 이동이 가능하고 **좌클릭**하면 범위 선택이 가능하다.

***

### 🌱 게임 화면에 Hello 출력하기
이제 블루프린트로 게임 화면에 **Hello**라는 단어를 출력해보자. 빈 화면에 **우클릭**한 후   
``` Print Text ```를 찾아서 눌러주면 박스 하나가 뜨게된다.   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/blueprint-summary/print-text.PNG)   

그 다음, ``` Event BeginPlay ``` 박스에서 줄을 드래그하여 ``` Print Text ``` 박스와 연결시켜준 후 좌측 상단의 **Compile**과 **Save**를 차례로 눌러주자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/blueprint-summary/connect.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/blueprint-summary/compile-save.PNG)   

이제 다시 프로젝트 화면으로 넘어와서 게임을 **실행(단축키 Alt+P)** 시켜주면 이렇게 게임 화면에 **Hello**라는 문자가 뜨는 것을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/blueprint-summary/hello.PNG)   

아주아주 간단하다. ☺☺☺

>- **Event** : 일종의 트리거. _어떤 상황에서 호출이 될 것이다_ 라는 약속을 한 것이다.
  - **BeginPlay** : 게임이 시작될 때 한 번만 실행되는 이벤트
  - **Tick** : 매 프레임마다 계속 실행되는 이벤트

***

### 🌱 주석달기
원하는 만큼 박스를 드래그하여 묶은 후, ``` C ``` 버튼을 누르게되면 해당 이벤트에 **주석(Comment)**을 달 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/blueprint-summary/comment.PNG)   

이 외에도 블루프린트에는 다양한 단축키가 존재한다. ~~따로 정리 해야겠다. 😂~~

***

## 👻 글을 마치며
이번 시간에는 블루프린트에 대한 기초부터 언리얼 엔진에서 어떻게 동작하는지를 살펴보았다. 코딩이 그림화 된 기분이랄까. 굉장히 쉬워서 놀랐던 것 같다. 프로토타입을 만들 때 사용하면 아주 유용할 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   