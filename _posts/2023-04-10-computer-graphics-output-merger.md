---
title: "[Computer Graphics] #10. 출력 병합기"
excerpt: "한정현 컴퓨터 그래픽스 강의 10장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, output merger]

permalink: /computer-graphics/output-merger/

toc: true
toc_sticky: true

date: 2023-04-10 18:34:57+0900
last_modified_at: 2023-04-10 18:35:01+0900
published: true
---

## 👻 출력 병합기
**출력 병합기(Output Merger)**는 GPU 렌더링 파이프라인의 마지막 단계로, 색상값까지 할당받은 각각의 프래그먼트가 스크린에 어떻게 보여지는지 결정하는 작업을 한다. 래스터라이저와 마찬가지로, 출력 병합기도 하드웨어로 고정되어 있는데 GL 함수를 사용하면 그 작동 방식을 일부 조절할 수 있다. 출력 병합기가 수행하는 두 가지 주된 작업은 **Z-버퍼링**과 **알파 블렌딩**이 있다.

***

## 👻 Z-버퍼링
뷰포트는 실제 스크린에 보여질 영역이다. 이 스크린에 나타날 픽셀들의 정보를 잠시 보관하는 공간을 **버퍼(Buffer)**라고 하는데, GL에서는 세 가지의 버퍼를 가지고 있다.

> 1. 컬러 버퍼(Color Buffer)
2. 깊이 버퍼(Depth Buffer)
3. 스텐실 버퍼(Stencil Buffer)

이 세 가지 버퍼를 합쳐서 **프레임 버퍼(Frame Buffer)**라 부른다. 컬러 버퍼는 이름 그대로 픽셀의 색상값을 모두 저장한다. 깊이 버퍼는 **Z-버퍼**라고도 불리는데, 컬러 버퍼와 동일한 해상도를 가지며 현재 컬러 버퍼에 저장되어 있는 픽셀의 z값을 저장한다. 여기서 스텐실 버퍼는 따로 다루지 않는다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger/z-buffering1.png)   

다음과 같이 뷰포트에 겹쳐진 두 개의 삼각형이 있다고 가정해보자. 초기화 된 상태에서 한 삼각형을 렌더링한 후 다른 삼각형을 렌더링하게 되는데, 이 때 z값을 비교해 백그라운드보다 앞에 있으면 안전하게 그려지며 두 물체 중 z값이 더 적은 삼각형이 렌더링되면 해당 좌표의 z값은 그 삼각형의 z값으로 바뀌며 덮어씌워지게된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger/z-buffering2.png)   

위 과정은 빨간 삼각형이 먼저 렌더링되고, 후에 파란 삼각형이 렌더링되는 과정을 보여준다. 초기화를 하게되면 백그라운드의 z값은 maxZ값이 된다. z값이 작을수록 더 앞에 있다는 의미이며 첫 삼각형은 모두 그려지게 될 것이다.

그 다음, 더 앞쪽에 위치한 파란 삼각형이 렌더링되면 빨간 삼각형과 겹치는 부분은 픽셀의 z값과 프래그먼트(파란 삼각형)의 z값을 비교하여 더 작은 값을 가지는 프래그먼트로 대체될 것이다. 만약, 후에 그려진 프래그먼트의 z값이 백그라운드보다 더 크다면 해당 프래그먼트는 버려진다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger/z-buffering3.png)   

반대로, 파란 삼각형을 먼저 그린다고 가정해보자. 초기화 후 첫 삼각형을 그리는 과정은 똑같을 것이며 두 번째로 그려지는 빨간 삼각형에 대해 겹치는 부분에서 다시금 비교가 일어나게 될 것이다. 하지만 결과값은 이전과 똑같이 나오게 된다.

이처럼 물체의 렌더링 순서를 바꾸어도 동일한 비교 과정을 거쳐 같은 결과값을 갖게 된다. 즉, Z-버퍼링에서 렌더링 오더는 중요한 게 아니라는 것을 알 수 있다.

***

### 🌱 Z-버퍼링 in GL
다음은 GL에서 Z-버퍼링을 수행하는 코드이다.

```ini
glClearDepthf(1.0f);  // initialized with depth 1.0
glClearColor(1.0f, 1.0f, 1.0f, 1.0f); // initialized with white
glClear(GL_DEPTH_BUFFER_BIT | GL_COLOR_BUFFER_BIT); // clear buffers
```

앞서 뷰포트 변환 때 ``` glDepthRange ```로 정의된 스크린 공간이 있을 때, Z-버퍼는 **배경의 z값에 해당하는 maxZ값으로 초기화**된다. 컬러 버퍼는 배경색으로 초기화되며 마지막으로 ``` glClear ```는 이를 이용해 Z-버퍼와 컬러 버퍼를 초기화한다. 이후엔 출력 병합기가 자동으로 Z-버퍼의 z값과 프래그먼트의 스크린 공간 좌표 중 z좌표를 비교하여 색상을 설정하게 된다.

이처럼 Z-버퍼를 이용하여 픽셀과 프래그먼트의 앞뒤 순서를 가리는 알고리즘을 **Z-버퍼링(Z-Buffering)** 혹은 **깊이 버퍼링(Depth Buffering)**이라 부른다.

Z-버퍼링을 활성화하기 위해서는 뒷면 처리와 동일하게 ``` glEnable(GL_DEPTH_TEST) ``` 함수를 호출해야 한다. 그 다음, ``` glDepthFunc ```를 사용하여 z값 비교에 사용할 방법을 지정해야 한다. 특별한 경우가 아니라면 프래그먼트의 z좌표가 픽셀의 z값보다 **작을 때** Z-버퍼와 컬러 버퍼가 갱신되므로 기본 파라미터값은 ``` GL_LESS ```이다.

***

## 👻 알파 블렌딩
우리는 이제까지 불투명한 물체를 전제로 개념을 공부해왔다. 이번엔 RGB에 A가 추가되어 물체의 투명도가 적용되었을 때, 어떻게 처리하는지 알아보도록 하자.

프래그먼트의 RGBA 색상 중 A를 **알파 채널(Alpha Channel)**이라고도 하는데, 투명도를 수치화한 것이다. 이 값을 이용하여 프래그먼트의 색상과 픽셀의 색상을 **혼합(Blending)**하는 과정을 **알파 블렌딩(Alpha Blending)**이라고 한다.

A는 대개 RGB 각각의 채널과 같은 크기의 비트 수를 가지지만, 정수 범위보다는 **정규화**된 범위인 [0, 1]이 선호된다. 참고로 A가 0이면 완전 투명, 1이면 완전 불투명을 의미한다. 전형적인 알파 블렌딩 식은 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger/alpha1.png)   

여기서 c는 블렌딩된 색상을 나타내고, α(알파)는 **프래그먼트의 불투명도(알파 채널)**, c<sub>f</sub>는 프래그먼트의 색상, c<sub>p</sub>는 픽셀의 색상을 나타낸다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/output-merger/alpha2.png)   

아까와 동일하지만 투명도가 첨가된 두 개의 삼각형을 렌더링한다고 가정해보자. 먼저 빨간 삼각형을 렌더링한 후 파란 삼각형을 렌더링하게 되면, 겹치는 부분은 Z-버퍼링 때처럼 교체되는 게 아닌, 색상이 혼합될 것이다.

반대로 파란 삼각형을 먼저 렌더링하고 빨간 삼각형을 렌더링한다고 가정해보자. 빨간 삼각형이 그려질 때 겹쳐지는 부분은 Z-버퍼에 저장된 z값보다 더 크기 때문에 해당 프래그먼트는 혼합되지 않고 버려질 것이다. 단순히 렌더링 순서만 바꿨을 뿐인데 결과값도 달라지게 되는 것이다.

이처럼 알파 블렌딩은 Z-버퍼링과 반대로 렌더링 오더가 변경되었을 때 결과값이 달라지는 문제가 발생한다. 따라서 해당 연산은 **모든 불투명한 삼각형이 처리된 뒤, 반투명한 삼각형들은 z값이 큰 물체 순으로 처리**되어야한다. 즉, 반투명한 삼각형들은 **정렬(Sorting)**되어있어야 한다. 하지만 삼각형들을 정렬하는 것은 그리 간단한 문제가 아니며 매 프레임마다 카메라 시선이 바뀐다면 새로운 시선에 대해 실시간으로 정렬이 이루어져야 한다. 뷰 프러스텀 안에 반투명한 삼각형이 많을수록 계산 부하는 커지게 된다.

***

### 🌱 알파 블렌딩 in GL
다음은 알파 블렌딩을 위한 GL 프로그램 코드이다.

```ini
glEnable(GL_BLEND);
glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);
glBlendEquation(GL_FUNC_ADD);
```

우선 ``` glEnable(GL_BLEND) ```로 알파 블렌딩 기능을 활성화시켜야한다. 그런 다음 프래그먼트와 픽셀이 혼합되는 방법을 지정한다. ``` glBlendFunc ``` 함수의 파라미터를 보면 이해가 쉽다. 여기서 ``` SRC ```는 프래그먼트를 의미하며 ``` GL_SRC_ALPHA ```는 앞서 보았던 프래그먼트의 알파값(α)이다. ``` GL_ONE_MINUS_SRC_ALPHA ```는 (1-α)값이 된다.

마지막으로 ``` glBlendEquation ```은 옵션값이며 그 두 수를 어떻게 연산할지에 대해 정의한다. 통상적으론 두 값을 서로 더하는 ``` GL_FUNC_ADD ```(Default)를 사용한다. 이 외에도 ``` GL_FUNC_SUBTRACT, GL_FUNC_REVERSE_SUBTRACT, GL_MIN, GL_MAX ```가 있다.

***

## 👻 글을 마치며
이번 시간에는 출력 병합 단계에 대해 알아보았다. 드디어 GPU 렌더링 파이프라인의 전반적인 흐름을 마무리지었다. 처음 그래픽스 공부를 시작했을 땐, 내용이 너무 어려워서 힘들고 막막했었는데 그래도 스터디 덕분에 서로 힘내서 여기까지 잘 올 수 있었던 것 같다. 이제 시작이라 생각하고 한 템포 쉰 다음 더 열심히 해야겠다! ~~그래픽스 재미있네!!~~

***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (10장-출력 병합)](https://youtu.be/qegJ7a6UwXg)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   