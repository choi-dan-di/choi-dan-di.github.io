---
title: "[Unreal Engine] BP - For Loop, While Loop"
excerpt: "조건에 따라 실행 흐름을 제어할 수 있는 노드(For Loop, While Loop)에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, for loop, while loop, loop]

permalink: /unreal/bp-loop/

toc: true
toc_sticky: true

date: 2022-11-14 19:15:18+0900
last_modified_at: 2022-11-14 19:15:20+0900
---

## 👻 반복문
이번 시간에는 블루프린트에서 어떻게 반복문을 표현하는지에 대해 알아보자. 반복을 하는 노드는 ``` For Loop ```과 ``` While Loop ```이 있다.

***

### 🌱 For Loop
``` C++ ```에서 ``` For문 ```과 동일한 기능을 지니는 노드이다. 인덱스의 범위를 지정해주면 알아서 증가하며 인덱스 개수만큼 반복문이 실행된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-loop/for-loop.PNG)   

> - **First Index** : 인덱스 시작값
- **Last Index** : 인덱스 종료값
- **Loop Body** : 조건을 만족할 때 반복하며 실행되는 코드
- **Index** : 현재 인덱스값
- **Completed** : 조건을 만족하지 않을 때 빠져나오는 코드

- 블루프린트   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-loop/for-loop2.PNG)   

- 결과   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-loop/for-loop-result.PNG)   

``` C++ ```과의 차이점은 **Last Index**까지 포함해서 반복한 후에 빠져나온다는 것이다.

***

### 🌱 For Loop with Break
중간에 빠져나오는 반복문을 만들고 싶으면 ``` For Loop with Break ``` 노드를 사용한다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-loop/for-loop-with-break.PNG)   

> 접근할 수 없는 랜덤 숫자값을 알고 싶을 때

- 블루프린트   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-loop/for-loop-with-break2.PNG)   

- 결과   
<img src="/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-loop/for-loop-with-break-result.PNG" width="20%">
<img src="/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-loop/for-loop-with-break-result2.PNG" width="20%">   

누를 때마다 다른 값이 나오는 걸 확인할 수 있다.

***

### 🌱 While Loop
특정 조건을 만족하면 계속 반복하는 노드를 나타낸다. 조건을 만족하지 않을 때 빠져나올 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-loop/while-loop.PNG)   

> - **Loop Body** : 조건을 만족할 때 반복하며 실행되는 코드
- **Completed** : 조건을 만족하지 않을 때 빠져나오는 코드

> 숙제를 10번 하고 놀 수 있다.

- 블루프린트   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-loop/while-loop2.PNG)   

- 결과   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-loop/while-loop-result.PNG)   

***

## 👻 글을 마치며
이번 시간에는 블루프린트에서 반복문을 어떻게 표현하는지에 대해 알아보았다. 코드보다 단순하지만 그만큼 기능이 제한되어있다는 느낌을 받았다. 중간중간에 다양한 조건을 넣으려면 이때까지 배웠던 것들을 잘 믹스해서 만들어야겠다는 생각이 들었다. 블루프린트 반복문은 일반 코드와 다르게 매 프레임마다 실행되니 인덱스가 커질수록 퍼포먼스에 영향을 미친다고 한다. 이러한 부분은 조심해서 개발해야할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/flow-control/bp-Loop)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   