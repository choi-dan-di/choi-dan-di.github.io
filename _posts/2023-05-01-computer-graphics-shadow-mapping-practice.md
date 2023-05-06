---
title: "[Computer Graphics] #15-1. 쉐도우 매핑 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 15장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, shadow mapping, shadow, mapping, map, practice]

permalink: /computer-graphics/shadow-mapping-practice/

toc: true
toc_sticky: true

date: 2023-05-01 20:12:58+0900
last_modified_at: 2023-05-01 20:13:02+0900
published: true
---

## 👻 연습문제 풀이
15장 쉐도우 매핑 챕터의 연습문제는 총 5개이다.

***

### 🌱 1
- **문제**

> 쉐도우맵 필터링.   
>
> (a) 왼쪽 그림에서, 첫 번째 패스에서 광원 기준으로 샘플한 점과, 두 번째 패스에서 카메라 기준으로 샘플한 점을 구분하자. 근접점 샘플링을 통해 쉐도우맵을 필터링하되 바이어스는 사용하지 않는다는 가정 하에, f<sub>1</sub>부터 f<sub>5</sub>까지 5개의 프래그먼트 각각에 대해 빛을 받는지 혹은 그림자가 지는지 판단하라.   
(b) 오른쪽 그림에서 q는 쉐도우맵에 투영된 프래그먼트인데 그 깊이 값은 0.5이다. 또한, q를 둘러싼 네 개의 텍셀에 대해서도 깊이 값이 표시되었다. 이 경우 PCF가 리턴하는 값은 얼마인가?   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping-practice/1.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 아래는 쉐도우 매핑을 위한 첫 번째 패스의 정점 쉐이더인데, ``` lightViewMat ```와 ``` lightProjMat ```는 각각 '광원 기준의' 뷰 변환과 투영 변환을 의미한다. 빈 칸을 채워라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping-practice/2.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping-practice/2-solve.jpg)   

***

### 🌱 3
- **문제**

> 쉐도우 매핑을 부적절하게 수행했을 때 부딪힐 수 있는 문제 중 하나는 이른바 **피터팬 현상**(Peter Panning)이라는 것인데, 렌더링 결과 생성된 영상에서 그림자를 만드는 물체가 그림자와 붙어 있지 않고 분리되어 나타나는 것을 말한다. 즉, 물체가 피터팬처럼 날아다니는 느낌을 준다. 왜 이런 문제가 발생하는가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping-practice/3-solve.jpg)   

***

### 🌱 4
- **문제**

> 아래는 쉐도우 매핑을 위한 두 번째 패스의 정점 쉐이더인데, 이는 클립 공간 좌표 (x, y, z, w)에서 (0.5x + 0.5w, 0.5y + 0.5w, 0.5z + 0.5w)를 얻은 후 이를 ``` v_shadowCoord ```에 저장한다. 빈칸을 채워라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping-practice/4.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping-practice/4-solve.jpg)   

***

### 🌱 5
- **문제**

> 아래는 바이어스 기반 쉐도우 매핑을 위한 두 번째 패스의 프래그먼트 쉐이더이다. 빈칸을 채워라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping-practice/5.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping-practice/5-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 쉐도우 매핑과 관련된 문제를 풀어보았다. 분명히 모두 다 아는 개념인데 또 쉐도우 매핑은 이대로 어려웠던 것 같다. 그래도 평소에 궁금했던 부분(그림자가 어떻게 생기는지)에 대한 궁금증이 풀려서 어려워도 재미있게 공부할 수 있었던 것 같다.

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   