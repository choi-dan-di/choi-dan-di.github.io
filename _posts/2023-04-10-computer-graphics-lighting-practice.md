---
title: "[Computer Graphics] #9-1. 라이팅 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 9장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, lighting, practice]

permalink: /computer-graphics/lighting-practice/

toc: true
toc_sticky: true

date: 2023-04-10 18:18:09+0900
last_modified_at: 2023-04-10 18:18:15+0900
published: true
---

## 👻 연습문제 풀이
9장 라이팅 챕터의 연습문제는 총 5개이다.

***

### 🌱 1
- **문제**

> 아래의 퐁 모델은 하나의 방향성 광원을 가정하여 정의되었다.
>   > max(n·l, 0)s<sub>d</sub> ⊗ m<sub>d</sub> + (max(r·v, 0))<sup>sh</sup>s<sub>s</sub> ⊗ m<sub>s</sub> + s<sub>a</sub> ⊗ m<sub>a</sub> + m<sub>e</sub>   
>
> (a) 여러 개의 방향성 광원을 다룰 수 있도록 위 식을 수정하라.   
(b) 방향성 광원을 점 광원으로 대체해 보자. 물체 표면의 한 점에 입력되는 빛의 세기는 그 점과 광원 사이의 거리 제곱에 반비례한다. 이에 맞춰 위 식을 수정하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/lighting-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 예제 코드 9-1의 16번 줄에서 v_view는 단위 벡터이다. 그런데, 예제 코드 9-2의 19번 줄에서 v_view를 정규화하였다. 그 이유를 설명하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/lighting-practice/2-solve.jpg)   

***

### 🌱 3
- **문제**

> 예제 코드 9-2의 24번 줄은 왜 max를 호출하는가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/lighting-practice/3-solve.jpg)   

***

### 🌱 4
- **문제**

> 스페큘러 반사 구현을 위해서는, 노멀 n과 빛 벡터 l을 이용해 반사 벡터 r을 계산해야 한다. n과 l의 내적을 이용해 r을 정의하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/lighting-practice/4-solve.jpg)   

***

### 🌱 5
- **문제**

> 다음 그림은 퐁 모델의 스페큘러 항 (max(r·v, 0))<sup>sh</sup>s<sub>s</sub> ⊗ m<sub>s</sub>에 관한 것이다. 가운데 열의 원뿔은 카메라가 하이라이트를 볼 수 있는 영역을 만한다. 첫째 열과 두 번째 열을 잇고, 두 번째 열과 세 번째 열을 이어라. 예를 들어, sh가 작을수록 원뿔이 작아진다면, 왼쪽 위 상자와 가운데 위 상자를 이어라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/lighting-practice/5.jpg)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/lighting-practice/5-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 라이팅 챕터의 연습문제를 풀어보았다. 퐁 라이팅의 개념만 완벽히 이해하면 큰 어려움없이 풀 수 있는 문제들이었다. 다만, 1번의 경우 수학적인 개념을 알지 못하면 시간이 좀 걸릴 것 같다. ~~내가 그랬다..😂~~ 다음 번엔 어려움 없이 술술 풀었으면 좋겠다.. 수학책을 다시 사야하나? 😂😂😂

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   