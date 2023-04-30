---
title: "[Computer Graphics] #14-1. 노멀 매핑 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 14장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, normal mapping, normal, map, practice]

permalink: /computer-graphics/normal-mapping-practice/

toc: true
toc_sticky: true

date: 2023-04-25 20:45:56+0900
last_modified_at: 2023-04-25 20:46:00+0900
published: true
---

## 👻 연습문제 풀이
14장 노멀 매핑 챕터의 연습문제는 총 6개이다.

***

### 🌱 1
- **문제**

> 정점 배열 데이터.   
>
> (a) 캐릭터에 탄젠트 공간 노멀 매핑을 적용하려고 한다. 정점 배열에 저장해야 하는 데이터를 모두 나열하라.   
(b) 위의 캐릭터에 스키닝 애니메이션도 적용하려고 한다. 정점 배열에 저장해야 하는 데이터를 모두 나열하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 아래는 탄젠트 공간 노멀 매핑을 위한 정점 쉐이더이다. 빈칸을 채워라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping-practice/2.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping-practice/2-solve.jpg)   

***

### 🌱 3
- **문제**

> 정점 배열에 저장된 정점별 탄젠트 공간의 기저 {T, B, N}이 정점 쉐이더에게 제공되었다. 탄젠트 공간 노멀 벡터를 월드 공간으로 변환하는 행렬을 구하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping-practice/3-solve.jpg)   

***

### 🌱 4
- **문제**

> 그림 14.6-(a)와 달리, 한 점을 공유하는 삼각형들의 노멀을 사용해 그 점의 노멀을 계산하는 알고리즘을 기술하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping-practice/4.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping-practice/4-solve.jpg)   

***

### 🌱 5
- **문제**

> 아래는 탄젠트 공간 노멀 매핑을 위한 프래그먼트 쉐이더이다. 빈칸을 채워라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping-practice/5.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping-practice/5-solve.jpg)   

***

### 🌱 6
- **문제**

> 다음 그림은 원통의 파라미터화 결과를 보여준다. 원통의 축이 좌표계의 y축과 같다. 원통 표면에 노멀 매핑을 적용하려고 한다. 원통의 각 정점 (x, y, z)에 대해 어떻게 탄젠트 공간을 정의할 것인지 기술하라. 단, 한 정점의 탄젠트 공간 계산 시, 주위 정점 정보를 사용할 수 없다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping-practice/6.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping-practice/6-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 노멀 매핑에 관한 문제를 풀어보며 개념을 다시금 정리했다. 최적화 쪽도 잠깐 알아본 결과 그냥 뭐 조금만 커지면 다 매핑시키는 것 같은 생각이 든다. 그래도 굉장히 재미있었던 개념이라 어렵긴 했지만 힘들지 않게 완강할 수 있었던 것 같다. 그래도 공간 간의 변환이 살짝 애매하긴 한데... 이건 코드로 직접 구현해보면서 감을 익혀야 할 것 같다. 실습도 있으니 커트라인 지켜서 잘 만들어보자😋

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   