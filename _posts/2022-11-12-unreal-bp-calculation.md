---
title: "[Unreal Engine] BP - 사칙 연산, 비교 연산, 논리 연산"
excerpt: "블루프린트에서 사칙 연산, 비교 연산, 논리 연산 해보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, arithmetic, comparison, calculation]

permalink: /unreal/bp-calculation/

toc: true
toc_sticky: true

date: 2022-11-12 21:57:49+0900
last_modified_at: 2022-11-12 21:57:51+0900
---

## 👻 사칙 연산
이번 시간에는 블루프린트에서 **사칙 연산** 하는 법을 알아보자. 노드 호출은 쉽다. 화면에 **우클릭** 후 각각의 **기호**를 입력하면 된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-calculation/add.PNG)   

기호는 ``` + ```, ``` - ```, ``` * ```, ``` / ```를 사용하고 ``` Operators ```에 뜨는 함수를 클릭하면된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-calculation/arithmetic.PNG)   

모두 두 개의 값을 받아 하나의 출력값을 가지며 입력값의 핀을 우클릭하여 **입력받을 값의 타입**을 설정할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-calculation/convert-pin.PNG)  

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-calculation/add-integer.PNG)   

위 캡쳐처럼 ``` Integer ``` 타입으로 변환해주면 정수 덧셈이 가능해지는 노드를 만들 수 있다. Get 노드를 이용하여 바로 연결시켜주면 알아서 입력값으로 들어가고 타입이 설정된다.

> 노드 우측 하단 **Add pin plus(+)** 버튼을 누르면 입력값의 개수를 늘릭 수 있다.   
반대로 입력값을 지우고 싶다면 핀 우클릭 후 ``` Remove pin ```을 클릭하면 된다.

***

### 🌱 나눗셈은 항상 조심!
> 다른 프로그래밍 언어에서는 0으로 나누면 프로그램이 에러가 나거나 뻗어버린다. 하지만 블루프린트에서는 값이 0으로 잘 출력된다. 또한 정수 나눗셈에서는 ``` C++ ```과 마찬가지로 소수점 뒷자리가 모두 날아가버리니 **실수 타입**으로 나눠주어야 한다는 걸 잊지 말아야 한다.

***

## 👻 비교 연산
값을 판별하고 비교할 때 사용한다. 호출 방법은 사칙 연산과 동일하게 **우클릭** 후 각각의 **기호**를 검색하면 된다.

> ``` > ```, ``` >= ```, ``` < ```, ``` <= ```, ``` == ```, ``` != ``` 등의 기호를 검색하면 웬만한 비교 연산은 모두 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-calculation/comparisons.PNG)   

Hp와 0을 비교하여 0보다 작거나 같으면 죽음을, 0보다 크면 살았음을 출력하는 간단한 예제를 만들어보자.

- 블루프린트   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-calculation/comparison.PNG)   

- 결과   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-calculation/comp-result.PNG)   

- Hp의 Default Value를 0으로 수정한 후의 결과   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-calculation/comp-result2.PNG)   

> 💡 **branch**   
분기문을 의미하는 브랜치 노드는 비교 연산과 잘 쓰인다. **B를 누른 후 좌클릭**하면 노드를 바로 생성할 수 있다.

***

## 👻 논리 연산
여러가지 조건을 한 번에 비교할 때 사용한다. 호출 방법은 위의 연산과 동일하게 **우클릭**후 검색하면 된다. 대표적으로 ``` not ```, ``` and ```, ``` or ```이 있고 검색시엔 해당 단어 뒤에 ``` bool ```을 붙여주면 된다. 마찬가지로 **Add pin plus**를 클릭하면 추가로 입력값을 받을 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-calculation/logicals.PNG)   

조건을 만족하면 **참**, 조건을 만족하지 않으면 **거짓**을 리턴한다.

***

## 👻 글을 마치며
이번 시간엔 각종 연산을 블루프린트에서 어떻게 하는지에 대해 알아보았다. 이미 알고 있던 것들이라 쉽게 할 수 있었다. 굉장히 직관적이라 간단하고 단축키도 편해서 금세 적응할 수 있었다. 그리고 무엇보다 재미있다. ☺

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/data-control/BP_Variables)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   