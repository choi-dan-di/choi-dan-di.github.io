---
title: "[Computer Graphics] #12-1. 스크린 물체 조작 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 12장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, screen space, object manipulation, practice]

permalink: /computer-graphics/screen-space-object-manipulation-practice/

toc: true
toc_sticky: true

date: 2023-04-19 21:13:50+0900
last_modified_at: 2023-04-19 21:13:53+0900
published: true
---

## 👻 연습문제 풀이
12장 스크린 물체 조작 챕터의 연습문제는 총 4개이다.

***

### 🌱 1
- **문제**

> 그림 12.3에서 스크린 공간 광선의 시작점은 (x<sub>s</sub>, y<sub>s</sub>, 0)이고, 카메라 공간 광선의 시작점은 (x<sub>c</sub>, y<sub>c</sub>, -n)이다.   
>
> (a) 뷰포트 변환의 역(inverse)을 이용하여 NDC로 표현된 클립 공간에서의 광선 시작점을 계산하라.   
(b) 위 (a)항의 결과는 식 (12.1)과 같아야 한다는 사실을 이용하여 x<sub>c</sub>와 y<sub>c</sub>를 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 스크린 상의 물체 선택을 위해 뷰포트 한 가운데를 마우스로 클릭했다고 하자.   
>
> (a) 뷰 프러스텀 파라미터는 다음과 같다: n = 12, f = 18, fovy = 120˚, aspect = 1. 카메라 공간 광선을 t의 매개변수 방정식으로 표현하라. [힌트: 뷰포트의 한 가운데를 마우스로 클릭했으므로, 스크린 공간과 카메라 공간 간 변환 행렬을 계산하는 대신, 직관적으로 카메라 공간 광선을 얻을 수 있다.]   
(b) 카메라 공간에 중심이 (0, -1, -14)에 있고 반지름이 2인 바운딩 구가 있다. 광선이 바운딩 구와 부딪히는 점의 t값 및 3차원 좌표를 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation-practice/2-solve.jpg)   

***

### 🌱 3
- **문제**

> 스크린 공간에 다음과 같은 삼각형이 있다. 정점별 속성은 {(R, G, B), z}이다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation-practice/12-3.PNG)   
(a) 세 정점 v<sub>1</sub>, v<sub>2</sub>, v<sub>3</sub>를 기준으로, (5.5, 3.5)에 놓인 픽셀의 무게중심 좌표를 계산하라.   
(b) 이 무게중심 좌표를 이용해 (5.5, 3.5)의 R과 z를 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation-practice/3-solve.jpg)   

***

### 🌱 4
- **문제**

> 그림 12.15의 마지막 단계는 월드 변환의 역(inverse)이다. 현재 월드 변환이 회전(R<sub>0</sub>)에 이은 이동(T)의 결합이라 하자. 한편, 손가락의 터치스크린 접촉점을 추적해서 얻은 좌표를 {p<sub>1</sub>, p<sub>2</sub>, ..., p<sub>n</sub>}이라 하자.   
>
> (a) 손가락이 p<sub>1</sub>에서 p<sub>2</sub>로 이동했을 때 얻은 회전 변환을 R<sub>1</sub>이라고 하자. R<sub>1</sub> 계산에 이용된 월드 변환의 역은 무엇인가? T와 R<sub>0</sub>를 이용해 표현하라.   
(b) 손가락이 p<sub>2</sub>에서 p<sub>3</sub>로 이동했을 때 얻은 회전 변환을 R<sub>2</sub>라고 하자. R<sub>2</sub> 계산에 이용된 월드 변환의 역은 무엇인가? T와 R<sub>0</sub>와 R<sub>1</sub>을 이용해 표현하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation-practice/4-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 스크린 물체 조작과 관련된 문제를 풀어보았다. 책도 보고 강의도 들으니 생각보다 이해를 빨리 할 수 있었던 것 같다. 그래도 역시나 문제 풀기 직전엔 머릿속이 새하얘지는 게... 응용하는 능력을 많이 키워야 할 것 같다. 😋

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   