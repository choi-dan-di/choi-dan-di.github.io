---
title: "[Unreal Engine] BP - Get, Set"
excerpt: "블루프린트의 Get, Set 명령어에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, get, set]

permalink: /unreal/bp-get-set/

toc: true
toc_sticky: true

date: 2022-11-12 20:10:29+0900
last_modified_at: 2022-11-12 20:10:32+0900
---

## 👻 Get과 Set
지난 시간에 변수 타입에 대해 알아보았다. 이러한 변수들의 값을 가져오거나 설정해주는 명령어를 ``` Get ```, ``` Set ```이라고 한다. 이 명령어들에 대해 알아보고 사용법도 알아보자.

들어가기에 앞서 ```Integer ``` 타입의 ``` Hp ```라는 변수를 하나 만들어두자.

> 컴파일&세이브는 필수! 그래야 변수의 영역이 할당되고 블루프린트에서 이용할 수 있다.

***

### 🌱 Set
먼저 ``` Hp ```라는 변수의 값을 설정하는 방법에 대해 알아보자. **우클릭** 후 ``` set hp ```를 검색해 노드를 하나 만들어보자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-get-set/set-hp.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-get-set/set-box.PNG)   

``` Hp ``` 변수의 값을 설정할 수 있는 노드가 만들어졌다. 가운데 숫자를 입력함으로써 해당 변수의 값을 설정할 수 있게 된다.

> 💡   
좌측 변수 목록에서 ``` Alt + 드래그 ```를 하면 ``` Set ``` 노드를 바로 만들 수 있다.

***

### 🌱 Get
이제 ``` Hp ```라는 변수의 값을 가져오는 방법에 대해 알아보자. Set과 똑같이 우클릭 후 ``` get hp ```를 검색해 노드를 하나 생성하면 된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-get-set/get-box.PNG)   

Get은 Set과 다르게 값을 가져오기만 하면 되는 것이라 입력받는 부분이 딱히 없다. 그래서 값을 가져와 출력할 이벤트만 지정해주면 된다.

> 💡   
좌측 변수 목록에서 ``` Ctrl + 드래그 ```를 하면 ``` Get ``` 노드를 바로 만들 수 있다.

***

### 🌱 변수값 설정 후 출력하기
텍스트를 출력하는 ``` Print Text ```와 연결하여 게임 시작 시 hp를 설정하고 값을 출력해보자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-get-set/set-get.PNG)   

Get 노드를 Print Text에 연결하면 중간에 ``` To Text ```라는 새로운 노드가 생성되는데, **타입 캐스팅**을 의미한다.

컴파일 및 세이브 후 프로젝트로 돌아가 실행하면 Set 노드에서 설정했던 Hp의 값이 출력되는 것을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-get-set/result.PNG)   

Set 노드에서 값을 바로 뽑아 출력도 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-get-set/result2.PNG)   

***

#### 🪐 응용하기
> MaxHp 변수를 새로 생성한 후 Default Value를 200으로 두고, Hp의 값을 MaxHp의 값으로 세팅하고 출력하기

- 블루프린트   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-get-set/result3.PNG)   

- 결과   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-get-set/result4.PNG)   

> 모든 노드는 이벤트 노드와 연결되어야 실행이 된다.

***

## 👻 글을 마치며
이번 시간엔 변수의 값을 설정하고 가져오는 get, set에 대해 알아보았다. 굉장히 간단하면서도 직관적이라 쉬운 것 같다. 코드에 익숙해져있어서 그런지 키보드와 마우스 둘 다 왔다갔다 거려서 손이 더 바쁜 것 같다. 지금은 간단해서 쉬워보이는데 노드들이 많아진다면 관리하기 어려워질 것 같다는 생각을 했다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/data-control/BP_Variables)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   