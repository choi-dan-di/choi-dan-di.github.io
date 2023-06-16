---
title: "[Project CatTower] #3. 캐릭터 변경, 스킬&스탯 창, 총 추가"
excerpt: "Mixamo를 이용한 캐릭터 변경, 인터랙션 추가, 총 메시 추가"

categories:
  - Project CatTower
tags:
  - [Project CatTower]

permalink: /cattower/ct-3/

toc: true
toc_sticky: true

date: 2023-06-16 18:26:44+0900
last_modified_at: 2023-06-16 18:26:48+0900
published: true
---

## 👻 캐릭터 변경
언리얼에서 기본으로 제공하는 매니(Manny)를 사용하다가, 최종적으로는 키가 작은 고양이 캐릭터를 목표로 하고 있기 때문에 비슷한 시야 높이를 가진 캐릭터를 [Mixamo](https://www.mixamo.com/#/)에서 찾아 변경해주었다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-3/mousey.PNG)   
<span style="font-size: 0.7rem; color: gray;">키는 마음에 드는데 사륜안이 약간 무섭.. 빠른 시일 내로 모델링도 배워야겠다...</span>

다운로드한 .fbx 파일을 언리얼에 Import하게 되면 자동으로 스켈레탈 메시부터 텍스처까지 로드된다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-3/mousey2.PNG)   

현재 C++ 코드에서 스켈레탈 메시를 지정하고 있기 때문에 모델 경로도 아래와 같이 바꿔주었다.

```c++
ConstructorHelpers::FObjectFinder<USkeletalMesh> TempMesh(TEXT("/Script/Engine.SkeletalMesh'/Game/Characters/Mousey_mixamo/Mousey_mixamo.Mousey_mixamo'"));
```

기존의 캐릭터보다 키가 많이 작아서 Capsule Component의 크기가 맞지 않아 변경해주며 C++ 코드에도 추가해주었다.

- **[코드] Capsule Component의 Half Height 수정하기**

```c++
GetCapsuleComponent()->SetCapsuleHalfHeight(60.0f);
```

![Alt Text](/assets/images/posts_img/projects/cattower/ct-3/capsule.PNG)   

***

### 🌱 애니메이션 수정
ABP 파일도 싹 다 수정해주었다. 애니메이션이 스켈레탈 메시 기준으로 묶여있어서 기존의 애니메이션은 쓰기가 애매했고, 마찬가지로 Mixamo에서 총과 관련된 여러 애니메이션을 다운받아 .fbx 파일을 Import 해주었다.

~~이 부분은 애니메이션 파일만 바꿔끼운 부분이라 설명은 생략!~~

- **결과**   
적용이 아주 잘 되었다!   
![Alt Text](/assets/images/posts_img/projects/cattower/ct-3/char-change.gif)   

***

## 👻 인터랙션 추가
로비에서 스킬과 스탯을 설정할 수 있도록 각각 던전 입장과 동일하게 인터랙션을 추가해주었다.

처음에는 던전 입장, 스킬, 스탯 각각의 블루프린트 인터페이스를 추가해줘야하나 하고 파일을 만들었었는데, Interact 기능을 사용하기 위해 인터페이스 연결만 해주면 되므로 그럴 필요가 없다는 것을 깨닫고 공통 Interact Interface로 사용하였다.

스킬과 스탯도 던전 입장과 동일하게 Collision에 Overlap 됐을 때 상호작용 키를 안내하는 WBP를 출력하도록 하고, 대신 각각 알맞은 WBP를 나타나게 하도록 설정해주었다.

- **스킬**   
![Alt Text](/assets/images/posts_img/projects/cattower/ct-3/skill.PNG)   

- **스탯**   
![Alt Text](/assets/images/posts_img/projects/cattower/ct-3/stat.PNG)   

잘 보이진 않지만 각각 WBP_Skill, WBP_Stat을 생성하도록 연결해주었다.

내용은 기획쪽의 비중이 높으므로 보류..   
<img src="/assets/images/posts_img/mokoko/08.png" width="20%">

추가로 창을 열었을 때 게임을 일시정지 할 수 있도록 **Set Game Paused** 노드를 이용하였다. 이 부분과 플레이어의 **InputAction Interact** 부분은 코드로 옮기는 작업이 추가로 필요할 것 같다. (언제하지?)

- **결과**   
![Alt Text](/assets/images/posts_img/projects/cattower/ct-3/skill-play.gif)   
<span style="font-size: 0.7rem; color: gray;">이래봬도 잘 멈췄다!</span>

***

## 👻 총 메시 추가
플레이어에게 총을 쥐어주었다. 총 모델은 에픽 런처의 무료 번들을 이용하였다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-3/weapon-bundle.PNG)   

캐릭터의 **스켈레톤(Skeleton)** 화면에서 **소켓(Socket)**을 추가해주었다. 이 소켓에 총 메시를 붙이면 자연스럽게 손에 총이 쥐어질 것이다.

![Alt Text](/assets/images/posts_img/projects/cattower/ct-3/socket.PNG)   

소켓을 만들고, C++로 넘어와 총 메시를 추가해주는 코드를 작성해주었다.

- **CTPlayer.h**

```c++
// 총 스켈레탈 메시 추가하기
UPROPERTY(VisibleAnywhere, Category = GunMesh)
    class USkeletalMeshComponent* gunMeshComp;
```

- **CTPlayer.cpp**

```c++
// 총 스켈레탈 메시 추가
gunMeshComp = CreateDefaultSubobject<USkeletalMeshComponent>(TEXT("GunMeshComp"));
// 부모 컴포넌트를 소켓으로 설정
gunMeshComp->SetupAttachment(GetMesh(), TEXT("RightHandSocket"));
// 총은 AK47로 세팅
ConstructorHelpers::FObjectFinder<USkeletalMesh> TempGunMesh(TEXT("/Script/Engine.SkeletalMesh'/Game/FPS_Weapon_Bundle/Weapons/Meshes/Ka47/SK_KA47.SK_KA47'"));
if (TempGunMesh.Succeeded())
{
    gunMeshComp->SetSkeletalMesh(TempGunMesh.Object);
    gunMeshComp->SetRelativeRotation(FRotator(90, 180, 0));
    gunMeshComp->SetRelativeScale3D(FVector(0.7f));
}
```

cpp 코드는 **생성자**에 추가해주었고 캐릭터 스켈레탈 메시를 추가하는 것과 동일하게 작성했다. 단, 부모 컴포넌트를 루트 컴포넌트가 아닌 소켓으로 해줘야하기 때문에 한 줄이 더 추가되었다.

> 이 부분에서 코드를 아무리 수정해도 소켓을 찾지 못하고 적용이 안 되는 에러가 발생했다. 구글링해보니 틀린 건 없었는데 안 되길래 **SetupAttachment**를 **AttachToComponent**로도 바꿔보고 별 짓을 다 해본 것 같다.
>
> 결국 BP_CTPlayer 삭제 후 재생성, 결과는 잘 적용됨. 현재까지 원인 몰루겠음.. ~~언리얼 버그였을까?~~
>
> <img src="/assets/images/posts_img/mokoko/21.png" width="20%">

- **결과**   
![Alt Text](/assets/images/posts_img/projects/cattower/ct-3/gun-mesh.gif)   

***

## 👻 해결해야 할 문제들...
- **Refactoring**
    - 현재 BP로 코딩되어있는 부분을 코드로 옮기는 작업이 필요할 듯
    - CTPlayer 파일도 좀 길어지는데.. 분리시켜볼까?
- **총 메시 적용안 된 원인 찾기**
    - 소켓 적용 안 됐던 현상
    - **SetupAttachment**와 **AttachToComponent** 정확한 기능 공부하기

그리고 ...

- **스킬&스탯 구성하기**
- **Stage 1 맵 제작하기**
- **기획서 세부사항 작성**
    - 정확히 어떤 식으로 플레이가 진행되는지

***

## 👻 글을 마치며
사실 위 작업 모두 5월 29일, 31일에 한 거였는데 미루고 미루다 이제 정리한다 😅 (반성해라) 인터랙션 작업을 반복 수행하다보니 어떤 순서로 작업이 수행되는지 익히게 된 것 같고 소켓 사용법도 조금은 익힌 것 같다. 얼른 기획 먼저 확실하게 정리해서 빨리빨리 진도를 나가야 할 것 같음!