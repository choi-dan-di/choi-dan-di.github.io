---
title: "[Project CatTower] #2. 달리기, 앉기 추가 & 던전 입장"
excerpt: "플레이어 달리기, 앉기 기능 추가, 던전 입장 인터랙션 구현"

categories:
  - Project CatTower
tags:
  - [Project CatTower]

permalink: /cattower/ct-2/

toc: true
toc_sticky: true

date: 2023-05-28 20:42:54+0900
last_modified_at: 2023-05-28 20:42:58+0900
published: true
---

## 👻 플레이어 기능 추가
왼쪽 Shift를 누르면 달리기(Sprint), 왼쪽 Ctrl을 누르면 앉기(Crouch) 기능을 구현해보았다.

***

### 🌱 달리기와 앉기

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/input-mapping.PNG)   

우선 Input Mapping은 위와 같이 Action Mapping으로 해주었고 코드도 동일하게 작성해주었다. 달리기 같은 경우 캐릭터 무브먼트의 MaxWalkSpeed의 값을 각각 1000, 600으로 변경해주었고 앉기 같은 경우 기본적으로 제공하는 Crouch() 함수를 이용하였다.

- **CTPlayer.h**   

```c++
public:
	// 달리기
	void BeginSprint();
	void EndSprint();

	// 쪼그려앉기
	void BeginCrouch();
	void EndCrouch();
```

- **CTPlayer.cpp**   

```c++
void ACTPlayer::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);

	// ...

	// 달리기
	PlayerInputComponent->BindAction(TEXT("Sprint"), IE_Pressed, this, &ACTPlayer::BeginSprint);
	PlayerInputComponent->BindAction(TEXT("Sprint"), IE_Released, this, &ACTPlayer::EndSprint);
	// 쪼그려 앉기
	PlayerInputComponent->BindAction(TEXT("Crouch"), IE_Pressed, this, &ACTPlayer::BeginCrouch);
	PlayerInputComponent->BindAction(TEXT("Crouch"), IE_Released, this, &ACTPlayer::EndCrouch);
}

void ACTPlayer::BeginSprint()
{
	GetCharacterMovement()->MaxWalkSpeed = 1000.0f;
}

void ACTPlayer::EndSprint()
{
	GetCharacterMovement()->MaxWalkSpeed = 600.0f;
}

void ACTPlayer::BeginCrouch()
{
	Crouch();
}

void ACTPlayer::EndCrouch()
{
	UnCrouch();
}
```

BindAction의 경우 Key Event를 추가로 입력해줘야한다.

> - **IE_Pressed** : 눌렀을 때
- **IE_Repeat** : 누르고 있을 때
- **IE_Released** : 뗐을 때

주로 Pressed와 Released를 많이 사용하는 것 같다.

Crouch의 경우 캐릭터의 Nav Movement에 있는 Can Crouch가 True인 상태에서만 동작하므로 디폴트값도 true로 세팅해주었다. 플레이어가 만들어질 때 적용되어야 하므로 생성자 함수에 구현하였다.

```c++
#include <GameFramework/CharacterMovementComponent.h>

// Crouch() 사용 가능하도록 디폴트 값을 true로 세팅
GetCharacterMovement()->GetNavAgentPropertiesRef().bCanCrouch = true;
```

GetCharacterMovement()로 캐릭터 무브먼트 컴포넌트를 가져올 수 있다. 물론 사용하려면 컴포넌트 인클루드를 해줘야 한다.

***

### 🌱 애니메이션 적용
애니메이션 블루프린트에서 Move를 일반 애니메이션이 아닌 또 다른 State Machine을 만들어서 적용시켰다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/change-state.PNG)   
![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/change-state2.PNG)   

나중에 더 추가적으로 변경할 거지만, 지금은 일단 걷는 상태에서의 달리기, 앉기 변경만 적용시켰다.

달리기의 경우 MaxWalkSpeed를 1000으로 올렸기 때문에 쉽게 speed를 600이상으로 조건을 걸었다. (기본 걷는 속도가 600으로 세팅되어있다.)

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/walk-to-sprint.PNG)   

반대는 speed가 601 이하일 경우로 설정해주었다.

앉기의 경우 애니메이션 적용하기 꽤나 힘들었는데, 내장 함수 Crouch를 사용하면 어느 변수를 이용해 확인해야 할지 감을 잡지 못 해서 그랬던 것 같다. 그래도 상태 변화와 관련된 변수를 하나 받아야 할 것 같아서 애님 헤더 파일에 다음과 같이 불리언 변수를 하나 추가해주었다.

```c++
// Crouch
UPROPERTY(EditDefaultsOnly, BlueprintReadWrite, Category = AnimCTPlayer)
	bool isCrouch = false;
```

speed와 마찬가지로 애니메이션 블루프린트에서 사용할 수 있도록 범위를 지정해 주었고, 애니메이션 블루프린트의 이벤트 그래프에서, Event Blueprint Update Animation으로 해당 변수의 값을 컨트롤 하도록 구현하였다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/update-event.PNG)   

찾아보니 캐릭터 클래스의 크라우칭 상태를 알려주는 노드가 있어서 이를 이용해서 현재 캐릭터가 앉아있는 상태인지 확인할 수 있었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/is-crouched.PNG)   

이 값을 그대로 isCrouch 변수에 저장하고, 이를 이용해서 애니메이션을 컨트롤해주었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/walk-to-crouch.PNG)   
![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/crouch-to-walk.PNG)   

앉아있다가 일어나는 상태는 isCrouch가 False값을 가지고 있기 때문에 NOT Boolean을 이용해서 값을 뒤집어 넣어주었다.

<span style="font-size: 0.7rem; color: gray;">앉는 부분 구현하는데 30분 정도 헤맸던 것 같다.. 그래도 유튜브나 구글링해서 찾아보기 전에 내가 먼저 해보았던 접근 방식이 해결 방식과 비슷해서 기분이 좋았다..😏</span>

***

### 🌱 결과

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/sprint-crouch.gif)   

***

## 👻 던전 입장하기
처음에 생각했던 것이 던전 입장 부분 근처에 가면 상호작용 키 입력 안내 문구가 뜨고, 그 키를 누르면 던전으로 이동하는 것이었다.

이를 위해서 던전 입구 부분을 블루프린트 클래스로 만들어 Box Collision을 설정해주었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/door1.PNG)   
![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/door2.PNG)   

메시는 대충 아무거나 갖다 썼다.

그 다음 박스 콜리전에 플레이어가 오버랩되면 안내 문구가 화면에 뜨도록 만들어주기 위해 User Widget을 상속받아 Widget Blueprint를 만들어주었다. 어차피 꾸미는 건 나중으로 미뤘기 때문에 간단하게 문구만 입력해주었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/wbp1.PNG)   
<span style="font-size: 0.7rem; color: gray;">너무 간단하다.</span>

그런 다음, 다시 던전 입장 블루프린트로 돌아와서 다음과 같이 블루프린트를 작성해주었다. 이것도 나중에 기회되면 코드로 옮기는 작업이 따로 필요할 것 같다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/door-bp.PNG)   

우선 객체가 생성되면 해당 위젯을 생성해 변수에 담아두고, 플레이어가 박스 콜리젼에 오버랩되면 뷰포트에 위젯을 띄우고, 벗어나면 지우는 방식으로 구현하였다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/interact1.gif)   

던전을 위해 New Level을 만들고 Stage1이라는 이름으로 만들어주었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/stage1.PNG)   

플레이어가 F를 눌러야 입장이 되므로 Input Mapping도 아래와 같이 해주었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/interact-input.PNG)   

그런 다음, 문과 플레이어의 상호 작용을 위해 블루프린트 인터페이스를 추가해주고 이를 이용해 두 블루프린트를 연결시켜준다. 우선 문의 클래스 세팅에서 인터페이스를 방금 만든 인터페이스로 설정해주었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/bpi1.PNG)   

플레이어의 블루프린트로 넘어와 조건을 충족하면 인터랙션을 실행하도록 설정해주었다. 이 부분도 C++ 코드로의 변경이 추후 필요하다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/interact2.PNG)   

오버랩 된 객체가 BPI Dungeon Entry 인터페이스를 상속받고 있으면 해당 객체의 Interact를 실행시키라는 의미같다. (솔직히 이 부분 이해가 잘 안 가서 코드로 옮기면서 확실히 이해해야 할 것 같다.)

문(던전 입장)의 Interact는 다음과 같다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/door-interact.PNG)   

여기서 그냥 한 번 더 오버래핑을 확인했다. 오버랩되는 객체가 플레이어 일 때 던전 입장이 되도록 설정하였다. 던전 입장은 Open Level을 이용하여 쉽게 구현할 수 있었다.

***

### 🌱 결과

![Alt Text](/assets/images/posts_img/projects/cattower/ct-2/interact-stage.gif)   

***

## 👻 글을 마치며
달리기와 앉기는 플레이어 기능 구현의 연장선상이라 어렵지않게 구현할 수 있었다. 던전 입장 이벤트 같은 경우 처음 응용해보았던 부분인데 생각보다 쉽게 해서 아주 재미있게 금세 만들 수 있었다. 더불어 머릿속으로만 생각했던 부분인데 직접 구현하니까 앞으로도 어떤 식으로 개발을 진행해야할지 감이 조금씩 잡히는 것 같다. 다만 인터페이스 쪽은 조금 더 많은 공부가 필요할 것 같다😂