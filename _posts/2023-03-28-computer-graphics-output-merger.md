---
title: "[Computer Graphics] #10. 출력 병합"
excerpt: "한정현 컴퓨터 그래픽스 강의 10장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, output merger]

permalink: /computer-graphics/output-merger/

toc: true
toc_sticky: true

date: 2023-03-21 19:27:38+0900
last_modified_at: 2023-03-21 19:27:40+0900
published: false
---

## 👻 출력 병합
프래그먼트 쉐이더에서 텍스처링과 라이팅을 수행하고나면 각각의 프래그먼트는 색상값까지 할당받은 상태가 된다. 이러한 프래그먼트는 곧바로 스크린에 보여지는 것이 아니라 출력 병합 단계를 거쳐야한다. 각각의 프래그먼트가 스크린에 어떻게 보여지는지 결정을 하는 단계이다.

Color and Depth Buffers
뷰포트는 실제 스크린에 보여ㅛ질 영역. 잠시 보관하고 있는 버퍼가 opengl에서는 3가지가 있다.
- color buffer
- depth buffer
- stencil buffer
이 세 가지 버퍼를 합해서 frame buffer라고 한다.
컬러 버퍼는 이름 그대로 색상값을 보관한다
뷰포트가 wxh일때, 컬러 버퍼도 wxh 해상도를 가지고 있다.
뎁스 버퍼는 해당 프래그먼트의 z값을 가지고 있다. 마찬가지로 wxh 해상도를 가진다

Z-buffering
스캔 전환에서 보간될 때 z값도 보간된 상태로 저장되어있다. 노멀로 라이팅을 수행하고 텍스처 좌표로 텍스처링을 수행한다. 이 두 가지가 rgb 값을 형성한다. z값까지 포함하여 출력 병합 단계로 넘어가게된다.
출력된 z좌표를 이용해 z버퍼링이 수행된다
뷰포트에 겹처진 두 개의 삼각형이 있다고 가정해보자. 초기화 된 상태에서 한 삼각형을 렌더링한 후 다른 삼각형을 렌더링하게 된다. z값을 비교해 백그라운드보다 앞에 있으면 안전하게 그려지며 두 물체 중 z값이 더 적은 애가 렌더링되면 해당 좌표의 z값은 그 삼ㄱ각형의 z값으로 바뀌며 덮어씌워지게된다.
순서를 바꾸게되어도 동일한 과정 비교를 거쳐 같은 결과값을 갖게 된다. 즉 렌더링 오더는 중요한 게 아니다

Z-buffering in GL
glClearDepthf(1.0f); // 뎁스 초기화
glClearColor(1.0f, 1.0f, 1.0f, 1.0f); // 색상 초기화
glClear(GL_DEPTH_BUFFER_BIT | GL_COLOR_BUFFER_BIT); // 버퍼 초기ㅗ하
뒷면 처리와 동일하게 glEnable 함수를 이용해 해당 연산을 활성화시켜줘야한다.
glEnable(GL_DEPTH_TEST);

Alpha Blending
불투명도를 계산하는 식을 이용해 색상을 정하는 것
c = acf + (1 - a)cp
cf:프래그먼트 색상
cp:픽셀 색상
픽셀과 프래그먼트 색상을 블렌딩한다
알파는 프래그먼트의 불투명도이다. z버퍼링과 반대로 렌더링 오더가 변경되었을 때 결과값이 달라지는 문제가 발생ㅊ한다
그래서 해당 연산은 z값이 큰 물체 순으로 계산되어야한다(뒤에 위치하는 물체 우선) back-to-front order
반투명은 sorted한 후 연산 필요

Alpha Blending in GL
glEnable(GL_BLEND);
glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);
glBlendEquation; // not necessary
프래그먼트가 소스이다
통상적으론 GL_FUNC_ADD를 사용한다. DEFAULT
이 외에도 GL_FUNC_SUBTRACT, GL_FUNC_REVERSE_SUBTRACT, GL_MIN, GL_MAX가 있다.


***

## 👻 글을 마치며


***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (10장-출력 병합)](https://youtu.be/qegJ7a6UwXg)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   