---
title: "[Unreal Engine] 언리얼 엔진 설치하기"
excerpt: "언리얼 엔진을 설치하고 프로젝트 생성까지 해보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, project]

permalink: /unreal/install-ue/

toc: true
toc_sticky: true

date: 2022-11-04 20:28:20+0900
last_modified_at: 2022-11-04 20:28:25+0900
---

## 👻 Unreal Engine 설치하기
이번 시간엔 언리얼 엔진을 설치하고 프로젝트까지 생성해보자.   
우선 언리얼 엔진을 설치하려면 회원 가입은 필수다.

***

### 🌱 언리얼 엔진(에픽 게임즈) 회원가입하기

[👉 언리얼 엔진 공식 사이트 👈](https://www.unrealengine.com/ko/)

언리얼 엔진 공식 사이트에 들어가서 우측 상단에 **로그인** 버튼을 누르면 로그인 하거나 계정을 만들 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/sing-up-1.PNG)   

이메일까지 인증하면 가입이 완료된다.

***

### 🌱 에픽 게임즈 런처 다운로드 후 실행하기
회원가입을 마쳤으면 로그인 한 후에 우측 상단의 **다운로드**를 클릭한다.

![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/download-1.PNG)   

**런처 다운로드**를 눌러 다운받으면 된다.   
다운이 다 되면 런처를 실행시켜보도록 하자.

![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/launcher.PNG)   
런처 화면은 달라지니 크게 부분부분만 참고하자. 나는 이미 런처를 예전에 깔았었고 _5버전_ 을 다운받아 실행시킨 적이 있어서 우측 상단에 저렇게 뜬다.   
이번에는 4.27버전으로 공부 할 예정이라 새로 다운을 받아보도록 하자.   

***

### 🌱 엔진 설치
상단의 ```라이브러리```를 **클릭**한다.

![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/launcher-2.PNG)   

그다음 **엔진 버전**이라는 글에 보면 **플러스(+)** 표시가 있는데 클릭하면 새 엔진을 설치할 수 있다. 나는 _4.27.2버전_ 을 다운 받았다.

![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/launcher-3.PNG)   

> 언리얼 엔진은 컴퓨터에 많은 디스크 공간을 차지한다. 엔진 버전마다 넉넉잡아 **15기가** 정도는 필요하기 때문에 반드시 설치 전에 디스크 공간이 충분한지 확인해야 한다. 또한 설치 크기가 큰 만큼 설치 시간도 오래 걸리니 여유를 가지고 기다려주자.

설치가 다 됐으면 엔진을 한 번 실행시켜보도록 하자!

***

## 👻 새로운 프로젝트 생성하기
이제 언리얼 엔진으로 만들 첫 프로젝트를 생성해보자.

***

### 🌱 새 프로젝트를 선택하거나 생성
![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/project-1.PNG)   

맨 처음 엔진을 실행시키면 나오는 화면이다.   
**게임**을 누르고 **다음**으로 넘어가자.

***

### 🌱 템플릿 선택
![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/project-2.PNG)   

그러면 템플릿을 선택하는 창으로 넘어가는데 지금은 엔진에 익숙해지는 게 목적이므로 비어있는 상태인 **기본**을 선택하고 다음으로 넘어가자.

*** 

### 🌱 프로젝트 세팅
![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/project-3.PNG)   

이제 프로젝트 세팅을 해줘야 하는데, 여기에서는 **프로젝트의 품질/성능 수준**, **대상 플랫폼**, **시작용 콘텐츠 여부** 등을 선택할 수 있다. 일단 처음이니 **시작용 콘텐츠 포함**으로 세팅해주고, 프로젝트의 저장 위치는 디스크 용량이 넉넉한 곳으로 설정해주자. 세팅이 다 됐으면 **프로젝트 생성** 클릭!

![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/project-3.PNG)   

프로젝트 생성 완료 ☺☺☺

***

## 👻 언리얼 엔진의 화면 구성 살펴보기
프로젝트를 생성하면 가운데에 _오브젝트_ 가 배치된 공간이 있고, 주위로 여러 창이 배치된 모습을 볼 수 있다. 여기서 레벨을 편집할 수 있는 등 여러가지 기능을 수행할 수 있다. 이를 **언리얼 엔진 에디터**, 혹은 **레벨 에디터(Level Editor)**라고 부르며 간단히 줄여서 **에디터**라고도 불린다. 레벨은 [여기서](/unreal/unreal-summary/#-레벨-구성) 간단히 개념 정리만 했었다.   

나는 영어 공부도 할 겸, 구글링 했을 때 각 단어들에 익숙해지기 위해서 언어를 영어로 변경해주었다. 언어는 ``` 편집 👉 에디터 개인설정 👉 지역&언어 ```에서 변경할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/english.PNG)   
Welcome To 영어지옥! 😂

> 💡 다시 한국어로 설정하기   
1. ``` Editor 👉 Editor Preferences 👉 Region&Languages ``` 선택
2. ``` Internationalization 👉 Editor language 👉 Korean(한국어) ``` 변경

이제 대략적으로 세팅을 완료했으니 화면 구성을 살펴보자.

***

### 🌱 뷰포트(Viewport)
![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/screen-1.PNG)   

**레벨 공간을 편집**하기 위한 화면이다.

***

### 🌱 메뉴 표시줄
![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/screen-2.PNG)   

일반적인 프로그램의 메뉴와 같다. 파일 또는 창, 언리얼 엔진의 기능들을 각 메뉴에서 관리한다.

***

### 🌱 액터 배치(Place Actor)
![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/screen-3.PNG)   

언리얼 엔진에서 제공하는 **액터들을 모아놓은 패널**이다. _기본 도형에서부터 조명, Volume 등_ 기능을 가진 액터들을 간편하게 꺼낼 수 있도록 카테고리별로 구분되어있다. 필요한 액터를 **드래그 앤 드롭**으로 간단하게 배치할 수 있다.

***

### 🌱 콘텐트 브라우저(Content Browser)
![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/screen-4.PNG)   

엔진에서 쓰이는 모든 파일 또는 리소스를 **애셋(Asset)**이라고 하는데, 현재 진행 중인 프로젝트에서 사용할 수 있는 모든 애셋을 관리하는 패널이다.

***

### 🌱 월드 아웃라이너(World Outliner)
![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/screen-5.PNG)   

**뷰포트에 배치된 액터의 리스트**다. 트리 구조로 한 번에 파악할 수 있고 _선택 및 숨김 등_ 을 적용할 수 있다.

***

### 🌱 디테일(Detail)
![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/screen-6.PNG)   

뷰포트 또는 월드 아웃라이너 패널에서 선택한 **액터의 상세 정보**가 표시된다. _편집 가능한 파라미터_ 를 여기에서 변경할 수 있다.

***

### 🌱 툴바(ToolBar)
![Alt Text](/assets/images/posts_img/engines/unreal/install-ue/screen-7.PNG)   

언리얼 엔진 에디터에서 **많이 사용되는 기능**을 _큰 아이콘의 버튼으로 배치된 모음_ 이다. 버튼의 오른쪽에 있는 **드롭다운** 버튼으로 각 버튼의 서브 메뉴를 표시하고 선택한다.

***

> 모든 패널은 독립적으로 움직일 수 있으며 마음대로 커스텀이 가능하다. 위치를 바꿨다가 ``` Window 👉 Load Layout 👉 Default Editor Layout ```으로 초기화할 수 있다.

***

## 👻 글을 마치며
이번 시간엔 언리얼 엔진 계정을 만들고 런처 다운로드, 실행까지 해보았다. 그리고 프로젝트를 생성해서 기본적인 구조를 살펴보았다. 앞으로 다양한 기능을 마음껏 써가며 내가 원하는 게임을 만들어 나갈 수 있도록 노력해야겠다. 😉

***

_출처_   
_[저자 깃허브](https://github.com/araxrlab/lifeunreal)_   
<span style="font-size: 0.7em; color: gray;">이 포스팅은 _'인생 언리얼 교과서(성안당)'_ 를 바탕으로 쓰였습니다.</span>   