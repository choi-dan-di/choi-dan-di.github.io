---
title: "[Computer Graphics] #10-1. 출력 병합기 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 10장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, output merger, practice]

permalink: /computer-graphics/output-merger-practice/

toc: true
toc_sticky: true

date: 2023-04-10 19:34:57+0900
last_modified_at: 2023-04-10 19:35:01+0900
published: true
---

## 👻 연습문제 풀이
10장 출력 병합기 챕터의 연습문제는 총 6개이다.

***

### 🌱 1
- **문제**

> 하나의 픽셀을 놓고 네 개의 삼각형이 경쟁하고 있다. 이들은 해당 픽셀 위치에서 서로 다른 z값을 가지고 있다. 삼각형들이 임의의 순서로 처리된다면, 이 픽셀에 대해 평균 몇 번의 z-버퍼 쓰기 연산이 수행되는가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 하나의 픽셀을 놓고 경쟁하는 세 개의 삼각형이 이 픽셀 위치에서 다음과 같은 RGBA 색상과 z값을 가지고 있다: {(1, 0, 0, 0.5), 0.25}, {(0, 1, 0, 0.5), 0.5}, {(0, 0, 1, 1), 0.75}. 삼각형은 뒤에서부터 앞으로 차례차례 처리된다. 픽셀의 최종 색상을 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger-practice/2-solve.jpg)   

***

### 🌱 3
- **문제**

> 뷰포트 안에 세 개의 삼각형이 있는데, 이들은 모두 z축에 수직이다. 빨간색 삼각형이 초록색 삼각형 뒤에 있고, 초록색 삼각형은 파란색 삼각형 뒤에 있다. 다음과 같은 RGBA 색상을 가진 프래그먼트 세 개가 하나의 픽셀을 놓고 경쟁한다: (1, 0, 0, 1), (0, 1, 0, 0.5), (0, 0, 1, 0.5). 이들은 뒤에서부터 앞으로 차례차례 처리된다. 픽셀의 최종 색상을 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger-practice/3-solve.jpg)   

***

### 🌱 4
- **문제**

> 하나의 픽셀을 놓고 경쟁하는 다섯 개의 프래그먼트가 다음과 같은 RGBA 색상과 z값을 가지고 있다:
>
>   > f<sub>1</sub> = {(1, 0, 0, 0.5), 0.2}   
>   > f<sub>2</sub> = {(0, 1, 1, 0.5), 0.4}   
>   > f<sub>3</sub> = {(0, 0, 1, 1), 0.6}   
>   > f<sub>4</sub> = {(1, 0, 1, 0.5), 0.8}   
>   > f<sub>5</sub> = {(0, 1, 0, 1), 1.0}
>
> (a) 이들 다섯 개 프래그먼트를 처리하는 올바른 순서는 무엇인가?   
(b) 픽셀의 최종 색상은 무엇인가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger-practice/4-solve.jpg)   

***

### 🌱 5
- **문제**

> 컴퓨터 그래픽스에서는 야외 장면의 현실감을 높이기 위해 종종 안개(fog)를 사용한다. 안개 중 가장 간단한 것은 이른바 선형 안개(linear fog)로, 뷰 프러스텀 내부에 안개가 고루 분포하게 만드는 것이다. 전방 평면(near plane)에서 후방 평면(far plane)으로 갈수록 안개가 짙어져서, 전방 평면에 위치한 물체는 완벽하게 보이고 후방 평면에 위치한 물체는 완전히 안개에 가려 보이지 않게 해야 한다. 이러한 선형 안개를 위해 다음과 같은 블렌딩 기법을 사용한다.
>
>   > c = fc<sub>f</sub> + (1 - f)c<sub>0</sub>
>
> 여기에서 c는 안개가 혼합된 최종 색상이고, f는 카메라로부터 거리가 멀어질수록 안개가 짙어지는 정도를 나타내며, c<sub>f</sub>는 안개의 색상이고, c<sub>0</sub>는 프래그먼트의 색상이다. 전방 평면까지의 수직 거리 N과 후방 평면까지의 수직 거리 F를 이용하여 f를 정의하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger-practice/5-solve.jpg)   

***

### 🌱 6
- **문제**

> 아래 그림과 같이 삼각형 세 개가 겹쳐져 있다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger-practice/6.PNG)   
(a) 삼각형 세 개는 모두 반투명한데, 빨간색, 초록색, 파란색 삼각형 순서로 처리된다. 렌더링 결과를 그려라.   
(b) 렌더링 결과에 어떤 문제가 있는가? 그리고 이를 해결하기 위한 방안은 무엇인가? 

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger-practice/6-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 출력 병합기 챕터의 연습문제를 풀어보았다. 개념적으로 이해하기 쉬웠던 챕터라 어렵지않게 풀 수 있었다. 6번이 특히 어려웠는데, 문제 자체를 이해하기 힘들었다. 그래도 스터디를 하면서 문제를 이해할 수 있게 되었고 결국엔 풀 수 있었던 것 같다. 스터디 처음 해보는 건데 도움이 많이 되는 것 같아서 뿌듯하다 ☺☺☺

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   