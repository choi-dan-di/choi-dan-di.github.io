---
title: "[Project CatTower] #1. 로비 제작 & 플레이어 이동 기능 구현"
excerpt: "Landscape를 이용한 로비 제작과 Input Mapping을 이용한 플레이어 이동 기능 구현"

categories:
  - Project CatTower
tags:
  - [Project CatTower]

permalink: /cattower/ct-1/

toc: true
toc_sticky: true

date: 2023-05-28 17:06:19+0900
last_modified_at: 2023-05-28 17:06:22+0900
published: true
---

## 👻 랜드스케이프 생성
New Level에서 Landscape Mode로 첫 랜드스케이프를 생성하고 Material을 생성해 직접 설정해주었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/landscape.PNG)   

맵 같은 경우는 급한 부분이 아니기 때문에 대충 푸릇한 Grass 부분, 모래 질감의 Soil 부분만 설정하였다. 차차 이쁘게 바꿀 예정.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/m-texture1.PNG)   
![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/m-texture2.PNG)   

텍스처는 언리얼의 StarterContent에서 기본으로 제공하는 Grass 텍스처와 개인적으로 다운받은 무료 애셋에 있는 Dirt 텍스처를 사용하였다. 두 텍스처에 대한 노멀맵도 같이 사용하였다. 이 두 텍스처를 한 머티리얼에서 사용해야하기 때문에 Layer Blend를 이용하였다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/layer-blend.PNG)   

이 부분은 [언리얼 Doc](https://docs.unrealengine.com/4.27/ko/BuildingWorlds/Landscape/QuickStart/4/)을 참고했는데, 그대로 따라해서 Soil을 LB Height Blend로 설정해 사용했더니 텍스처가 제대로 나오지 않는 현상이 발생했다. 그래서 그냥 이것저것 해보다가 둘 다 똑같이 Weight Blend 타입으로 설정했더니 잘 되긴 되는데 워닝이 많이 발생해서 바꿔야할 것 같다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/map-check.PNG)   

<img src="/assets/images/posts_img/mokoko/45.png" width="20%">   

그렇게 완성(?)된 로비

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/map.PNG)   
<span style="font-size: 0.7rem; color: gray;">허허벌판..초라..머쓱..</span>

건물 등의 무료 애셋은 다음과 같다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/free-asset.PNG)   

> 던전을 먼저 구현하고 맵을 수정할 예정이다.

***

## 👻 플레이어 기능 구현
현재 언리얼에서 제공하는 마네킹과 애니메이션 스타터 팩을 사용해서 아이들 상태 및 이동 상태에만 애니메이션을 붙인 상태이다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/cpp-menu.PNG)   

위 C++ 코드로 생성한 클래스를 상속받은 블루프린트 클래스를 이용하여 나머지를 설정해주었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/bp-menu.PNG)   

***

### 🌱 스켈레탈 메시 설정
플레이어에 사용할 스켈레탈 메시를 C++ 코드로 설정해주었다. 다음은 플레이어 생성자 내에서 설정한 코드 부분이다.

```c++
// 스켈레탈 메시 설정
ConstructorHelpers::FObjectFinder<USkeletalMesh> TempMesh(TEXT("/Script/Engine.SkeletalMesh'/Game/AnimStarterPack/UE4_Mannequin/Mesh/SK_Mannequin.SK_Mannequin'"));
if (TempMesh.Succeeded())
{
    GetMesh()->SetSkeletalMesh(TempMesh.Object);
    GetMesh()->SetRelativeLocationAndRotation(FVector(0, 0, -90), FRotator(0, -90, 0));
}
```

스켈레탈 메시 경로는 해당 메시 선택 후 Ctrl + C로 복사하였다.

ConstructorHelpers의 FObjectFinder를 사용해 메시를 불러오고, 성공적으로 불러왔다면 현재 메시의 스켈레탈 메시를 SetSkeletalMesh를 이용해 해당 메시로 변경해주고 SetRelativeLocationAndRotation을 이용해 초기 Location, Rotation을 설정해주었다.

💡   
블루프린트 클래스에서는 X, Y, Z 순으로 각각 Roll, Pitch, Yaw 순서를 의미한다. 단, C++ 코드에서는 FRotator가 Pitch, Yaw, Roll 순으로 파라미터를 입력받는 것에 주의해야한다. 이것 때문에 메시가 맨날 누워있었더라는..

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/frotator.PNG)   

***

### 🌱 3인칭 카메라 설정
- **CTPlayer.h**

```c++
public:
	// 3인칭 카메라 붙이기
	UPROPERTY(VisibleAnywhere, Category = Camera)
		class USpringArmComponent* springArmComp;
	UPROPERTY(VisibleAnywhere, Category = Camera)
		class UCameraComponent* tpsCamComp;
```

- **CPTlayer.cpp**   

```c++
#include <GameFramework/SpringArmComponent.h>
#include <Camera/CameraComponent.h>

// 3인칭 카메라 설정
springArmComp = CreateDefaultSubobject<USpringArmComponent>(TEXT("SpringArmComp"));
springArmComp->SetupAttachment(RootComponent);
springArmComp->SetRelativeLocationAndRotation(FVector(18, 100, 100), FRotator(0, -10, 0));
springArmComp->TargetArmLength = 250;
// SpringArm에 카메라 붙이기
tpsCamComp = CreateDefaultSubobject<UCameraComponent>(TEXT("TpsCamComp"));
tpsCamComp->SetupAttachment(springArmComp);
```

이 부분도 플레이어 생성자 내에 구현하였고 스프링암 컴포넌트와 카메라 컴포넌트를 사용하기 위해 위 두 헤더를 include 해주었다.

먼저 CreateDefaultSubobject로 컴포넌트를 생성해주고, SetupAttachment로 다른 컴포넌트에 붙여주었다.

***

### 🌱 이동 및 회전
- **CTPlayer.h**

```c++
public:
	// 이동 입력 처리
	UPROPERTY(EditAnywhere, Category = PlayerSetting)
		float walkSpeed = 600;
	// 이동 방향(방향 벡터)
	FVector direction;

	// value는 Input Mapping에서 설정한 값이 들어옴
	// 상하좌우 : 1, -1, -1, 1
	// 좌우 이동
	void InputAD(float value);
	// 상하 이동
	void InputWS(float value);

	// 회전 입력 처리
	void LookUp(float value);
	void Turn(float value);

	// 점프
	void InputJump();
```

- **CPTlayer.cpp**   

```c++
void ACTPlayer::InputAD(float value)
{
	direction.Y = value;
}

void ACTPlayer::InputWS(float value)
{
	direction.X = value;
}

void ACTPlayer::LookUp(float value)
{
	AddControllerPitchInput(value);
}

void ACTPlayer::Turn(float value)
{
	AddControllerYawInput(value);
}

void ACTPlayer::InputJump()
{
	Jump();
}
```

상하(WS), 좌우(AD) 키보드 입력에 따라 실행할 함수 InputAD, InputWS는 입력 시에 Scale에 해당하는 방향 벡터를 설정해준다. 회전은 마우스의 X, Y축이 움직임에 따라 실행되는 함수 LookUp, Turn이 정의되었고 이는 캐릭터의 Pitch, Yaw의 값을 변경해준다.

캐릭터 점프는 Character Movement에 내장되어있는 Jump() 함수를 호출하는 것으로 구현하였다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/input-mapping.PNG)   

입력이 들어오면 Tick 함수를 이용해 실시간으로 플레이어가 이동할 수 있도록 처리한다.

```c++
void ACTPlayer::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);
	// 플레이어 이동 처리
	// 방향 벡터를 내 기준으로 변환
	direction = FTransform(GetControlRotation()).TransformVector(direction);
	AddMovementInput(direction);
	// 방향 벡터 초기화
	direction = FVector::ZeroVector;
}
```

방향 벡터를 월드 좌표 기준이 아닌 플레이어 좌표 기준이 되어야 하므로 FTransform을 이용해 방향 벡터를 플레이어 기준으로 변경하고 AddMovementInput을 이용해 현재 벡터에 바로 방향 벡터가 적용될 수 있도록 해주었다. 그런 다음 방향 벡터를 제로 벡터로 초기화하여 다음 입력에 대비하였다.

입력 매핑은 바인딩을 해줘야하므로 SetupPlayerInputComponent도 다음과 같이 코드를 추가하였다.

```c++
void ACTPlayer::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);

	// 좌우 입력 이벤트 처리 함수 바인딩
	PlayerInputComponent->BindAxis(TEXT("Horizontal"), this, &ACTPlayer::InputAD);
	// 상하 입력 이벤트 처리 함수 바인딩
	PlayerInputComponent->BindAxis(TEXT("Vertical"), this, &ACTPlayer::InputWS);

	// 회전
	PlayerInputComponent->BindAxis(TEXT("LookUp"), this, &ACTPlayer::LookUp);
	PlayerInputComponent->BindAxis(TEXT("Turn"), this, &ACTPlayer::Turn);

	// 점프
	PlayerInputComponent->BindAction(TEXT("Jump"), IE_Pressed, this, &ACTPlayer::InputJump);
}
```

축(Axis)은 BindAxis, 액션(Action)은 BindAction을 사용한다.

마지막으로 플레이어가 회전할 때 카메라도 같이 회전하기 위해 생성자 함수 내에 다음과 같은 코드를 추가하였다.

```c++
// 플레이어 회전시 카메라도 같이 회전(스프링암만 회전)
springArmComp->bUsePawnControlRotation = true;
tpsCamComp->bUsePawnControlRotation = false;
bUseControllerRotationYaw = true;
```

플레이어는 Yaw 회전만 허용할 것이고, 카메라는 스프링암 컴포넌트에 붙어있기 때문에 굳이 회전시킬 이유가 없으므로 false로 설정하였다.

***

### 🌱 애니메이션 설정
에픽에서 제공하는 AnimStarterPack이라는 무료 애셋을 이용해 대충 붙여보았다. 어차피 나중에 더 찾아서 내가 원하는 애니메이션을 붙일 거라 긴 시간을 소모하진 않았다. (팔 쫙 뻗은 상태로 돌아다니면 이상하니까..)

플레이어 애니메이션에서 사용할 변수를 관리하기위해 AnimInstance를 상속받은 C++ 클래스를 만들어주었고, 그 클래스를 상속 받은 애니메이션 블루프린트 클래스(ABP)를 만들어서 구현하였다.

- **AnimCTPlayer.h**   

```c++
public:
	// 플레이어 이동 속도
	UPROPERTY(EditDefaultsOnly, BlueprintReadWrite, Category = AnimCTPlayer)
		float speed = 0;
```

현재 이동에만 애니메이션이 들어가기 때문에 플레이어 이동 속도 변수 하나만 추가해주었다.
UPROPERTY 내에 블루프린트에서 읽고 쓸 수 있도록 설정해주었고, ABP에서 해당 변수(speed)를 가져와서 애니메이션 구현에 사용하였다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/abp-variables.PNG)   

부모에게 상속받은 변수를 사용하려면 좌측 My Blueprint 창에서 해당 옵션값을 활성화시켜줘야한다. 그래야 변수가 보이고 블루프린트에서 사용할 수 있음!

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/anim-graph1.PNG)   

애님 그래프는 위와 같으며 MoveFSM은 아래와 같다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/move-fsm1.PNG)   

각각 상태에 맞는 애니메이션을 연결해주었고(서 있는 애니메이션이랑 앞으로 걸어가는 애니메이션), Idle과 Move 사이의 상태는 Speed가 0보다 크거나 0.1 보다 작을 때로 설정해주었다.

마지막으로 Event Graph에서 이 블루프린트가 애니메이션을 업데이트할 때 스피드를 변경해주는 것으로 마무리지었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/event-graph.PNG)   

Get Velocity는 플레이어의 현재 속도를 벡터 형식으로 가져온다. 이를 Vector Length를 이용해 float로 변환 후 speed 변수에 설정해주었다.

***

## 👻 결과
![Alt Text](/assets/images/posts_img/projects/cattower/ct-1/player-move.gif)   

***

## 👻 글을 마치며
책 보고 공부했었던 내용 그자체지만 랜드스케이프 경우는 내가 처음 만져보았던 기능이었다. 직접 내가 내 손으로 원하는 맵을 만들 수 있어서 재미있었고, 머티리얼 생성의 경우 그래픽스 공부가 많은 도움이 되었던 것 같다. 공부하기 전에 봤으면 노멀이 뭔지도 몰랐을텐데 텍스처와 노멀맵이 어떤 식으로 동작하는지 아니까 손쉽게 만들 수 있었던 것 같다. 한 가지 아쉬운 건 캐릭터인데.. 모델링을 먼저 하고 싶은데 시간 너무 잡아 먹을까봐 손도 못 대는 중 ㅠㅠ