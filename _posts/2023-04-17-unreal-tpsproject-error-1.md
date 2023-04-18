---
title: "[Unreal Engine] Assertion failed: Element owner passed to RegisterElementOwner was null for key 에러 고치기"
excerpt: "교재 실습 중 나타난 에러 설명 및 해결법 정리"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, TPSProject, Error]

permalink: /unreal/tpsproject-error-1/

toc: true
toc_sticky: true

date: 2023-04-17 19:25:00+0900
last_modified_at: 2023-04-17 19:25:04+0900
published: false
---

## 👻 서론
언리얼 엔진과 C++에 익숙해지기 위해 교재를 실습한 지 꽤 됐었는데 요즘 스터디를 하느라 자주 하진 못했다..(반성)

그런데 오랜만에 프로젝트를 열었더니 에러가 계속 나는 것이다. 그래서 다음 번에도 발생할 때를 대비하여 찾기 쉽게 정리를 해두려고 한다.

***

## 👻 에러 발생
프로젝트를 더블 클릭하여 열면 다음과 같은 에러 리포트를 보여주기 시작했다. ~~꽤 됐는데 귀찮아서 대충 넘겨버린..~~

![Alt Text](/assets/images/posts_img/engines/unreal/tpsproject-error-1/error1.PNG)   

가장 첫 줄은 어떤 에러이며, 발생하는 이유 그리고 해결법에 대해 알려주는 것 같은데... 스크롤을 좀 내리면 다음과 같이 정확히 어디에서 에러를 뱉어내는 지 확인이 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/tpsproject-error-1/error2.PNG)   

그래서 코드를 확인해보면 딱 다음 줄이 나오는데.. 솔직히 빌드도 잘 되고 처음 구현했을 땐 딱히 그렇다 할 에러 하나 발생하지 않았는데 왜 시간이 좀 지나고 나서 나오는지는 모르겠다..

```c++
ConstructorHelpers::FClassFinder<UAnimInstance> tempClass(TEXT("/Script/Engine.AnimBlueprint'/Game/Blueprints/ABP_Enemy.ABP_Enemy_C'"));
if (tempClass.Succeeded())
	GetMesh()->SetAnimInstanceClass(tempClass.Class);
```

***

## 👻 해결 방법 1
처음에 구글링을 좀 하다가 감이 잘 안 잡혀서 그냥 **해당 코드를 삭제하고 빌드 후 프로젝트를 열어보았다. 그러면 또 잘 실행된다..** ~~당연한 말씀..~~

그리고 다시 위 코드를 추가하고 빌드 후 컴파일까지 해봤는데 에러도 안 나고 잘 되길래 무리없이 진도를 나갔었다.

그런데 또 오늘, 같은 에러가 발생하게 된 것이다. 참고로 파일들 generate도 해 봤는데 안 돼서.. 챗GPT에게 물어보았다.

***

## 👻 해결 방법 2
요즘 챗GPT가 아주 유용하다 ㅎㅎ

![Alt Text](/assets/images/posts_img/engines/unreal/tpsproject-error-1/gpt1.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/tpsproject-error-1/gpt2.PNG)   
<span style="font-size: 0.7rem; color: gray;">절대 구글링하기 귀찮아서 물어본 거 아님</span>

결론적으로 이러한 에러 원인과 위의 코드를 비교해 보면 결국 AnimInstance를 가져오는 과정에서 성공적으로 해당 클래스를 가져오지 못 했다는 것인 것 같다. 그러면 경로가 잘못 되었거나, 경로가 잘못 되었거나.. 뭐..

그래서 일단은 해당 코드를 주석 처리하고 프로젝트를 연 다음 ABP_Enemy 파일의 경로를 다시 확인해보려한다.

![Alt Text](/assets/images/posts_img/engines/unreal/tpsproject-error-1/build.PNG)   
(언리얼은 다 좋은데 첫 빌드 시간이 너무..걸린다..맨날 이거 기다리다가 딴 길로 샌다..😅)

아무튼! 경로는 확인해 보니 딱히 틀린 부분은 없었다. 마지막에 '_C'를 붙여줘야 블루프린트 클래스로 인식을 하기 때문에 붙여준 것 빼고는 복붙했고... 이제 블루프린트 내를 한 번 더 살펴봐야 할 것 같다.

두 번째로 살펴봐야 할 부분은 애니메이션 블루프린트가 적용되어야 할 적 블루프린트에서의 AnimInstance이다.

![Alt Text](/assets/images/posts_img/engines/unreal/tpsproject-error-1/anim-class.PNG)   

내가 코드를 지정해주지 않아서 원래는 선택이 되어있지 않았는데, 다시 재설정해주고 해당 파일의 경로를 다시 복사하여 C++ 코드도 수정해 주었다. 이 후 내가 수행한 과정은 다음과 같다.

> 1. BP_Enemy의 AnimInstance 설정
2. 컴파일 & 저장
3. C++ 코드 수정(주석 처리 해제 및 경로 재확인)
4. C++ 빌드
5. 프로젝트 한 번 더 컴파일
6. 재실행

제너레이팅까지 해 봤지만 실패
~~난 바보야..~~

***

## 👻 해결 방법 3
에러 메세지부터 차근차근 다시 보기로 결정했다.

***

## 👻 글을 마치며


***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/vector-rotator/BP_Rotator)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   