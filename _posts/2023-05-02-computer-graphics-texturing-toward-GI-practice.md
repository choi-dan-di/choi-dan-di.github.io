---
title: "[Computer Graphics] #16-1. 전역 조명과 텍스처링 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 16장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, texturing, gi, global illumination]

permalink: /computer-graphics/texture-toward-GI-practice/

toc: true
toc_sticky: true

date: 2023-05-02 23:46:40+0900
last_modified_at: 2023-05-02 23:46:44+0900
published: true
---

## 👻 연습문제 풀이
16장 전역 조명과 텍스처링 챕터의 연습문제는 총 6개이다.

***

### 🌱 1
- **문제**

> 퐁 모델의 스페큘러 항에서 반사 벡터는 ``` 2n(n·l) - l ```로 정의된다(여기서 ``` n ```은 노멀, ``` l ```은 빛 벡터이다). 반면, 광선 추적법에서 반사 광선의 방향은 ``` I - 2n(n·I) ```로 정의된다(여기서 ``` I ```는 1차 광선의 방향이다). 왜 이런 차이가 발생하는가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/texturing-toward-GI-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 반지름이 2이고 원점을 중심으로 가지는 구에 대해 광선 추적법을 적용해 보자.   
>
> (a) 1차 광선의 시작점은 (10, 1, 0)이고 방향 벡터는 (-1, 0, 0)이다. 이 광선을 t의 매개변수 방정식으로 표현하라.   
(b) 구의 함수는 x<sup>2</sup> + y<sup>2</sup> + z<sup>2</sup> - 2<sup>2</sup> = 0이다. 이 함수와 광선의 매개변수 방정식을 이용하여 구와 광선의 교차점을 계산하라.   
(c) 반사 광선을 계산하기 위해서는 교차점에서의 노멀이 필요하다. 본 문제에서는 노멀을 쉽게 계산할 수 있다. 어떻게 계산할 것인가?   
(d) 반사 광선의 방향은 ``` I - 2n(n·I) ```로 정의된다(여기서 ``` I ```는 1차 광선의 방향이다). ``` I - 2n(n·I) ```를 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/texturing-toward-GI-practice/2-solve.jpg)   

***

### 🌱 3
- **문제**

> 라이트 매핑은 대개 이미지 텍스처링과 결합되어 디퓨즈 반사 색상을 결정한다.   
>
> (a) 그런데 디퓨즈 반사 색상 자체를 라이트맵에 저장하면 런타임 성능을 보다 향상 시킬 수 있을 것이다. 하지만, 실제 이렇게 사용되지 않는다. 어떤 문제점 때문일까?   
(b) 라이트 매핑은 종종 **다크 매핑**(dark mapping)이라고 풍자되는데, 라이트 매핑을 수행한 후 이미지 텍스처 색상이 더 어두워졌기 때문이다. 왜 이런 문제가 발생하는가? 이 문제를 어떻게 해결할 수 있을까?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/texturing-toward-GI-practice/3-solve.jpg)   

***

### 🌱 4
- **문제**

> 큐브맵에 사용할 여섯 개의 이미지를 렌더링을 통해 얻고자 한다. 각 이미지는 뷰 변환과 투영 변환을 이용해 생성된다.   
>
> (a) 여섯 개의 이미지를 생성하는 데 서로 다른 뷰 변환이 사용되는가? 맞는지 틀리는지 답하고 그 이유를 설명하라.   
(b) 여섯 개의 이미지를 생성하는 데 서로 다른 투영 변환이 사용되는가? 맞는지 틀리는지 답하고 그 이유를 설명하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/texturing-toward-GI-practice/4-solve.jpg)   

***

### 🌱 5
- **문제**

> 아래 그림은 큐브맵의 여섯 개 이미지를 펼쳐 놓은 것이다. 반사 벡터 R을 계산한 결과 그 방향은 (0.5, 0.4, -0.2)였다.   
>
> (a) 반사 벡터는 어느 면과 교차하는가?   
(b) 해당 면에서 교차점의 좌표를 계산하라.   
(c) 이 교차점을 이용해 해당 면에 저장된 텍스처에 접근해야 한다. 교차점으로부터 텍스처 좌표를 계산하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/texturing-toward-GI-practice/5.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/texturing-toward-GI-practice/5-solve.jpg)   

***

### 🌱 6
- **문제**

> 다음 그림은 큐브 매핑을 위한 텍스처 좌표 (s, t)를 계산하는 과정이다(이는 GPU 내부에서 수행된다). 반사 벡터 R을 계산한 결과 이는 -x 면과 교차하는 것으로 판정되었고, 교차점의 좌표는 (-1.0, 0.8, 0.4)로 계산되었다. 그림에서 A와 B의 좌표를 계산하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/texturing-toward-GI-practice/6.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/texturing-toward-GI-practice/6-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 전역 조명과 텍스처링에 관련된 문제를 풀어보았다. 라이팅을 공부할 때 하나만 알면 되는 줄 알았는데 그게 아닌 여러 물체가 상호작용하면서 적용되는 조명이 있다는 것을 알게 되었고 조금은 복잡하지만 어떻게보면 또 간단한 방법으로 구현한다는 것이 신기했던 것 같다. 아직까지 텍스처링에 대한 개념이 완벽히 잡히지 않은 것 같은데 실습을 통해서 코드상으로 어떻게 진행되는지 확실히 학습할 필요가 있는 것 같다.

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   