---
title: "[Unreal Engine] BP - 다형성(Polymorphism)"
excerpt: "블루프린트에서의 다형성에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, oop, polymorphism]

permalink: /unreal/bp-polymorphism/

toc: true
toc_sticky: true

date: 2023-01-05 17:36:15+0900
last_modified_at: 2023-01-05 17:36:19+0900
---

## 👻 다형성
이번 시간에는 블루프린트에서의 다형성에 대해 알아보자. 다형성을 직역하면 **다양한 형태를 가지는 특성**이다. 같은 코드를 여러 방면으로 실행되는 특성이라고 할 수 있다. 대표적으로 상속 관계에서의 **재정의(Override)**가 있다.

블루프린트에서의 함수 재정의는 아주 간단하게 할 수 있다. 우선 두 클래스를 만들어 상속 관계로 지정해주고 부모 클래스에 멤버 함수를 하나 만든다. 그런 다음 자식 클래스의 화면에서 재정의 할 함수를 지정해주면 끝이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-polymorphism/override.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-polymorphism/override2.PNG)   

이렇게 만들면 레벨 블루프린트에서 설정해 둔 코드를 바꾸지 않고 기능을 변경할 수 있게 된다. 즉, 같은 코드이지만 다르게 동작하는 다형성이 적용되는 것이다.

***

## 👻 글을 마치며
이번 시간에는 다형성에 대해 알아보았다. 이로서 객체지향의 모든 특성을 블루프린트로 실습해보고 복습할 수 있었다. 점점 머릿속의 개념이 잡히는 기분이다. 👍

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/oop/BP_Polymorphism)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   