---
title: "[Computer Graphics] #1. 서론"
excerpt: "한정현 컴퓨터 그래픽스 강의 1장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg]

permalink: /computer-graphics/introduction/

toc: true
toc_sticky: true

date: 2023-03-20 17:07:20+0900
last_modified_at: 2023-03-20 17:07:24+0900
---

## 👻 3차원 컴퓨터 그래픽스
**컴퓨터 그래픽스**란, 컴퓨터를 이용하여 영상(image)을 생성하는 작업을 의미한다. **3차원 컴퓨터 그래픽스**는 이 작업에서 **3차원으로 표현된 물체를 입력으로 받아 2차원 영상으로 출력하는 작업**을 의미한다. 이러한 2차원 영상을 **프레임(frame)**이라 하는데 이 프레임들을 연속적으로 보여주면 물체가 움직이는 듯한 느낌을 줄 수 있다.

그래픽스에는 크게 **실시간 그래픽스**와 **비실시간 그래픽스**로 나뉜다. 실시간 그래픽스의 대표적인 예는 **게임**이며 성능은 1초당 몇 프레임을 만들어낼 수 있느냐, 즉 **frames per second(fps)**로 측정할 수 있다. 보통 초당 30 프레임 이상을 만들어내야 한다. 게임 뿐 아니라 AR/VR 등도 실시간 그래픽스에 속한다.

반면, 비실시간 그래픽스는 영화에서 쓰이는 **특수 효과**가 대표적이다. 해당 작업은 실사 영상과 구분이 되지 않는 사진 사실적인 영상을 만들어내는 것이 목표이며 상당히 많은 연산을 수행한다.

두 그래픽스는 알고리즘으로 구분할 수 있으며, 앞으로 알아볼 내용은 주로 실시간 그래픽스에 관한 내용이다.

***

## 👻 컴퓨터 그래픽스 제작 단계
컴퓨터 그래픽스는 보통 **5단계**로 구분된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/introduction/cg-stage.png)   

모델링, 리깅, 애니메이션 제작의 단계는 **그래픽 아티스트** 혹은 **그래픽 디자이너**가 오프라인에서 수행하는 단계이다.

만들어진 애니메이션 재생, 렌더링, 후처리의 단계는 **런타임(Run-Time)** 때 수행되며 컴퓨터가 자동으로 실행하는 단계이다.

***

### 🌱 Modeling
**모델(Model)**이란, 컴퓨터가 이해하고 처리할 수 있는 형태로 표현한 물체를 의미한다. **모델링(Modeling)** 단계에서는 바로 이러한 모델들을 만들어 낸다.

보통 모델은 **폴리곤(Polygon; 다각형)**들로 구성이 되며, 이렇게 폴리곤으로 구성된 물체를 **폴리곤 메시(Polygon Mesh)**라고 한다. 다각형 중 삼각형이 가장 간단한 구조이므로 삼각형 메시가 가장 많이 쓰인다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/introduction/model.png)   

이렇게 만들어진 폴리곤 메시에 입혀서 시작적 사실성을 높여주는 **텍스처(Texture)** 제작도 모델링 과정에 속한다. 이러한 텍스처를 만들어 폴리곤 메시에 입히는 것을 **텍스처링(Texturing)**이라고 한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/introduction/texture.png)   

위의 이미지에서 왼쪽에 위치한 이미지가 **이미지 텍스처**의 한 예이고, 가장 간단하면서도 가장 많이 사용되는 텍스처 형식이다.

***

### 🌱 Rigging
위에서 만든 모델의 움직임을 표현하고 싶을 때, 가장 간단하게 사용할 수 있는 것은 모델의 **골격(Skeleton)**을 구성한 후, 각각의 **뼈(bone)**를 움직이게 하는 것이다. 골격을 폴리곤 메시에 삽입하면 애니메이션을 입힐 수 있다.

애니메이션을 위해 **폴리곤 메시와 스켈레톤의 상관 관계를 정하는 작업**을 **리깅(Rigging)**이라고 한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/introduction/rigging.png)   

***

### 🌱 Animation
뼈를 이용한 애니메이션을 **스켈리탈 모션(Skeletal Motion)**이라한다. **애니메이션(Animation)**은 리깅 단계를 거치고 난 폴리곤 메쉬의 애니메이션을 프레임 별로 설정해주는 단계이다. 기본적으로 며칠에서, 많게는 몇달이 걸릴 수도 있는 작업 단계이다. 모든 작업이 끝나고 나면 프레임 별로 애니메이션이 생성되게 될 것이고, 이렇게 만들어진 애니메이션은 런타임에 프레임 별로 재생이 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/introduction/animation.png)   

***

### 🌱 Rendering
**렌더링(Rendering)**은 런타일 때 만들어진 모델을 애니메이트 시키는 과정이다. 앞서 모델링 단계에서 만들었던 텍스처가 입혀지는 과정인 **텍스처링(Texturing)**이 기본적으로 수행되어야 하고, 조금 더 사실적으로 묘사하기 위해 빛과 물체의 상호 작용을 처리하는 **라이팅(Lighting)**이 그 핵심을 이룬다. 라이팅을 적용시키면 그림자 등을 제작할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/introduction/rendering.png)   

***

### 🌱 Post-Processing
**후처리(Post-Processing)**은 렌더링과 같이 필수적인 단계는 아니고 **선택적으로 수행되는 단계**이다. 앞서 만들었던 결과물에 더하여 조금 더 사실적으로 표현하기 위해 수행된다. 빠른 속도로 진행되는 애니메이션의 경우, 한 프레임에 **그 이전 프레임의 잔영(殘影)을 남기면 시각적 사실성을 높일 수 있는데**, 이를 **모션 블러(Motion Blur)**라 한다. 또한, 일반 사진과 같이 **초점이 맞춰지는 영역 바깥 부분을 흐릿하게 처리**하는 것을 **초점 심도(Depth of Field)**라 한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/introduction/post-processing.png)   

***

## 👻 Graphics API
위에서 보았던 단계 중, 그래픽 디자이너가 오프라인에서 수행하는 모델링, 리깅, 애니메이션 단계의 경우 Autodesk의 **3ds Max** 혹은 **Maya**와 같은 소프트웨어가 널리 쓰인다. 반면, 런타임 애니메이션, 렌더링, 후처리는 **응용 프로그램**에 의해 실행된다.

게임 같은 경우는 대체로 **게임 엔진(Game Engine)**같은 툴을 많이 사용한다. 대표적으로 유니티(Unity)와 언리얼(Unreal)이 존재한다. 게임 엔진은 애니메이션, 렌더링, 후처리에 필요한 필수불가결한 요소들을 망라하는 개발 툴인데, 근래의 게임 엔진은 여기에 더불어 **물리 기반 시뮬레이션, 사운드, 인공지능 등의 기능도 제공**한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/introduction/graphics-api.png)   

게임 프로그램 아래에 게임 엔진이 있고, 게임 엔진 아래에 **그래픽스 인터페이스(Graphics API)**가 있다. 일반적으로 게임 엔진은 그래픽스 API에 의해 개발되며 가장 많이 쓰이는 두 가지 API는 **Direct3D**와 **OpenGL**이다.

> - **Direct3D** : DirectX API의 구성 요소 중 하나로, 마이크로소프트 플랫폼에서만 쓸 수 있다.
- **OpenGL** : 국제 표준 단체인 크로노스 그룹(Khronos Group)에 의해 관리되며, 다양한 플랫폼에서 사용되는 표준 API이다.

모바일 기기를 위한 API로 **OpenGL 기능의 일부로 정의**된 **OpenGL ES(Embeded System)**도 존재한다.

그래픽스 API의 아래엔 **GPU(Graphics Processing Unit)**가 존재한다. 이러한 API는 그래픽스 응용에 필수적인 함수들을 제공하는데, 오늘날 이러한 함수들은 대부분 GPU 내에서 하드웨어로 구현이 되어있다.

OpenGL을 구동하면 GPU가 작업을 수행하게 된다. 즉, 그래픽스 API는 GPU에 대한 **소프트웨어 인터페이스**라 할 수 있다.

***

## 👻 글을 마치며
이번 시간에는 그래픽스 강의를 시작하고 그래픽스에 관해 간단하게 알아보았다. 이번에 그래픽스 스터디를 새로 시작하게 되었고, 그 스터디의 대표 교재&강의의 내용이었는데 아무래도 DirectX를 조금이라도 공부를 해놓아서 그런지 이해를 쉽게 할 수 있었던 것 같다. 언리얼의 기능들과 관련지어 공부할 수 있도록 노력해야 할 것 같다. ~~근데 다음 내용이 당장에 수학 기초인데 잘 할 수 있을지 모르겠다. 😂😂~~

***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (1장-서론)](https://youtu.be/EdkPDiwsTF0)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   