---
title: "[Paper2D Training] #7. 타일맵 교체하기"
excerpt: "타일맵으로 교체하고 충돌 적용하기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, tilemap, map, collision]

permalink: /paper2d-training/tilemap-collision/

toc: true
toc_sticky: true

date: 2023-01-10 17:49:04+0900
last_modified_at: 2023-01-10 17:49:07+0900
---

## 👻 타일맵 교체하기
맵을 타일맵으로 교체해보자. 아트 리소스를 추가하고 처음 배경 설정했던 것과 비슷하게 파일을 설정해주면 된다. ``` Sprite Actions ```에서 ``` Apply Paper2D Texture Settings ```를 클릭해 설정을 변경해주고 이번에는 스프라이트가 아닌 **타일셋(Tile Set)**을 생성해주면 된다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/create-tile-set.PNG)   

해당 아트 리소스는 타일맵을 생성하기 위한 재료라고 볼 수 있다. 그런 다음 ``` 우클릭 👉 Paper2D 👉 Tile Map ```을 통해 타일맵을 생성해주고 위에서 생성한 타일셋을 이용해 타일맵을 만들어주면 된다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/tile-map.PNG)   

타일맵의 상세 페이지는 팔레트와 비슷하다. **Paint, Eraser, Fill**은 각각 한 칸 칠하기, 지우기, 채우기 기능을 한다. 타일셋 상세 페이지에서 한 타일의 크기를 지정할 수 있고 타일맵 상세 페이지 우측에서 맵의 크기와 기타 옵션을 설정할 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/fill-paint.PNG)   

***

### 🌱 Layer
우측에 위치한 **레이어(Layer)**를 이용해 맵을 여러 단계로 쌓아 만들 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/layer.PNG)   

이제 다시 인게임 세팅 화면으로 돌아가 만들어 둔 타일맵을 적용시키면 쉽게 맵을 교체할 수 있다.

- **결과**
![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/set-tilemap.PNG)   

***

## 👻 직교 투영
몬스터의 y축을 더 깊게 변경시키면 몬스터 이미지 자체가 커진다. 언리얼은 기본적으로 3D 개발을 위해 만들어진 엔진이기 때문에 카메라가 **원근 투영(Perspective Projection)** 방식을 기본으로 적용한다. 액터들의 y축이 달라도 동일한 크기로 보일 수 있게 하려면 카메라를 **직교 투영(Orthographic Projection)** 방식으로 변경해야 한다. 카메라가 적용된 플레이어의 상세 페이지로 들어가 ``` Camera ```의 투영 옵션을 설정해주면 된다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/orthographic.PNG)   

> - **Perspective** : 원근   
- **Orthographic** : 직교

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/projection.PNG)   
<span style="font-size: 0.7rem; color: gray;">원근 투영과 직교 투영</span>

캐릭터가 너무 크게 나온다면 ``` Ortho Width ```를 조절하여 이미지의 크기를 변경할 수 있다.

***

## 👻 충돌 적용하기
타일맵 중 벽에 해당하는 블록에 충돌 기능을 추가할 것이다. **타일셋** 상세 페이지로 들어가 콜라이더를 추가할 블록을 선택한 후 상단의 ``` Colliding Tiles ```를 클릭하면 우측에 다양한 모양의 콜라이더가 뜬다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/add-collision.PNG)   

알맞은 모양의 콜라이더를 추가해주면 된다. 아래는 ``` Add Circle ```을 클릭했을 때 나타나는 예시이고 난 ``` Add Box ```를 통해 만들어주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/add-circle.PNG)   

이제 벽 모양의 타일은 모두 콜라이더가 적용이 되었고, 타일맵 페이지로 가서 ``` Collision Thickness ```를 조절하면 **콜라이더의 두께**를 설정할 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/show-collision2.PNG)   

> 콜라이더 보기 : ``` Show 👉 Collision ``` 혹은 ``` Alt + C ```   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/show-collision.PNG)   

현재 **Fill**을 통해 벽을 모두 채워줬기 때문에 모든 타일에 콜라이더가 적용이 되어있다. 맵의 모양대로 **Eraser**를 사용해 지워줘야 맵 모양으로 콜라이더가 적용이 될 것이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/wall.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/collision.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/tilemap-collision/collision.gif)   

***

## 👻 글을 마치며
이번 시간에는 통맵을 타일맵으로 변경하고 콜라이더를 적용해보았다. 이제 점점 게임 비스무리한 게 만들어지고 있는 것 같다. 에디터가 생각보다 직관적이어서 조금만 생각하거나 구글링해보면 기능을 쉽게 찾을 수 있을 것 같다. 프로젝트의 살을 붙여나가는 재미가 쏠쏠한 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/ji8q)_