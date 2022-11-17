---
title: "[Unreal Engine] BP - 함수 기초"
excerpt: "블루프린터의 함수에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, function, basic, function basic]

permalink: /unreal/bp-function-basic/

toc: true
toc_sticky: true

date: 2022-11-17 00:06:36+0900
last_modified_at: 2022-11-17 00:06:38+0900
---

## 👻 함수 기초
이번 시간에는 블루프린터에서의 함수 개념에 대해 알아보자. 우리가 자주 사용했던 ``` Print Text ```같은 노드가 함수에 해당된다. 블루프린트 화면에서 좌측 **Functions** 탭을 이용해 추가할 수 있지만 그렇게 추가된 함수는 **C++**의 **멤버 함수**에 해당된다. 지금 공부할 내용은 아주 기초부분이기 때문에 밖으로 넘어와서 **Blueprints** 폴더에 새 항목을 추가해주자.

***

### 🌱 함수 생성하기
컨텐트 브라우저에서 **Blueprints 폴더** 안에 하나의 파일을 생성해 줄 것이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/create-lib.PNG)   

``` 우클릭 👉 Blueprints 👉 Blueprint Function Library ```를 눌러 생성한 후 더블클릭해주자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/default.PNG)   

함수 하나가 생성된 것을 볼 수 있다.

> **이렇게 만든 함수와 BP에서 생성하는 함수와의 차이점은 무엇일까?**   
이렇게 만들면 **C++**에서 말하는 **정적 함수(Static Function)**를 의미한다. 일반 블루프린트에서 함수를 생성하게되면 멤버 함수라고 했는데, **멤버 함수**는 클래스 내에 생성되는 함수를 의미한다.

***

### 🌱 함수 호출하기
위에서 만든 함수를 다시 블루프린터로 넘어와서 검색하면 노드를 생성하여 사용 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/call-func.PNG)   

지금은 함수를 생성하기만 하고 기능을 정의하지 않았으니 다시 function library로 넘어가 함수를 정의해보자.

***

### 🌱 함수 정의하기
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/in-out.PNG)   

다시 함수를 생성했던 창으로 넘어와 우측 디테일을 보면 ``` Input ```, ``` Output ```을 설정해줄 수 있는 탭이 있다. ``` Integer ``` 타입의 입력값 두 개를 설정해보자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/input.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/input2.PNG)   

Input 값을 설정해주니 왼쪽 노드에도 핀이 추가되었다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/input3.PNG)   

블루프린트에서도 핀이 추가되었다는 것을 알 수 있다.

- **덧셈 함수 만들기**
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/func2.PNG)   

출력값을 하나 추가해서, 두 입력값을 더한값을 출력해주는 함수를 만들 수 있다.

- **블루프린트**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/func3.PNG)   

함수를 세팅하고 컴파일하면 바로 적용되는 것을 볼 수 있다.

***

## 👻 로컬 변수
함수 내에서만 사용하는 변수를 의미한다. function library의 좌측 탭을 보면 로컬 변수를 생성할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/local-variable.PNG)   

우리가 일반적으로 추가해서 사용하는 변수는 **멤버 변수**에 해당한다. ~~이건 나중에 자세히 다루도록..~~

로컬 변수는 함수 내에서만 사용하고 소멸되는 변수이다. 일반 변수와 선언 방식은 동일하다. 개발 방식만 다를 뿐이지 메모리 사용도 다른 프로그래밍 언어와 동일하다. 마지막으로 함수를 하나 더 추가해 살펴보자.

<img src="/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/local2.PNG" width="20%">
<img src="/assets/images/posts_img/engines/unreal/blueprint/function/bp-function-basic/local3.PNG" width="20%">

위 이미지처럼 로컬 변수는 함수마다 다르게 설정할 수 있다. ``` My Add ```에서 선언한 로컬 변수를 ``` NewFunction 1 ```에서는 사용할 수 없다. 다른 데에서 쓰려면 출력값에 저장해 꺼내 써야한다.

***

## 👻 글을 마치며
이번 시간에는 블루프린트에서 어떻게 함수를 생성하고 정의하는 지 알아보았다. 생각보다 간단한데 기본 개념없이 헤딩 했으면 많이 헤맸을 것 같다. ~~누가 블루프린트에서 만드는 함수가 멤버 함수인 줄 아냐구..~~ 그래도 페이지가 분리되어있고 함수만 따로 모아서 정리해두니까 확실히 편한 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/function/BP_FunctionBasic)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   