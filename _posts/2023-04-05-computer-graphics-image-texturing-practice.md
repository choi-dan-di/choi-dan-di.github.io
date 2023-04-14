---
title: "[Computer Graphics] #8-1. 이미지 텍스처링 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 8장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, image texturing, texturing, practice]

permalink: /computer-graphics/image-texturing-practice/

toc: true
toc_sticky: true

date: 2023-04-05 22:37:30+0900
last_modified_at: 2023-04-05 22:37:33+0900
published: true
---

## 👻 연습문제 풀이
8장 이미지 텍스처링 챕터의 연습문제는 총 9개이다.

***

### 🌱 1
- **문제**

> 아래 그림은 스크린 공간 삼각형과 교차하는 스캔 라인을 보여주는데, 스캔 라인의 y좌표는 3.5이다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/8-1.PNG)   
(a) 삼각형 두 변이 스캔 라인과 교차하는 점에서의 텍스처 좌표를 계산하라.   
(b) 스캔 라인 상에 놓인 두 프래그먼트의 텍스처 좌표를 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 텍스처 좌표 s가 [0, 1] 범위를 넘어섰고, 텍스처 포장을 위해 반복 모드(repeat mode)를 사용한다고 가정하자. ⌊ ⌋ 혹은 ⌈ ⌉ 함수를 사용하여 s를 [0, 1] 범위로 전환하는 방정식을 작성하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/2-solve.jpg)   

***

### 🌱 3
- **문제**

> 근접점 샘플링으로 필터링할 텍스처 공간에 한 픽셀이 (s', t')로 투영되었다. ⌊ ⌋ 혹은 ⌈ ⌉ 함수를 사용하여 텍셀 ID를 구하는 방정식을 작성하라. 한 텍셀의 ID는 해당 텍셀의 왼쪽 아래 모퉁이의 좌표이다. 예를 들어, 중심 좌표가 (1.5, 3.5)인 텍셀의 ID는 (1, 3)이다.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/3-solve.jpg)   

***

### 🌱 4
- **문제**

> 다음 그램은 네 개의 텍셀을 가진 텍스처 공간에 16개의 픽셀이 투영된 것을 보여준다. 각 텍셀의 색상을 c<sub>i</sub>라고 할 때, 겹선형 보간으로 결정되는 픽셀 색상 c를 정의하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/8-4.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/4-solve.jpg)   

***

### 🌱 5
- **문제**

> 한 픽셀이 8 × 8 해상도의 텍스처 공간에 투영되었다. 아래 왼쪽 그림의 동그라미는 픽셀, 직사각형은 그 픽셀의 발자국을 나타낸다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/8-5.PNG)   
(a) 텍스처 색상은 회색조이며, 오른쪽 그림은 밉맵의 0번과 1번 레벨 텍스처를 보여주는데, 숫자는 텍셀 밝기를 나타낸다. 이와 같은 방식으로 2번과 3번 레벨 텍스처를 그려라.
>
> (b) 삼선형 보간 기법으로 밉맵을 필터링하고, λ 계산을 위해 픽셀 발자국의 '긴' 변 길이를 사용한다고 가정하자. 어떤 레벨이 선택되는가? 또한, 각 레벨에서의 필터링 결과를 계산하라.
>
> (c) 삼선형 보간 기법으로 밉맵을 필터링하고, λ 계산을 위해 픽셀 발자국의 '짧은' 변 길이를 사용한다고 가정하자. 어떤 레벨이 선택되는가? 또한, 각 레벨에서의 필터링 결과를 계산하라.


- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/5-solve.jpg)   

***

### 🌱 6
- **문제**

> GL 함수 ``` glTexParameteri(GLenum target, GLenum pname, GLint param) ```에서 ``` target ```은 ``` GL_TEXTURE_2D ```로, ``` pname ```은 ``` GL_TEXTURE_MIN_FILTER ```로 설정되었다.
>
> (a) ``` param ```을 ``` GL_LINEAR ```로 설정할 경우, 어떤 문제가 발생하는가?   
(b) λ가 계산되었을 때, 밉맵에서 이로부터 가장 가까운 레벨을 선택하기 위하여 ``` param ```을 ``` GL_LINEAR_MIPMAP_NEAREST ```로 설정했다면, 총 몇 번의 선형 보간이 이뤄지는가? 여기서 묻는 것은 '겹선형 보간'의 횟수가 아닌 '선형 보간'의 횟수이다. 아래 문항도 마찬가지다.   
(c) ``` param ```을 ``` GL_NEAREST_MIPMAP_LINEAR ```로 설정했다면, 총 몇 번의 선형 보간이 이뤄지는가?   
(d) ``` param ```을 ``` GL_LINEAR_MIPMAP_LINEAR ```로 설정했다면, 총 몇 번의 선형 보간이 이뤄지는가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/6-solve.jpg)   

***

### 🌱 7
- **문제**

> 의료 영상 분야에서는 종종 3차원 텍스처링이 필요하다. 2<sup>l</sup> × 2<sup>l</sup> × 2<sup>l</sup> 해상도를 가지는 3차원 이미지를 생각해보자. 이를 이용해 어떻게 밉맵을 만들 것인가? 이 밉맵은 총 몇 개의 레벨을 가지는가? 밉맵 최상위 레벨 텍스처의 해상도는 무엇인가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/7-solve.jpg)   

***

### 🌱 8
- **문제**

> 아래는 그림 8.15의 밉맵과 사각형을 이용해 텍스처링을 수행한 결과를 보여준다. 사각형에는 a부터 e까지 5개의 픽셀이 표시되어 있다. 한편, 사각형 아래에는 2 × 2 텍셀 격자와 픽셀 발자국 쌍 5개를 보여준다. a부터 e까지의 픽셀은 각각 이 중 어느 경우에 해당하는가?   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/8-8.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/8-solve.jpg)   

***

### 🌱 9
- **문제**

> 다음 그림은 두 색상으로 구성된 텍스처로, 검은색 텍셀은 0, 밝은 회색 텍셀은 0.8 값을 가진다. 동그라미로 표시된 픽셀이 4개 텍셀의 한가운데로 투영되었는데, 이 픽셀의 발자국은 변의 길이가 2√2인 정사각형이다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/8-9.PNG)   
(a) 밉맵의 모든 레벨은 근접점 샘플링으로 필터링되고, 그 결과는 선형 보간된다. 근접점 샘플링 시, 픽셀로부터 같은 거리에 있는 텍셀이 여러 개 존재한다면 위의 혹은 오른쪽 위의 텍셀을 선택한다. 픽셀의 최종 텍스처링 색상은 무엇일까?
>
> (b) 밉맵의 모든 레벨이 겹선형 보간으로 필터링되고 그 결과가 선형 보간된다면, 픽셀의 최종 텍스처링 색상은 무엇일까?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing-practice/9-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 8장 이미지 텍스처링 챕터의 연습문제를 풀어보았다. 이 부분도 7장과 동일하게 계산식이 많아서 어렵지 않게 풀 수 있었던 것 같다. 아직까진 밉맵 필터링에 대해서 완벽히 이해하진 못한 것 같다. 그래도 스터디를 하면서 모르는 부분에 대해 알 수 있었고 아마도 계속 질문을 해야할 것 같다..😂

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   