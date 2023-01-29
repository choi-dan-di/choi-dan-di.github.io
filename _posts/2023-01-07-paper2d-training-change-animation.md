---
title: "[Paper2D Training] #2. 캐릭터 애니메이션 변경하기"
excerpt: "입력키에 따른 캐릭터 애니메이션 변경해보기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, animation, change animation, macro, mappings, input]

permalink: /paper2d-training/change-animation/

toc: true
toc_sticky: true

date: 2023-01-07 16:50:34+0900
last_modified_at: 2023-01-07 16:50:38+0900
---

## 👻 애니메이션 변경하기
이번 시간에는 입력키에 따라 스프라이트를 변경해 볼 것이다. 대기 중인 상태의 ``` _idle ``` 플립북을 입력키에 따라 알맞게 세팅해주면 잘 실행이 될 것 같다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/change-animation/wsad.PNG)   

``` BP_Knight ```의 클래스 상세 페이지에 추가해주었다. ``` side_idle ```은 오른쪽만 바라보는 이미지이므로 왼쪽 방향을 가리키는 **A키**를 누르면 z 축으로 180도 회전하는 노드를 추가해주면서 쉽게 완성할 수 있었다.

> ![Alt Text](/assets/images/posts_img/projects/paper2d-training/change-animation/set-relative-rotation.PNG)   
>
> ``` Set Relative Rotation ``` 노드를 통해 스프라이트의 회전값을 세팅해 줄 수 있다.

하지만 이런 식으로 일일이 입력 하나하나에 노드를 연결하는 것은 좋지 못한 개발 방식이다. 매번 누르는 것을 확인한 다음 플립북을 세팅해주는 방식은 프로젝트의 스케일이 커질수록 비효율적이다. 이러한 단점을 보완하기 위해 **입력 매핑(Input Mapping)** 방식을 사용한다.

***

### 🌱 입력 매핑
``` Edit 👉 Project Settings 👉 Engine 👉 Input ```에서 **키 매핑**을 해줄 수 있다. 일일이 코드를 구현하지 않아도 입력 키에 따라 자동으로 매핑시켜 원하는 동작을 바로 수행할 수 있게 해준다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/change-animation/mappings.PNG)   

**Action Mappings, Axis Mappings**로 입력 매핑을 해줄 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/change-animation/action-mappings.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/change-animation/axis-mappings.PNG)   

**Action Mappings**는 Key의 구조체를 반환하고, **Axis Mappings**는 설정한 스케일의 값을 반환한다. 이러한 반환값을 통해 이벤트의 흐름을 간단하게 제어할 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/change-animation/input-axis.PNG)   

세팅 화면에서 설정해 둔 매핑의 이름으로 해당 이벤트를 블루프린트에서 호출할 수 있으며 분기문을 이용해 알맞은 플립북을 세팅해 주었다.

***

## 👻 매크로
위에서 설정한 분기문이 살짝 복잡한데, 조금 더 간편하게 해줄 수 있는 노드가 있다.

```
Compare [Type]
```

![Alt Text](/assets/images/posts_img/projects/paper2d-training/change-animation/compare-float.PNG)   

값의 비교와 동시에 출력 분기문을 설정해줄 수 있는 노드이다. 해당 노드는 **f**가 아닌 **M**이 붙어있는 것을 확인할 수 있는데, 이는 **매크로(Macro)**를 의미한다.

***

### 🌱 함수 vs 매크로
함수와 매크로는 언뜻 보면 같은 기능을 하는 노드로 보여진다. 실제로도 기능은 거의 동일하게 작동되지만 큰 차이점이 하나 존재한다.

바로 함수는 실행 후 종료가 될 때까지 연속적으로 코드의 실행이 일어나지만 매크로는 그렇지 않다는 점이다. 함수 내에서는 실행 시간을 지연시키는 ``` Delay ``` 노드를 사용할 수 없다. 하지만 매크로는 가능하다. 이러한 점이 첫 번째고, 두 번째는 함수는 다른 주소값을 가지는 공간에 프로그램 실행 시 한 번만 생성되어 함수 호출 시 해당 주소값으로 넘어갔다 오는 방식이지만 매크로는 실행 시점에 코드들이 전부 복사되어 실행되는 점이다. 매크로에 수천 개의 노드가 있다면 복사하는 데 많은 부담이 되니 주의해야 할 것 같다.

***

## 👻 글을 마치며
이번 시간에는 입력에 따른 캐릭터의 다양한 애니메이션을 세팅해보았다. 여기까지 하니 진짜 캐릭터가 살아움직이는 것 같은 기분이 들었다. 그리고 생각보다 어렵지 않았다. 기초 공부를 확실하게 한 보람이 있는 것 같다. 이제 따라치는 것만이 아닌, 스스로 생각하며 직접 해보는 시간을 더 많이 가져야 할 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/ji8q)_