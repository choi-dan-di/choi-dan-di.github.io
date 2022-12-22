---
title: "[DirectX 12] DirectX, 그래픽스, 렌더링 파이프라인 개요"
excerpt: "DirectX, 그래픽스, 렌더링 파이프라인의 개요 알아보기"

categories:
  - DirectX
tags:
  - [DirectX, dx, dx12, graphics, rendering pipeline, pipeline, summary]

permalink: /directx/directx-summary/

toc: true
toc_sticky: true

date: 2022-12-23 00:33:57+0900
last_modified_at: 2022-12-23 00:34:00+0900
---

## 👻 DirectX
**DirectX**란 **Windows 프로그램에 고성능 하드웨어 가속 멀티미디어 지원을 제공하는 낮은 수준의 응용 프로그래밍 인터페이스(API) 집합**이다. 쉽게 말해 게임을 만들 때 화면에 표시되는 모든 그래픽을 관리하기 위해 사용되는 API라고 볼 수 있다. 우리가 게임을 이용할 때 보여지는 UI의 전반적인 기능을 만들 수 있다. 예를 들어, 캐릭터를 움직이거나 어떠한 모션을 취하거나 등의 빠르고 사실적인 3D 표현을 위해서 사용한다.

풀네임은 **Microsoft DirectX**로, 마이크로소프트에서 개발하였으며 현재 마이크로소프트 윈도우, 세가, 드림캐스트, 마이크로소프트 엑스박스 및 엑스박스 360을 위한 비디오 게임 개발에 널리 쓰이고 있다.

> 💡 **API**   
**API**란 **Application Programming Interface**의 약자로써 **컴퓨터나 컴퓨터 프로그램 사이의 연결**을 의미한다. API의 맥락에서 애플리케이션이라는 단어는 고유한 기능을 가진 모든 소프트웨어를 나타내고, 인터페이스는 두 애플리케이션 간의 서비스 계약이라고 할 수 있다.
>
> 그러한 의미에서 DirectX는 사용자 컴퓨터의 하드웨어와 프로그램을 연결시켜주는 매개체라고 볼 수 있다.

DirectX에는 Direct2D, Direct3D, DirectMusic 등이 존재하며 각 요소를 알맞게 사용하면 2차원부터 3차원까지 현실적으로 나타낼 수 있게된다.

***

## 👻 컴퓨터 그래픽스
**컴퓨터 그래픽스(CG: Computer Graphics)**란 **컴퓨터를 이용해 실제 세계의 영상을 조작하거나 새로운 영상을 만들어내는 기술**이다. DirectX을 이용해 컴퓨터 그래픽스 작업을 하면 사실적인 묘사가 가능하게 되는 것이다. 컴퓨터 그래픽스에는 여러 분야가 존재한다.

- **계산기하학**   
디지털 공간에서 물체를 표현하고 이에 관련된 다양한 연산을 다룬다. 모양을 변형 시키거나, 여러 물체가 참여하는 대한 연산, 질의 등의 계산이 계산기하학에 포함된다.

- **컴퓨터 애니메이션**   
움직이는 영상을 컴퓨터로 제작하는 기법을 다룬다. 3차원 공간에서 움직이는 물체나 사람을 표현하는 기법을 연구하므로 이 분야의 연구결과는 컴퓨터 게임이나 영화 제작에서 많이 응용된다. 실제 배우의 3차원 움직임을 포착하여 컴퓨터로 저장하는 기법인 모션 캡처 또한 이 분야의 연구 대상이다.   
물이나 공기 등의 유체의 움직임을 시뮬레이션하는 것 또한 크게 분류하면 이 범주에 포함된다.

- **렌더링**   
렌더링은 그래픽 데이터를 실제 이미지로 나타내는 과정을 연구한다. **사실적 묘사(Photorealistic)**와 **비사실적 묘사(Non-Photorealistic)**로 나눠지며 최근 3차원 장면 생성 프로그램(예: 3ds Max, Maya, Softimage, Rhino 등)은 사실적 묘사 기법 중 하나인 Radiosity 기법뿐만 아니라 정확한 정반사에 의한 조도 표현을 위한 **레이-캐스팅(Ray-Casting) 기법**, 부드러운 그림자를 표현하기 위한 **그림자 맵핑 기법** 등을 함께 사용할 수 있도록 만들어져 있다.   

    - **실시간 렌더링**   
    실시간 렌더링은 3차원 이미지를 빠른 시간에 렌더링하여 사용자의 입력에 즉시 반응하는 애니메이션을 만들어내는 기술이다. 컴퓨터 게임 등 인터랙티브 응용 프로그램에 주로 사용된다.

***

## 👻 렌더링 파이프라인
위의 컴퓨터 그래픽스 중 3차원 컴퓨터 그래픽스에서 **렌더링 파이프라인(Rendering Pipeline)**이란 **3차원 이미지를 2차원 래스터 이미지로 표현을 하기위한 단계적인 방법**을 말한다. 여기서 **래스터(raster)**란 컴퓨터에서 화상 정보를 표현하는 한 가지 방법으로 이미지를 2차원 배열 형태의 픽셀로 구성하고 이 점들의 모습을 조합, 일정한 간격의 픽셀들로 하나의 화상 정보를 표현하는 것을 말하는데 즉, 한 줄에서 연속된 픽셀들의 집합을 래스터라고 한다.

쉽게 말해, GPU가 프레임을 렌더링하면서 데이터가 어떻게 들어오고 나가는지를 나타내는 순차적 흐름이라 볼 수 있다. 렌더링 파이프라인의 흐름도는 아래 이미지의 우측으로 묶인 부분이다.

![Alt Text](/assets/images/posts_img/basics/directx/directx-summary/rendering-pipeline.png)   

- **Input Assembler Stage** : 정점 세팅   
- **Vertex Shader Stage** : 정점 변환   
- **Tessellation Stages(Hull Shader Stage, Tessellator Stage,**   
**Domain Shader Stage)** : 정점 추가 (거시적)   
- **Geometry Shader Stage** : 정점 추가 (미시적)   

------------------------ 정점 단위 계산 ------------------------

- **Rasterizer Stage** : 정점 대상으로 보간하는 작업   
- **Pixel Shader Stage** : 색상 설정   
- **Output Merger Stage** : 색상 조합   

~~단계 별 자세한 설명은 DirectX의 공부를 진행하면서 이해가 될 때마다 따로 정리할 계획이다.~~

***

## 👻 요약
> **DirectX**를 통해 3차원 **컴퓨터 그래픽스**를 그려내는 과정을 **렌더링 파이프라인**이라고 한다.

***

## 👻 글을 마치며
이번 시간에는 다렉 공부 입문 후 단어의 대략적인 뜻을 공부해보았다. 원래라면 다렉11으로 입문해야 하는데 강의 내용이 다렉12라서 고민이 좀 많이 됐었다. 그래도 어차피 해야하는 거라면 미리 하는 게 좋으니 시작은 했다만.. 여전히 어려운 것 같다. 😭 다렉 강의를 다 보고나면 책을 구매해 개념을 계속 학습하면서 실습을 해 볼 예정이다. ~~정 모르겠으면 11로 다시 공부해보는 것도..~~

***

_출처_   
_[DirectX란?_SONY](https://www.sony.co.kr/electronics/support/articles/S500081155)_   
_[DirectX 위키백과](https://ko.wikipedia.org/wiki/DirectX)_   
_[컴퓨터 그래픽스 위키백과](https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B7%B8%EB%9E%98%ED%94%BD%EC%8A%A4)_   
_[그래픽스 파이프라인 위키백과](https://ko.wikipedia.org/wiki/%EA%B7%B8%EB%9E%98%ED%94%BD%EC%8A%A4_%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8)_   
_[Direct3D 12 파이프라인 및 셰이더](https://learn.microsoft.com/ko-kr/windows/win32/direct3d12/pipelines-and-shaders-with-directx-12)_