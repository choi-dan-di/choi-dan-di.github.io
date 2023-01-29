---
title: "[3D Game Training] #8. 외부 애니메이션 적용하기 (번외)"
excerpt: "애니메이션 리타게팅을 이용해 기존 캐릭터에 애니메이션 추가하기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, animation, blueprint, unreal, animation, retargeting]

permalink: /3d-game-training/animation-retargeting/

toc: true
toc_sticky: true

date: 2023-01-23 17:45:31+0900
last_modified_at: 2023-01-23 17:45:33+0900
---

## 👻 애니메이션 리타게팅
이번 시간은 번외편으로, **애니메이션 리타게팅(Animation Retargeting)**에 대해 간단히 알아보자. 애니메이션 리타게팅은 **애셋에 없는 애니메이션을 적용하고 싶을 때 사용**한다. 외부에서 가져온 애니메이션의 스켈레톤 정보와 동일하게 일치시켜주면 기존의 캐릭터에 해당 애니메이션을 적용할 수 있게 된다.

``` IK Rig ```에서는 스켈레톤 메쉬의 정보를 수정할 수 있고, ``` IK Retargeter ```로 적용하고 싶은 스켈레톤 메쉬에 해당 애니메이션을 적용할 수 있다.

> **언리얼 엔진4에서 제공하는 기본 스켈레톤 메쉬의** ``` IK Rig ``` **정보**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/ue4-ik-mannequin.PNG)   

***

### 🌱 스켈레톤 메쉬 수정
- ``` IK Rig ``` **파일 생성**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/create-ik-rig.PNG)   

**애니메이션을 적용하고 싶은** 스켈레톤 메쉬에 대해 파일을 생성해주어야 한다. 파일을 만든 후 창을 열면 해당 메쉬의 스켈레톤 계층 구조를 알 수 있고, 연결하고자 하는 애니메이션을 가진 스켈레톤 메쉬의 계층 구조와 동일한 체인을 지정해줘야 원활하게 애니메이션을 이동시킬 수 있다.

- **Retarget Root 지정**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/set-retarget-root.PNG)   

변경하고자 하는 스켈레톤을 선택해 우클릭하면 리타게터 루트를 설정할 수 있다.

- **Chain 설정**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/ik-retargeting.PNG)   

이제 체인을 설정해줘야 하는데, 우측 하단 ``` IK Retargeting ``` 창의 ``` Add New Chain ```을 클릭해 직접 스텔레톤의 범위를 설정해주거나 좌측 계층 구조에서 묶을만큼 선택해 우클릭하면 체인을 만들 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/new-retarget-chain.PNG)   

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/add-new-retarget-chain.PNG)   

- **전부 설정한 모습**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/ik-retargeting2.PNG)   

***

### 🌱 애니메이션 적용
이제 ``` IK Retargeter ```를 이용해 애니메이션을 이동시켜보자. 여기서 선택하는 스켈레톤 메쉬는 적용될 메쉬가 아닌 **적용할 메쉬**를 선택해주어야 한다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/create-ik-retargeter.PNG)   

상세 페이지에서, 애니메이션을 받을 ``` IK Rig ```을 ``` Target IKRig ```에서 선택해주면 완성이다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/rigs.PNG)   

- **Preview**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/preview.PNG)   

여러 애니메이션을 눌러 잘 옮겨졌는지 확인 후 원하는 애니메이션을 **엑스포트(Export)**하면 다른 일반 애니메이션처럼 사용할 수 있게 된다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/export.PNG)   

- **엑스포트 된 애니메이션**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/retargeted-animation.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-retargeting/retargeter-result.gif)   

***

## 👻 글을 마치며
이번 시간에는 애니메이션 리타게터에 대해 알아보았다. 이제 원하는 애니메이션을 다운받아도 기존의 캐릭터에 잘 적용시킬 수 있을 것 같다. 스켈레톤 연결 정보만 동일하면 같은 애니메이션을 다른 여러개의 스켈레톤 메쉬에 적용할 수 있다는 것을 알게 되었다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/AXLS)_