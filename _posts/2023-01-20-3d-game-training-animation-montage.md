---
title: "[3D Game Training] #4. 전진 모션과 사격 모션 합치기"
excerpt: "애니메이션 몽타주를 이용해 걸어가면서 총을 쏘는 애니메이션 만들어보기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, animation, montage, blueprint, unreal]

permalink: /3d-game-training/animation-montage/

toc: true
toc_sticky: true

date: 2023-01-20 20:07:31+0900
last_modified_at: 2023-01-22 17:34:12+0900
---

## 👻 애니메이션 몽타주
마우스를 클릭하면 총을 쏘는 애니메이션을 적용해 볼 것이다. 달리는 와중에도 클릭하면 사격 애니메이션이 나올 수 있도록 만들어보자.

우선 **애니메이션 몽타주(Animation Montage)**를 생성해주었다. 몽타주는 편집 기법의 하나로 두 가지의 장면을 섞어서 새로운 장면을 만들어내는 기법이다. 지금 개발하는 과정에서는 플레이어가 앞으로 전진하면서 상체만 사격 애니메이션이 적용되도록 할 것이기 때문에 뭉타주를 사용하였다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/create-montage.PNG)   

애니메이션 몽타주는 크게 ``` Group 👉 Slot 👉 Section ```의 계층 구조로 이루어져있다. 가수의 앨범에 비유하자면 그룹이 **앨범(Album)**, 슬롯이 **트랙(Track)**, 섹션이 **노래(song)**와 비슷하다고 볼 수 있다. 또한, ``` Anim Slot Manager ```를 통해 그룹과 슬롯을 관리할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/anim-slot.PNG)   

> 💡 **Anim Slot Manager가 안 보인다면?**   
좌측 상단 ``` Window 👉 Anim Slot Manager ``` 활성화

우측 하단 ``` Asset Browser ```에서 적용하고자 하는 애니메이션을 그룹으로 **드래그 앤 드롭**하면 애니메이션이 적용되는 것을 확인할 수 있고, 여러개를 적용시키면 차례대로 애니메이션이 출력되는 것을 확인할 수 있다.

이렇게 만들어 둔 몽타주를 플레이어 쪽에 적용시켜야한다. 마우스 좌클릭 이벤트를 먼저 매핑해주고, **플레이어 컨트롤러**에 매핑한 이벤트를 만들어주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/input-fire.PNG)   

``` Fire Weapon ```이라는 함수를 따로 만들고 여기에 몽타주를 실행시키는 노드를 연결해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/fire-weapon.PNG)   

마지막으로 **애니메이션 블루프린트**에 방금 만든 슬롯을 연결해주면 완성이다. 물론 슬롯 노드에 방금 만든 슬롯을 연결해줘야한다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/slot-setting.PNG)   

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/anim-graph.PNG)   

대기, 혹은 움직임 상태를 먼저 적용시키고 마우스 좌클릭하게 되면 슬롯에 적용된 애니메이션을 실행시키는 구조라고 볼 수 있다.

***

### 🌱 애니메이션 블렌딩
위까지 진행되었을 때 확인을 해본다면 적용이 잘 되었지만 애니메이션의 연결이 부자연스러운 것을 확인할 수 있다. 달리고 있는 와중에 사격을 하게되면 머리부터 발끝까지 모두 바뀌기 때문이다. **상체는 사격 애니메이션을, 하체는 여전히 달리고 있는 애니메이션**을 적용시켜줘야 하는데, 이 때 **애니메이션 블렌딩(Animation Blending)**을 해줘야한다.

애니메이션을 섞기 위해선 ``` Layered blend per bone ```이라는 노드를 사용한다. (노드의 자세한 기능을 확인하고 싶다면 [공식 문서](https://docs.unrealengine.com/4.27/ko/AnimatingObjects/SkeletalMeshAnimation/NodeReference/Blend/)를 활용하는 습관을 길들이자.)

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/layer-setup.PNG)   

바탕이 되는 애니메이션에 **블렌딩 할 애니메이션**을 연결해주고, 해당 노드의 우측 디테일에서 적용 할 **뼈의 범위**를 설정해주면된다. 블렝딩 할 애니메이션(사격 애니메이션)은 허리 위로만 적용이 되어야 하기 때문에 기준이 되는 ``` Spine_01 ``` 부위를 입력해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/anim-graph2.PNG)   

현재 상태의 애니메이션을 저장하기 위해 **캐시(Cache)**를 이용하였다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/animation-montage.gif)   
<span style="font-size: 0.7rem; color: gray;">사격 모션이 작아서 잘 안 보이지만 잘 블렌딩 되었다.</span>

***

## 👻 줌인 & 줌아웃
마우스 휠을 사용하여 카메라를 **줌인(Zoom In) & 줌아웃(Zoom Out)**하는 기능도 붙여주었다. 마우스 휠 이벤트를 매핑해주고 플레이어 쪽에 연결시켜주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/input-wheel.PNG)   

우선 이벤트를 확인해보니 휠을 위로 올리면 양수가, 아래로 내리면 음수가 반환되는 것을 확인했고, 임의로 50 정도의 값을 정해 ``` Target Arm Length ``` 값을 세팅해주었다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-montage/wheel.gif)   

***

## 👻 글을 마치며
이번 시간에는 애니메이션 몽타주에 대해 알아보았고 어떤 기능을 하는지 연습해보았다. 애니메이션 블렌딩이 크게 어렵진 않았는데 모션이 작아서 조금 확인하는 게 힘들었다. 그리고 더 다양한 기능들이 많이 있어서 이것저것 만져보며 최대한 있는 기능들을 활용할 수 있도록 노력해야 할 것 같다. 마우스 휠로 카메라를 줌인, 줌아웃 하는 기능은 강의 커리큘럼엔 없었지만 그냥 내가 만들어보고 싶어서 만들어보았다. 금세 만들었고 내가 원하는 대로, 생각한 대로 결과물이 나와서 재미있었던 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/AXLS)_