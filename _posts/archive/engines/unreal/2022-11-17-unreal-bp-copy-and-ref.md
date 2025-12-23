---
title: "[Unreal Engine] BP - 복사와 참조"
excerpt: "블루프린터의 복사와 참조에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, function, copy, reference]

permalink: /unreal/bp-copy-and-ref/

toc: true
toc_sticky: true

date: 2022-11-17 01:29:05+0900
last_modified_at: 2022-11-17 01:29:09+0900
---

## 👻 들어가기에 앞서
> 두 수의 값을 바꾸는 **swap 기능**을 구현해보자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-copy-and-ref/swap.PNG)   

이번엔 함수로 구현해보자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-copy-and-ref/my-swap.PNG)   

우선 ``` Set Integer ```라는 함수를 통해 A와 B를 바꿔주었다. 브레이크 포인트를 건 후 실행시키면 A, B 값이 바뀌었다는 것을 확인할 수 있다.

> 💡 ``` Set by ~ (by ref) ```   
해당 타겟을 원하는 값으로 세팅해주는 노드이다. 함수 내부의 경우 입력값을 가져올 수 있는 방법이 없기 때문에 해당 노드를 사용하여 값을 변경해주었다.   
- **Target** : 값을 바꿔줄 기존 값
- **Value** : 타겟에 적용해줄 값

<img src="/assets/images/posts_img/engines/unreal/blueprint/function/bp-copy-and-ref/a.PNG" width="40%">
<img src="/assets/images/posts_img/engines/unreal/blueprint/function/bp-copy-and-ref/b.PNG" width="40%">

값이 잘 바뀐 걸 확인한 후 블루프린트에서 함수를 이용해 출력을 해보자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-copy-and-ref/my-swap2.PNG)   

원래라면 ``` A : 200, B : 100 ```이 나와야 하는데 ``` A : 100, B : 200 ```이 나온다.

왜 그렇게 나오는 지에 대해 이해하려면 **복사**와 **참조**의 개념을 알아볼 필요가 있다.

***

## 👻 복사
모든 변수의 값을 가져오는 방식은 **복사(Copy)**를 사용한다. ``` get ``` 노드는 해당 변수에 있는 값을 복사해서 **읽어온다**는 뜻이다.

``` My Swap ``` 함수에서 A, B의 값이 바뀐 것은 해당 함수 내에 **복사된 A, B 값만** 바꿔지게 된 것이고, 외부에 있던─우리가 값을 바꾸고 싶은─ A, B는 값이 그대로 남아있게 된 것이다.

실제 값을 건드리고 싶다면 **참조 방식**을 사용해야한다.

***

## 👻 참조
``` My Swap ``` 함수에서 실제 값을 바꾸고 싶다면, 입력값을 **참조(Reference)**하여야한다. 참조 방식으로 값을 가져오게되면 복사된 값을 변경하는 게 아닌, **실제 변수에 담겨져있는 값을 건드리게 된다.**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-copy-and-ref/ref.PNG)   

함수 정의 부분에서, 입력값을 확장시키면 ``` Pass-by-Reference ```라는 체크 부분이 있다. 이 부분을 체크해주면 값을 참조해서 가져오라는 뜻이되며, 실제 A, B 값을 조작하게 되는 것이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-copy-and-ref/ref2.PNG)   

값을 참조 방식으로 가져오게되면 핀이 **마름모** 꼴로 바뀌게 되고, **(by ref)**라는 설명이 붙게된다.

> 💡 **어떻게 가능할까?**   
참조 방식을 사용하게되면 값을 복사해서 가져오는 게 아닌, 해당 변수의 **주소값**을 가져와서 사용하게 된다. **포인터**의 개념과 유사하다.   
주소값을 가져와서 그 주소에 있는 데이터를 실제로 건드리게 되어 값이 변경되게 되는 것이다.

~~어째 다 같은 말 같다..~~

***

## 👻 글을 마치며
이번 시간에는 복사와 참조의 개념에 대해 알아보았다. 이거 학부생 때 처음 공부할 때도 굉장히 멘붕 왔었던 것 같은데, 역시나 개념 확립이 어렵긴 하다. 여기서는 간단하게 개념만 정리했지만 이 부분은 조금 더 심화학습 할 필요가 있는 것 같다. 개인적으로 공부해서 ~~지금도 개인적으로 공부 중이긴 하지만~~ 더 자세하게 정리되면 따로 포스팅 해야겠다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/function/BP_CopyAndRef)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   