---
title: "[Unreal Engine] BP - 연습 문제 : 총알 재장전"
excerpt: "블루프린트로 총알 재장전하는 기능 구현해보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, practice, reload-bullet]

permalink: /unreal/bp-reload-bullet/

toc: true
toc_sticky: true

date: 2022-11-13 18:08:03+0900
last_modified_at: 2022-11-13 18:08:05+0900
---

## 👻 총알 재장전 기능 만들기
이번 시간에는 블루프린트를 이용하여 총알을 재장전하는 기능을 만들어보자.

> - 총알은 기본 30발이 주어진다.
- 좌클릭을 할 때마다 1발씩 줄어든다.
- 0발이 되면 더 이상 쏠 수 없다.
- R 키를 누르면 총알이 재장전된다.

***

### 🌱 총알 쏘기
좌클릭으로 총알을 쏘는 기능을 구현해보자.   
그 전에 ``` Ammo ```라는 변수를 만들고 **Default Value**를 30으로 설정해뒀다.

- 블루프린트   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/practice/bp-reload-bullet/shooting.PNG)   

맨 처음 실행했을 때 총알 개수를 표현해주고 싶어서 **Event BeginPlay**도 연결시켜주었고, **Print Text**로 총알 개수를 화면에 나타내주었다.   
총알 개수를 표현할 때 앞부분에 **'Fire! Ammo :'**라는 문구를 추가시켜주기 위해 **Format Text** 노드를 사용하였고, 좌클릭 시 총알 개수를 1씩 감소시켜주었다.

**마우스 좌클릭 이벤트**는 ``` Left Mouse Button ``` 노드를 추가해줌으로써 사용할 수 있다.

> **Left Mouse Button**   
- ``` Pressed ``` : 좌클릭 했을 때 발생하는 이벤트
- ``` Released ``` : 클릭을 뗐을 때 발생하는 이벤트
- ``` Key ``` : 좌클릭 시 발생하는 이벤트의 정보

- 결과   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/practice/bp-reload-bullet/shooting-result.PNG)   

***

### 🌱 총알 유무 판단하기
위처럼 만드니 총알은 한 발씩 잘 감소되지만 0발일 때 좌클릭을 하면 **-1, -2, -3, ...** 이렇게 끝도 없이 총알 개수가 감소된다. 이러한 결과를 방지하기위해 총알이 0발이면 더이상 좌클릭 이벤트가 실행되지 않는 기능을 추가해줘야한다.

- 블루프린트   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/practice/bp-reload-bullet/no-bullet.PNG)   

좌클릭 시 총알의 개수를 확인하기 위해 브랜치를 중간에 추가해주었다. 총알 개수가 최소 1개면 좌클릭 이벤트를 그대로 실행하고, 0개면 총알이 없다는 문구를 출력해보았다.

- 결과   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/practice/bp-reload-bullet/no-bullet-result.PNG)   

> 💡 **1씩 증가하거나 빼주는 노드**   
**증감연산자**처럼 알아서 증가, 감소를 시켜주는 노드가 있다. ``` ++ ```, ``` -- ```로 검색한다.   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/practice/bp-reload-bullet/inc-dec.PNG)   
이 노드를 사용하면 **연산 노드**와 **Set 노드**를 합칠 수 있다.

***

### 🌱 총알 재장전하기
이제 R 키를 눌러 총알을 재장전하는 기능을 구현해보자. 이미 총알이 풀이면 재장전 하는 게 아니라 문구를 띄워줄 것이다.

- 블루프린트   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/practice/bp-reload-bullet/reload-bullet.PNG)   

R 키를 눌렀을 때 총알이 재장전되려면 우선 총알의 풀 개수를 저장해둬야한다. ``` MaxAmmo ``` 변수를 새로 만들어 똑같이 30개를 기본 값으로 넣어주고, R 키를 누를 때마다 현재 ``` Ammo ```와 ``` MaxAmmo ```의 값을 비교해줬다. 값이 같지 않으면 재장전을 해주고, 이미 풀이면 **Full Ammo!**라는 문구를 출력해주었다.

**키 입력 이벤트** 노드는 ``` Keyboard Events [원하는 키] ```를 검색하면 사용할 수 있다.

- 결과   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/practice/bp-reload-bullet/reload-bullet-result.PNG)   

완성이다!

***

## 👻 글을 마치며
이번 시간에는 총알을 재장전하는 기능을 구현해보며 블루프린트를 연습해보았다. 확실히 노드들이 많아지니까 순서 정하는 것이 가장 어려웠다. 그래도 천천히 생각해보니 쉽게 구현할 수 있었던 것 같다. 중복되는 노드들을 합쳐서 줄일 수 있을 것 같은데 ~~위험한지 아닌지 아직은 잘 모르겠어서~~ 이 부분은 공부가 좀 더 필요할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/data-control/practice/BP_ReloadBullet)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   