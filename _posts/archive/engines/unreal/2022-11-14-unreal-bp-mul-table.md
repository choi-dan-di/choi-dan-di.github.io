---
title: "[Unreal Engine] BP - 연습 문제 : 구구단"
excerpt: "블루프린트 반복 노드를 이용해 구구단 구현해보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, loop, multiplication table, mul]

permalink: /unreal/bp-mul-table/

toc: true
toc_sticky: true

date: 2022-11-14 19:30:10+0900
last_modified_at: 2022-11-14 19:30:12+0900
---

## 👻 구구단 구현하기
이전에 배웠던 반복문을 통해 2단부터 시작하는 구구단을 구현해보자.

- 블루프린트   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-mul-table/mul-table.PNG)   

- 결과   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-mul-table/mul-table-result.PNG)   

결과가 길어서 잘 안보이면 컨텐트 브라우저 하단 **Output Log**를 열어 확인할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-mul-table/output-log.PNG)   

- 결과(Output Log)   
<img src="/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-mul-table/log1.PNG" width="20%">
<img src="/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-mul-table/log2.PNG" width="20%">

***

### 🌱 While Loop으로 구현하기
위 방식은 ``` For Loop ```을 사용하여 구현해 본 것이다. 이번에는 ``` While Loop ```를 사용해서 한 번 구현해보자.

- 블루프린트   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-mul-table/while-loop.PNG)   

결과는 위와 동일하게 나온다.

***

## 👻 글을 마치며
이번 시간에는 반복문 예제로 구구단을 구현해보았다. For Loop을 사용해서는 금방 만들었었는데 While Loop으로는 생각보다 어려워서 조금 헤맨 것 같다. 어디부터 어디까지 루프 바디를 정하고 제어식을 어디에 연결해야할지 힘들었다. 그래도 어찌저찌 만들긴 했지만 효율적인 코드는 아니었다. 더 간단하게 만들려면 생각을 많이 해야할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/flow-control/practice/BP_MulTable)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   