---
title: "[Paper2D Training] #1. 배경, 캐릭터, 카메라 세팅하기"
excerpt: "Sprite, Texture의 차이점, Flipbook, Camera 세팅에 대해 알아보기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, sprite, texture, flipbook, papercharacter, character]

permalink: /paper2d-training/2d-game-basic/

toc: true
toc_sticky: true

date: 2023-01-07 00:00:41+0900
last_modified_at: 2023-01-07 00:00:45+0900
---

## 👻 배경 세팅하기
우선 2D 게임의 배경이 되는 png 파일을 구해서 아트 리소스로 추가해주었다. 초기 이미지 파일은 **텍스처(Texture)**로 되어있는데, 이 텍스처는 인게임에 바로 적용할 수 없고 어떠한 액터의 재질을 나타내는 용도 등으로 쓰일 수 있다. 이미지를 바로 배경처럼 사용하고 싶다면 텍스처 파일을 **Sprite**로 변경하는 작업이 필요하다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/apply-paper2d-texture-settings.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/create-sprite.PNG)   

우선 페이퍼2D 텍스처로 세팅해주는 작업을 한 후에 스프라이트를 생성해주면 스프라이트 파일이 새로 만들어진다. 스프라이트 파일은 인게임에 바로 적용이 가능해서 배경처럼 사용할 수 있게되는 것이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/sprite.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/sprite2.PNG)   

***

## 👻 캐릭터 세팅하기
이제 캐릭터 아트 리소스를 다운받아 추가해주었다. 움직이는 동작을 프레임으로 자른 이미지들이 모여있는 아트 리소스이다. 드래그 앤 드롭으로 쉽게 인게임에 세팅할 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/character.PNG)   

하지만 이렇게 추가해주면 움직이는 캐릭터를 만들 수 없다. 캐릭터가 움직이는 것같은 애니메이션을 추가하려면 **플립북(Flipbook)**을 생성해주어야한다.

캐릭터 아트 리소스도 배경 이미지와 동일하게 사전 작업을 해준 다음 스프라이트 파일을 생성한다. 그 다음 스프라이트 파일을 전체 선택해 우클릭하면 플립북을 생성할 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/create-flipbook.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/down_attack.gif)   

이렇게 만들어진 플립북을 드래그 앤 드롭하여 인게임에 바로 추가해줄 수 있지만, 이렇게 되면 이벤트 적용도 불가하고 오로지 하나의 플립북 모션밖에 보여주지 못한다. 이벤트에 따라 다른 모션을 나타나게 해주려면 **블루프린트 클래스**에서 플립북 모션을 적용해주면 된다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/class-sprite.PNG)   

블루프린트 클래스는 ``` PaperFlipbookActor ```를 상속받아 만들었다.

***

## 👻 카메라 세팅하기
클래스의 상세 페이지에서 ``` SpringArm ```과 ``` Camera ```를 통해 카메라를 세팅해줄 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/camera.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/camera2.PNG)   

하지만 카메라의 시점이 캐릭터를 따라가려면 부모 클래스를 캐릭터 타입으로 변경해줘야한다. 클래스의 상세 페이지에서 **Class Settings**를 클릭한 후 우측에서 부모 클래스를 ``` PaperCharacter ```로 변경해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/parent-class.PNG)   

그런 다음, 인게임에서 실행 버튼을 누르면 카메라 시점은 캐릭터를 잘 따라가지만 물리 법칙(중력)에 의해 캐릭터가 밑으로 수직 하강하게 된다. 중력을 적용하지 않기 위해 ``` Gravity Scale ```을 0으로 만들어주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/2d-game-basic/gravity-scale.PNG)   

***

## 👻 글을 마치며
이번 시간에는 배경 설정부터 캐릭터 설정, 카메라 시점 세팅까지 해보았다. 게임을 만든 결과가 눈 앞에 바로 보이니까 더욱 신기하고 재미있는 것 같다. 여기에 기능까지 구현한다면 더 흥미로워질 것 같다. 아직 언리얼 엔진 에디터 조작이 익숙치 않아 시간은 좀 오래 걸리지만 하나하나씩 알아가는 재미가 확실한 것 같다. 그리고 언리얼 엔진은 상속 관계가 아주 많이 있고 각 상황에 따라 사용해야 할 것들이 정해져 있다는 게 확실히 느껴졌다.

***

_출처_      
_[인프런 Rookies님 강의](https://inf.run/ji8q)_