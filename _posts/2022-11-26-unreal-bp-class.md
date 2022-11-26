---
title: "[Unreal Engine] BP - 블루프린트 클래스"
excerpt: "블루프린트에서 객체 지향 프로그래밍 하는 방법에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, oop, class, member, member variables, member function]

permalink: /unreal/bp-class/
 
toc: true
toc_sticky: true

date: 2022-11-26 20:52:18+0900
last_modified_at: 2022-11-26 20:52:22+0900
---

## 👻 블루프린트 클래스
블루프린트에서 객체 지향 프로그래밍의 시작인 **클래스(Class)**를 생성하고 조작하는 방법에 대해 알아보자.

***

### 🌱 클래스 생성
``` Content 👉 Blueprints ``` 폴더 내에 우클릭하면 ``` Blueprint Class ```를 통해 생성할 수 있다. 자주 사용하는 부분이기 때문에 외부에서 바로 선택할 수 있도록 만들어져있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/create-class.PNG)   

블루프린트 클래스를 생성하려 하면 상속받을 부모 클래스를 지정하라는 창이 뜬다. 지금은 ``` Actors ```를 상속받아 만들어 볼 것이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/class.PNG)   

만든 클래스를 더블클릭하면 상세 창으로 이동하게 된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/viewport.PNG)   

***

### 🌱 속성 추가
이제 클래스에 속성을 추가해보자. 좌측 상단 ``` Components ``` 부분에 있는 ``` Add ``` 를 눌러 원하는 액터를 추가시켜줄 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/add.PNG)   

**Cylinder**와 **Cube**를 추가하여 대충 아무 모양으로 만들어주었다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/class2.PNG)   

이러면 이제 실린더(Body)와 큐브(Gun)로 만들어진 플레이어 클래스가 완성된 것이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/class3.PNG)   

컴파일 및 저장 후 다시 맵으로 돌아가면 클래스의 이미지가 바뀐 것을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/class4.PNG)   
<span style="font-size: 0.7em; color: gray;">귀엽당...</span>

***

### 🌱 클래스 사용
맵에서 드래그&드랍으로 사용할 수 있다. 여러개를 소환한 후 클래스 자체를 수정해보자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/before.PNG)   

클래스를 드래그&드랍으로 불러내 맵에 세팅해보았다. 다시 클래스 화면으로 넘어가서 컴포넌트를 하나 더 추가해주자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/edit.PNG)   

대충 하나를 더 만든다음 컴파일 및 저장을 하고 맵으로 다시 돌아오면 자동으로 바뀐 것을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/after.PNG)   

이렇게 클래스를 사용하게되면 클래스 자체를 수정해도 전체를 바꿀 수 있으니 관리하기가 편하다.

이제 객체를 컨트롤 할 때에는 레벨 블루프린트를 사용하기보단 **클래스 내의 블루프린트**를 사용하게 된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/bp.PNG)   

여기서 객체를 조작할 수 있게 된다.

> 클래스(오브젝트) 타입의 변수는 **참조값**, 즉 **주소**를 가리킨다.

***

## 👻 멤버 변수
클래스가 고유하게 가지고 사용하는 변수를 의미한다. 멤버 변수는 다음과 같이 생성할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/variables.PNG)   

클래스 페이지에서 좌측 하단에 보면 변수를 추가할 수 있는데, 여기가 **멤버 변수**를 설정할 수 있는 공간이다. 블루프린트를 공부하면서 알아보았던 변수 설정 방법과 동일하다. 변수 우측에 눈 표시를 뜨게끔 만들어주면 외부에서 접근이 가능하다.

멤버 변수를 설정할 때에는 메모리의 성능을 고려하는 게 좋다.

> 멤버 함수는 힙 영역에 한 번 올라가면 여러 객체가 하나의 함수를 공유하는 방식이다. 하지만 멤버 변수는 각 객체마다 존재하기 때문에 객체의 크기는 곧 멤버 변수의 크기라 할 수 있다. 고로 수가 많아지는 걸 고려하면 멤버 변수의 수는 그리 많지 않게 설정하는 편이 메모리 관리에 좋다.

***

## 👻 멤버 함수
멤버 함수는 좌측 **Variables** 위 ``` Functions ``` 탭에서 추가 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/function.PNG)   

``` Move ```라는 함수를 추가해주어 클래스만의 전용 함수를 설정해주었다. 멤버 함수에서는 멤버 변수에 접근해서 사용이 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/level-bp.PNG)   

클래스 내에 멤버 함수를 만든다음 레벨 블루프린트에서는 반드시 해당 함수가 실행될 클래스를 명시해줘야한다.

> 💡 **SpawnActor from Class**   
- 액터를 스폰해주는 노드
- ``` Class ``` : 스폰할 클래스 선택
- ``` Spawn Transform ``` : 스폰위치 (필수값)
- ``` Collision Handling Override ``` : 스폰지점 충돌 시 예외 처리
- ``` Return Value ``` : 클래스 타입 리턴

***

## 👻 Is Valid
클래스는 값 복사 방식이 아닌 **참조** 방식을 사용한다. 고로 주소값을 가리키게 되는데, 외부에서 가져다 쓸 때 만약 해당 클래스가 비어있다면 에러가 나게 될 것이다. 이러한 에러를 방지하기 위해 **유효성 검사**를 해줘야한다. ``` Is Valid ``` 노드로 유효성 검사를 진행할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/is-valid.PNG)   

둘 다 같은 의미이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/bp-class/func2.PNG)   

함수를 만들 때 이렇게 응용할 수 있다. **레퍼런스 변수**는 항상 참조 방식을 사용한다는 걸 명심해야한다.

***

## 👻 글을 마치며
이번 시간에는 블루프린트에서 클래스를 만들고 관리하는 방법에 대해 알아보았다. 이 부분은 처음 접하는 부분이라 되게 흥미로웠다. 유효성 검사 부분에서는 레퍼런스 변수에 대한 이해가 하나도 안 됐었다.. 참조 하는 건 알겠는데 왜 참조를 하는거지..하면서 머릿속이 복잡했는데 그냥 계속 보고 보고 또 보고 검색하고 보다보니 이해가 된 것 같다. 역시 구글링을 잘 해야 지식이 쑥쑥 느는 것 같다. 😋

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/oop/BP_Class)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   