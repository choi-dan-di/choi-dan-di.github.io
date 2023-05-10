---
title: "[Computer Graphics] #18-1. GPU 테셀레이션 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 18장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, gpu tessellation, tessellation, practice]

permalink: /computer-graphics/surface-tessellation-practice/

toc: true
toc_sticky: true

date: 2023-05-09 21:27:42+0900
last_modified_at: 2023-05-09 21:27:46+0900
published: true
---

## 👻 연습문제 풀이
18장 GPU 테셀레이션 챕터의 연습문제는 총 2개이다.

***

### 🌱 1
- **문제**

> 아래 그림은 테셀레이션이 완료된 정사각형 정의역이다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/surface-tessellation-practice/1.jpg)   
(a) 각 정점은 고유한 (u, v) 좌표를 부여받는다. 빈칸을 (u, v) 좌표로 채워라.   
(b) 위와 같은 테셀레이션에 필요한 테셀레이션 레벨은 무엇인가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/surface-tessellation-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 아래 그림은 테셀레이션이 완료된 정삼각형 정의역이다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/surface-tessellation-practice/2.jpg)   
(a) 각 정점은 고유한 무게중심 좌표를 부여받는다. 빈칸을 무게중심 좌표로 채워라.   
(b) 위와 같은 테셀레이션에 필요한 테셀레이션 레벨은 무엇인가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/surface-tessellation-practice/2-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 테셀레이션과 관련된 연습문제를 풀어보았다. 정의역 내부를 분할하는 것은 어렵지 않게 이해했는데 경계면을 분할하는 것은 약간 이해하기 힘들었다. 그리고 삼각형이 그래픽스에선 중요한 도형인데 개념을 100퍼센트 이해한 건 아닌 것 같아서 개인적으로 공부해서 정리를 해야 할 필요가 있는 것 같다. 그래픽스 입문 진짜 끝!

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   