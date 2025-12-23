---
title: "[Unreal Engine] BP - 연습 문제 : 로또 번호 생성기"
excerpt: "블루프린터로 로또 번호를 생성하는 기능 구현해보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, data structure, array, practice, lotto]

permalink: /unreal/bp-lotto-number-generator/

toc: true
toc_sticky: true

date: 2022-11-24 19:43:31+0900
last_modified_at: 2022-11-24 19:43:34+0900
---

## 👻 로또 번호 생성기
1~45 사이에 있는 수 중에서 6개의 숫자를 무작위로 출력해주는 기능을 구현해보자. 단, 중복은 허용하지 않는다.

우선, 만들기 전에 6개의 번호를 담는 배열 ``` Numbers ```를 추가해주고, 배열의 값을 모두 출력해주는 ``` Print Numbers ``` **커스텀 이벤트**를 만들었다.

- **Print Numbers**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-lotto-number-generator/print-numbers.PNG)   

그런 다음 번호를 생성하는 부분을 크게 세 가지로 만들어보았다.

> 1. **Contains** 사용하기
2. **Add Unique** 사용하기
3. **Shuffle** 사용하기

***

### 🌱 Contains 사용하기
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-lotto-number-generator/contains.PNG)   

``` while loop ``` 노드로 ``` Numbers ```에 6개의 수가 모두 채워질 때까지 반복했다. 기존의 ``` Numbers ``` 배열 내에 랜덤수가 포함하는 지 ``` Contains ``` 노드로 체크한 후 값이 없으면 배열에 추가해주었다. 그렇게 6개가 채워질 때까지 반복하면 끝이다.

***

### 🌱 Add Unique 사용하기
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-lotto-number-generator/add-unique.PNG)   

``` Add Unique ``` 노드는 중복된 값을 제외하고 배열에 값을 추가해준다. 배열의 길이가 6이 될때까지 무한 반복하며 추가해주는 방법이다. 가장 간단하다.

***

### 🌱 Shuffle 사용하기
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-lotto-number-generator/shuffle.PNG)   

``` Shuffle ``` 노드는 배열 내의 값들을 무작위로 섞어준다. 그 점을 활용해 ``` Temp ```라는 배열을 하나 만들어 1부터 45까지의 수를 넣어준 다음 섞어주었다. 그 다음 앞에서 6개만 가져와 ``` Numbers ```에 넣어주었다. 이렇게하면 중복 체크 할 필요없이 바로 값을 추출해 올 수 있다는 장점이 있다.

***

## 👻 글을 마치며
이번 시간에는 블루프린트로 로또 번호를 생성하는 기능을 구현해보았다. 노드를 많이 알지 못할수록 노드의 수가 많아지고 블루프린트가 복잡해지는 것 같다. while 문을 사용할 땐 항상 제어식을 의식해야 할 것 같다. 처음 만들었을 때 count 수를 증가시켜주는 부분을 까먹어서 무한 루프 에러가 한 번 떴었다. 😂 그래도 언리얼 만질 때가 가장 재미있다. ㅎㅎ

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/data-structure/practice/BP_LottoNumGenerator)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   