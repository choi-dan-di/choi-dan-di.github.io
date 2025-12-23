---
title: "[Unreal Engine] BP - 은닉성(Hiding)"
excerpt: "블루프린트에서의 은닉성에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, oop, hiding]

permalink: /unreal/bp-hiding/

toc: true
toc_sticky: true

date: 2023-01-05 17:05:46+0900
last_modified_at: 2023-01-05 17:05:49+0900
---

## 👻 은닉성
핵심적인 기능만 묶어서 노출 여부를 결정하는 은닉성에 대해 알아보자. 흔히 **캡슐화(Encapsulation)**라고도 한다.

블루프린트에서는 접근제한자를 간단하게 설정할 수 있다.

***

### 🌱 변수
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-hiding/variable.PNG)   

변수같은 경우 오른쪽에 **눈 모양**의 아이콘이 있는데 이 아이콘을 이용하여 디테일 화면에서 변수에 직접 접근 유무를 설정해줄 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-hiding/private.PNG)   

또한, 해당 클래스의 상세페이지에서 우측 **Variable**의 **Private** 부분에 체크함으로써 내부에서만 사용가능하도록 설정이 가능하다. 해당 기능을 활성화하면 레벨 블루프린트에서 **Hp** 변수에 접근할 수 없다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-hiding/gethp.PNG)   

***

### 🌱 함수
멤버 함수도 마찬가지로 설정할 수 있다. 우선 **TestPublic, TestProtected, TestPrivate** 함수를 만들어 각 접근제한자를 지정해주면 쉽게 캡슐화를 할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-hiding/functions.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-hiding/public.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-hiding/test-public.PNG)   

**Public**은 외부에서 사용할 수 있지만 **Private, Protected**으로 설정하게되면 외부에서 사용할 수 없다. 단, **Protected**로 설정하게 되면 해당 클래스를 상속받은 자식 클래스 내부에서 사용이 가능하다. 아래는 자식 클래스 내에서 함수를 호출해본 경우이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-hiding/child-function.PNG)   

***

## 👻 글을 마치며
이번 시간에는 블루프린트에서 어떻게 함수들을 캡슐화하는지에 대해 알아보았다. 기초적인 이론을 알고 있으니 이해하기가 훨씬 수월했던 것 같다. 그렇지만 코드보다 더 많은 기능들이 들어가 있어서 그런지 각 기능의 정확한 적용 범위는 조금 헷갈리는 것 같다. 여러 상황으로 변경해보며 확인해봐야 할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/oop/BP_Hiding)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   