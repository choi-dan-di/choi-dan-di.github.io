---
title: "[Computer Graphics] #7-1. 래스터라이저 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 7장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, rasterizer, practice]

permalink: /computer-graphics/rasterizer-practice/

toc: true
toc_sticky: true

date: 2023-04-04 23:13:27+0900
last_modified_at: 2023-04-04 23:13:30+0900
published: true
---

## 👻 연습문제 풀이
7장 래스터라이저 챕터의 연습문제는 총 8개이다.

***

### 🌱 1
- **문제**

> 아래 그림의 삼각형 정점 세 개는 왼손 클립 공간에서 NDC로 표현되었다.   
<img src="/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/7-1.png" width="50%">   
(a) 뒷면 제거를 위해서는 우리는 각 정점의 xy좌표만 사용하는데, v<sub>1</sub>과 v<sub>2</sub>의 x좌표가 동일하고, v<sub>1</sub>과 v<sub>3</sub>의 y좌표가 동일하다고 가정하자. 이 삼각형이 2차원 xy평면으로 투영되었을 때, 정점은 시계 방향으로 정렬되는가, 아니면 반시계 방향으로 정렬되는가? 2차원 삼각형 그림을 그려 답하라.
>
> (b) 위 (a)항에서 시계 및 반시계 방향 여부는 행렬식의 부호를 통해 판별할 수 있다. 이 행렬식은 양수인가 음수인가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 뷰포트의 모퉁이 점이 (10, 20, 1)과 (100, 200, 2)로 주어졌다. 뷰포트 변환은 축소확대에 이은 이동으로 구성된다.
>
> (a) 축소확대 행렬을 계산하라.   
(b) 이동 행렬을 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/2-solve.jpg)   

***

### 🌱 3
- **문제**

> GL 프로그램이 glViewport(10, 20, 200, 100)과 glDepthRange(0, 1)을 호출하였다. 뷰포트 변환은 축소확대에 이은 이동으로 구성된다.
>
> (a) 축소확대 행렬을 계산하라.   
(b) 이동 행렬을 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/3-solve.jpg)   

***

### 🌱 4
- **문제**

> 다음 그림과 같은 3차원 뷰포트가 주어졌다. 뷰포트 변환 행렬을 계산하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/7-4.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/4-solve.jpg)   

***

### 🌱 5
- **문제**

> 스크린 공간에 다음과 같은 삼각형이 있다. 정점별 애트리뷰트는 {(R, G, B), z}이다. 픽셀 좌표 (5.5, 3.5)에서 R과 z를 계산하라. 단, 7.5절에서 기술한 바와 같은 '기울기에 기반한 알고리즘'을 기계적으로 적용하는 대신, y좌표가 3.5인 스캔 라인과 왼쪽 변 간 교차점에서의 R과 z를 '한 번의 선형 보간'을 통해 계산한 후, 해당 스캔 라인에서 역시 '한 번의 선형 보간'을 통해 (5.5, 3.5)에서의 R과 z를 계산하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/7-5.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/5-solve.jpg)   

***

### 🌱 6
- **문제**

> 스크린 공간에 다음과 같은 삼각형이 있다. 정점별 애트리뷰트는 RGB 색상, 2차원 텍스처 좌표, z좌표로 구성된다. 픽셀 좌표 (4.5, 3.5)에서 R과 z를 계산하라. 단, 7.5절에서 기술한 바와 같은 '기울기에 기반한 알고리즘'을 기계적으로 적용하는 대신, y좌표가 3.5인 스캔 라인과 오른쪽 변 간 교차점에서 '한 번의 선형 보간'을 통해 애트리뷰트를 계산한 후, 해당 스캔 라인에서 역시 '한 번의 선형 보간'을 통해 (4.5, 3.5)에서의 애트리뷰트를 계산하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/7-6.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/6-solve.jpg)   

***

### 🌱 7
- **문제**

> 아래는 투영 변환 행렬인데, n = 1, f = 2, fovy = π/2이며, 뷰 프러스텀의 너비와 높이는 같다. 카메라 공간의 점 (0, 2, -2)와 (0, 0, -1)에 투영 변환 및 원근 나눗셈을 적용한 후 그 중간점을 계산하라. 한편, (0, 2, -2)와 (0, 0, -1)의 중간점인 (0, 1, -1.5)에 투영 변환 및 원근 나눗셈을 적용하라. 두 결과는 같은가 다른가? 그 이유를 설명하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/7-7.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/7-solve.jpg)   

***

### 🌱 8
- **문제**

> 아래 그림은 투영 변환의 예이다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/7-8-11.PNG)   
(a) 카메라 공간의 점 (0, 1, -1.5)에 투영 변환 및 원근 나눗셈을 적용하라.   
(b) 위 (a)항의 결과를 보면, 아래 그림과 같은 투영 변환의 결과를 상상할 수 있을 것이다. 즉, 뷰 프러스텀 안에 균등하게 분포되어 있던 평면들이 투영 변화 결과 비균등하게 분포된다. 이유를 설명하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/7-8-12.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer-practice/8-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 7장 래스터라이저 챕터의 연습문제를 풀어보았다. 문제가 그렇게 많진 않은데 아무래도 계산식이 많다보니 시간이 꽤나 소요된 것 같다. 그래도 어려운 부분은 없었고, 스캔 전환에서 겹선형 보간식이 헷갈렸었는데 복습하고 연습문제도 계속 풀다보니 완벽히 이해한 것 같다.

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   