---
title: "[Unreal Engine] BP - 배열(Array)"
excerpt: "블루프린터에서 제공하는 배열(Array)에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, data structure, array]

permalink: /unreal/bp-array/

toc: true
toc_sticky: true

date: 2022-11-23 23:23:58+0900
last_modified_at: 2022-11-23 23:24:00+0900
---

## 👻 배열 만들어보기
이번 시간에는 블루프린트에서 배열을 사용하는 방법에 대해 알아보자. 기본적으로 사이즈의 여유가 있는 **동적 배열**이고, 변수를 추가해준 다음 값의 형태를 결정해줄 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/variable.PNG)   

우선 변수를 하나 생성해 준 다음 우측 디테일 창에서 ``` Variable Type ``` 탭 가장 오른쪽 부분을 눌러주면 타입을 지정할 수 있는 창이 뜬다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/array.PNG)   

배열(Array)을 선택해주면 변수 타입이 바뀐 것을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/variable2.PNG)   

컴파일, 저장 후 다시 디테일 창을 보면 변수의 집합을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/default-value.PNG)   

보통은 직접 설정하기보단 함수들을 이용하여 만들게 된다.

***

## 👻 배열 출력하기
일반적인 프로그래밍과 똑같다. 반복문을 이용해 배열을 순회하며 값을 하나씩 뽑아낼 수 있다. 이전에 배웠던 ``` For Loop ```을 사용해도 되지만 **배열 전용 반복 노드**인 ``` For Each Loop ```을 사용하면 훨씬 간편하게 값을 뽑아낼 수 있다.

- ``` For Loop ``` 사용   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/for-loop.PNG)   

> ``` get ``` 노드를 이용해서 **특정 인덱스**에 해당하는 배열의 값을 가져올 수 있다.

- ``` For Each Loop ``` 사용

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/for-each-loop.PNG)   

> 💡 **For Each Loop**   
- **Array Element** : 배열의 **요소(값)**를 추출한다.
- **Array Index** : 배열의 **인덱스**를 추출한다.

***

## 👻 자주 사용하는 기능
배열을 사용하면서 자주 사용하게 될 기능에 대해 간단하게 알아보자.

***

### 🌱 LENGTH
배열의 길이를 가져온다. ``` Length - 1 ```은 **마지막 인덱스**를 나타낸다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/length.PNG)   

***

### 🌱 ADD, ADDUNIQUE
배열에 값을 추가한다. 배열의 가장 마지막에 추가된다. ``` Add Unique ```는 기존 배열과 중복되는 값이라면 추가를 하지 않는다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/add.PNG)   

***

### 🌱 FIND, CONTAINS
배열을 탐색한다. 원하는 값을 찾는다. ``` Find ```는 값을 찾으면 해당 **인덱스**를 리턴하고 ``` Contains ```는 값의 존재 여부에 따라 **불리언 값**을 리턴한다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/find.PNG)   

***

### 🌱 RESIZE
배열의 크기를 설정한다. 설정한 크기만큼 배열의 크기가 할당되며 초기값을 정해주지 않는다면 **0**으로 세팅된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/resize.PNG)   

***

### 🌱 SET
배열의 값을 세팅한다. ``` Size to Fit ```을 체크하면 해당 인덱스가 현재 인덱스보다 초과되어도 공간을 할당해 값을 채워넣는다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-array/set.PNG)   

> 💡 **Set Array Elem**   
- **Target Array** : 설정할 배열
- **Index** : 설정할 인덱스
- **Item** : 설정할 값
- **Size to Fit** : 체크 시, 설정한 인덱스만큼 배열 확장

***

## 👻 글을 마치며
이번 시간엔 블루프린트에서 배열을 어떻게 사용하는지 알아보았다. 단일 타입만 보다가 배열을 보니 약간 신기했다. ~~어쩜 까먹고 있어서 그랬을지도~~ 근데 단일 타입의 노드와 크게 달라보이는 게 없어서 코딩할 때보다는 한 눈에 알아보기 힘들 것 같다. 연습 문제를 풀어보면서 얼른 익혀야겠다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/data-structure/BP_Array)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   