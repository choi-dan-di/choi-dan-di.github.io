---
title: "[Computer Graphics] #13-1. 캐릭터 애니메이션 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 13장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, character animation, character, animation, practice]

permalink: /computer-graphics/character-animation-practice/

toc: true
toc_sticky: true

date: 2023-04-24 18:27:32+0900
last_modified_at: 2023-04-24 18:27:35+0900
published: true
---

## 👻 연습문제 풀이
13장 캐릭터 애니메이션 챕터의 연습문제는 총 7개이다.

***

### 🌱 1
- **문제**

> 아래 그림은 드레스 포즈 골격의 계층 구조 및 이와 관련된 변환을 보여준다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/1.PNG)   
(a) M<sub>2,p</sub>가 무엇인지 설명하라.   
(b) M<sub>2,d</sub>가 무엇인지 설명하라.   
(c) M<sub>6,d</sub>의 빈칸을 채워라.   
(d) 아래 그림은 위 그림의 역변환을 보여준다. 빈칸을 채워라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/1-1.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 아래에서 왼쪽 그림은 팔의 드레스 포즈를 보여준다. 문제를 간단하게 만들기 위해서, 위팔의 뼈 공간을 캐릭터 공간이라 하자. 아래팔과 손의 뼈 공간 원점은 캐릭터 공간 기준으로 각각 (12, 0)과 (22, 0)이다. 드레스 포즈의 아래팔이 90˚, 손이 -90˚ 회전하여 오른쪽에 보인 애니메이션 포즈를 만들었다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/2.PNG)   
(a) 드레스 포즈에서 아래팔과 손의 부모 변환 행렬 M<sub>f,p</sub>와 M<sub>h,p</sub>를 계산하라.   
(b) M<sub>f,p</sub>와 M<sub>h,p</sub>를 이용하여, 아래팔과 손에 속한 정점을 캐릭터 공간으로 변환하는 행렬 M<sub>f,d</sub>와 M<sub>h,d</sub>를 계산하라.   
(c) M<sub>f,d</sub><sup>-1</sup>과 M<sub>h,d</sub><sup>-1</sup>을 계산하라.   
(d) 아래팔과 손의 애니메이션을 묘사하는 지역 변환 행렬 M<sub>f,l</sub>과 M<sub>h,l</sub>을 계산하라.   
(e) 아래팔에 속한 정점을 애니메이션하고 이를 다시 캐릭터 공간으로 변환하는 행렬 M<sub>f,a</sub>를 계산하라.   
(f) 손에 속한 정점을 애니메이션하고 이를 다시 캐릭터 공간으로 변환하는 행렬 M<sub>h,a</sub>를 계산하라.   
(g) 아래팔의 뼈 공간에서 (8, 0)의 좌표를 가지는 정점을 v라고 표기하자. 이는 아래팔과 손에 의해서만 영향 받는다. 두 뼈의 블렌딩 가중치는 동일하다. 스키닝 알고리즘을 사용하여 애니메이션 포즈에서 v의 캐릭터 공간 좌표를 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/2-solve.jpg)   

***

### 🌱 3
- **문제**

> 왼쪽에 보인 드레스 포즈에서 아래팔이 -90˚, 손이 90˚ 회전하여 오른쪽과 같은 애니메이션 포즈를 만들었다. 위 2번 문항과 같은 (a)에서 (g)까지 답하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/3.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/3-solve.jpg)   

***

### 🌱 4
- **문제**

> 왼쪽에 보인 드레스 포즈에서 아래팔이 -90˚, 손이 90˚ 회전하여 오른쪽과 같은 애니메이션 포즈를 만들었다. 정점 v는 아래팔과 손에 의해 영향 받는다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/4.PNG)   
(a) 아래팔의 뼈 공간에서 v의 좌표는 무엇인가?   
(b) 아래팔에 의해 회전된 v가 위팔의 뼈 공간에서 어떤 좌표를 가지는지 계산하는 과정을 두 개의 행렬을 사용해서 설명하라.   
(c) 손의 뼈 공간에서 v의 좌표는 무엇인가?   
(d) 손에 의해 회전된 v가 위팔의 뼈 공간에서 어떤 좌표를 가지는지 계산하는 과정을 네 개의 행렬을 사용해서 설명하라.   
(e) v에 대한 아래팔과 손의 블렌딩 가중치는 80%와 20%이다. 위팔의 뼈 공간에서 v의 좌표를 계산하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/4-solve.jpg)   

***

### 🌱 5
- **문제**

> 아래는 키프레임 애니메이션과 스키닝을 결합하는 슈도코드이다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/5.PNG)   
(a) 6번 줄에서 어떤 키 데이터들이 보간되는가?   
(b) 10번 줄에서 호출한 정점 쉐이더는 스키닝 말고도 라이팅과 텍스처링을 수행한다. 이러한 정점 쉐이더를 위한 정점 배열의 구성 요소를 나열하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/5-solve.jpg)   

***

### 🌱 6
- **문제**

> 아래 그림은 스키닝 알고리즘이 어떻게 구현되는지 보여준다.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/6.PNG)   
(a) 위 캐릭터는 총 몇 개의 뼈를 가지고 있는가?   
(b) 한 정점에 영향을 주는 뼈의 개수는 몇 개인가?   
(c) 오른쪽 위 빈칸을 채워라.   
(d) 행렬 팔레트의 각 원소 M<sub>i</sub>는 두 개 행렬의 결합인데, 하나는 애니메이션 전과정에 걸쳐 고정 불변이고, 다른 하나는 매 프레임 갱신된다. 이들 행렬의 역할이 무엇인지 기술하라.

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/6-solve.jpg)   

***

### 🌱 7
- **문제**

> 그림 13.16에서 날아가는 공을 캐릭터가 응시하도록 만들기 위해 머리뼈와 목뼈를 잇는 3자유도 관절을 사용하도록 하자. 머리뼈를 회전시켜 공을 응시하도록 하는 해석적 기법의 알고리즘을 기술하라.   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/7.PNG)   

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation-practice/7-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 캐릭터 애니메이션에 대한 문제를 풀어보았다. 개인적으로 흥미로웠던 챕터였다. 언리얼 엔진을 조금씩 만져보면서 스켈레탈 메시를 자주 만졌었는데 어떻게 움직이는지 항상 궁금했었기 때문이다. 그래도 스터디 덕분에 궁금했던 부분을 많이 해소할 수 있었던 아주 좋은 시간이었다!

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   