---
title: "[3D Game Training] #9. 애니메이션의 특정 타이밍에 이벤트 적용하기 (번외)"
excerpt: "애니메이션 노티파이를 이용해 애니메이션 이벤트 추가하기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, animation, blueprint, unreal, animation, notify]

permalink: /3d-game-training/animation-notify/

toc: true
toc_sticky: true

date: 2023-01-23 18:54:41+0900
last_modified_at: 2023-01-23 18:54:43+0900
---

## 👻 애니메이션 노티파이
**애니메이션 노티파이(Animation Notify)**는 **애니메이션 중간에 특정한 이벤트를 넣고 싶을 때 사용**하는 기능이다. 애니메이션 상세 페이지에서 관리할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/notifies.PNG)   

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/add-notify.PNG)   

``` Notifies ```라고 적혀진 부분이 노티파이를 관리하는 곳이고, 이전에 계속 봐왔던 초록색 블럭들도 모두 노티파이이다. ``` Add Notify ```를 눌러 원하는 노티파이를 설정할 수 있다.

> 대표적으로 사용되는 노티파이는 하단에 바로 출력된다. ``` Play Sound ```는 사운드, ``` Play Particle Effect ```는 효과를 출력시키는 노티파이이다.   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/add-play-sound.PNG)   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/anim-notify.PNG)   
``` Play Sound ``` 노티파이를 추가해 소리를 입히면 해당 프레임을 지날 때 소리가 나게 된다.

***

### 🌱 Custom Notify
``` New Notify ```를 눌러 새로운 노티파이를 생성하면 해당 스켈레톤 메쉬에 적용되는 노티파이가 새로 만들어지게 되며 스켈레톤 노티파이 추가 창에도 새로 생기게 된다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/skeleton-notify.PNG)   

> 💡 스켈레톤 노티파이는 말 그대로 해당 스켈레톤의 애니메이션 노티파이로 생성되기 때문에 완전 삭제를 하려면 스켈레톤 메쉬 페이지에서 해야한다.

이렇게 생성된 커스텀 노티파이는 **애니메이션 블루프린트**에서 호출하여 사용할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/notify-event.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/notify-event-result.gif)   
<span style="font-size: 0.7rem; color: gray;">FootStep은 "뚜벅뚜벅!"을 출력하는 Print Text 노드를 호출한다.</span>

하지만 노티파이가 많아지게되면 여기서 관리하는 것은 한계가 있다. 노티파이만 따로 관리하고 싶다면 블루프린트 클래스를 이용해야한다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/notify-bpc.PNG)   

블루프린트 클래스를 생성할 때 ``` AnimNotify ```를 상속받아 만들면 노티파이를 따로 관리할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/received-notify.PNG)   

여기서 함수를 생성할 때 오버라이드 된 함수 중 ``` Received Notify ``` 함수를 사용하면 해당 노티파이가 적용된 메쉬와 애니메이션 등을 받아 블루프린트로 개발할 수 있다. 이렇게 만들어진 블루프린트 클래스는 애니메이션 페이지에서 메인으로 사용할 수 있는 노티파이 목록에 추가된다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/add-custom-notify.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/animation-notify/custom-notify-event-result.gif)   
<span style="font-size: 0.7rem; color: gray;">AN_FootStep의 Received Notify는 "뚜벅 테스트"를 출력하는 Print Text 노드를 호출한다.</span>

***

### 🌱 Other Notifies
> - ``` Notify State ``` : **적용 범위**를 설정해줄 수 있는 노티파이
- ``` Sync Marker ``` : **블렌더 등 여러 개의 애니메이션을 사용**할 때 싱크를 맞춰줄 수 있는 노티파이

***

## 👻 글을 마치며
이번 시간에는 애니메이션 노티파이에 대해 알아보았다. 계속 궁금했던 부분이었는데 확실하게 알 수 있었던 시간이었다. 확실히 3D 게임을 만들면서 파일도 많고 창을 이동하는 횟수도 잦기 때문에 헷갈리는 부분이 없지않아 있지만 대략적인 프로그래밍의 흐름을 이해하는 데는 크게 어렵지 않은 것 같다. 한 기능을 공부할 때마다 개인 프로젝트에서 어떤 식으로 개발해야할 지 머릿속으로 정리가 되어서 좋은 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/AXLS)_