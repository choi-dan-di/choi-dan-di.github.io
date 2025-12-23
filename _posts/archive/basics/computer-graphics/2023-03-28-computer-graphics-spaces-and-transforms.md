---
title: "[Computer Graphics] #4. 좌표계와 변환"
excerpt: "한정현 컴퓨터 그래픽스 강의 4장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, space, transform, rotation, translate, scaling, affine]

permalink: /computer-graphics/spaces-and-transforms/

toc: true
toc_sticky: true

date: 2023-03-28 20:45:29+0900
last_modified_at: 2023-03-28 20:45:34+0900
---

## 👻 좌표계와 변환
게임과 같은 3차원 가상 공간은 많은 물체들로 구성되어 있는데, 각 물체의 위치는 **이동(Translation)**에 의해, 방향은 **회전(Rotation)**에 의해 결정된다. 또한 각 물체는 **축소확대(Scaling)**될 수도 있다. 이러한 이동, 회전, 축소확대를 총칭하여 **변환(Transform)**이라고 한다.

***

## 👻 2차원 변환의 행렬 표현
2차원에서의 물체의 변환에 대해 알아보자.

***

### 🌱 Scaling
**스케일링(Scaling)**은 축소, 확대를 의미하며 2차원 공간에서는 x, y 방향으로 두 가지 인자를 사용한다. 다음과 같은 ``` 2 × 2 ``` 행렬로 표현된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/2d-scaling-matrix.png)   

축소확대 인자를 주 대각선에 놓아서 행렬을 만들고, 이 인자가 **1보다 크면 확대, 1보다 작으면 축소**가 이루어진다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/2d-scaling-example.png)   

다각형을 축소확대하려면 그 다각형의 **모든 정점에 대해** 위와 같은 연산을 수행하면 된다. 2차원 벡터 ``` (x, y) ```는 **행렬-벡터 곱셈**을 통해 축소확대된다.

***

### 🌱 Rotation
![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/2d-rotation-graph.png)   

어떠한 벡터가 주어졌을 때 ``` θ ```만큼 회전하여 변환시킨 좌표를 구한다. 벡터의 길이는 변하지 않는다. 회전 변환도 똑같이 ``` 2 × 2 ``` 행렬을 만들어 행렬-벡터 곱셈을 이용하여 구할 수 있다.

벡터 ``` p ```의 길이가 ``` r ```이라면, ``` p ```의 좌표는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/2d-rotation-xy.png)   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/2d-rotation-xa.png)   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/2d-rotation-ya.png)   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/2d-rotation-matrix.png)   

기본적으로 **반시계 방향**의 회전을 표현하며, 시계 방향 회전은 ``` θ ``` 대신 ``` -θ ```를 대입한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/minus-90.png)   

``` -θ ```는 ``` 2π-θ ```와 동일하다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/equal-270.png)   

***

### 🌱 Translation and HC
벡터의 이동에 대해 알아보자. **변위 벡터(Displacement Vector)**를 기존의 벡터에 **더하여** 이동 할 좌표를 쉽게 구할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/2d-translation.png)   

하지만 위에서 축소확대와 회전은 행렬-벡터 곱셈을 이용하여 구했는데, 이러한 점을 통일시키기 위하여 **동차 좌표(Homogeneous Coordinate)**를 이용한다.

동차 좌표는 간단히 ``` (x, y, 1) ```로 표현할 수 있으며 앞의 ``` (x, y) ```좌표를 **카테시안 좌표(Cartesian Coordinate)**라고 한다. 마지막 원소 1은 아무 의미 없이 삽입된 것으로 생각해도 된다.

이동 행렬은 단위 행렬에 마지막 열에 동차 좌표를 삽입하면 ``` 3 × 3 ``` 행렬로 쉽게 만들 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/2d-translation-matrix.png)   

동차 좌표로 표현된 점에 이 행렬을 곱하면 이동 행렬을 곱셈으로 구현할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/2d-translation-matrix-multiple.png)   

> 💡 **Homogeneous Coordinates**   
동차 좌표의 마지막 원소(w)는 반드시 1일 필요는 없다. 0만 아니면 어떤 값이든 가능하다. 단, 1 이상의 수를 지정했다면 x, y에도 똑같이 w만큼 곱해줘야한다.
>
> 카테시안 좌표가 ``` (2, 3) ```일 때, 동차 좌표는 다음과 같이 지정할 수 있다.
>
> ```
> (2, 3, 1), (4, 6, 2), (6, 9, 3), ...
> ```
>
> 3차원 좌표계에서 이 점과 원점을 지나는 직선은 무한히 많은 동차 좌표들의 집합이 된다. 하나의 동차 좌표를 카테시안 좌표로 바꾸려면 이 직선을 따라 w = 1 평면으로 **투영(Project)**시키면 된다.
>
> ![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/homogeneous-coordinates.png)   
>
> 동차 좌표는 그래픽스에서 많이 사용하는 부분이기 때문에 확실히 이해하도록 하자!

모든 좌표는 앞으로 ``` x, y, 1 ```로 표현이 될 것이기 때문에 축소확대, 회전도 ``` 3 × 3 ``` 행렬로 맞춰준다. 마지막 열에 ``` (0, 0, 1) ```을 추가해주면 해당 사이즈로 행렬을 만들 수 있다. 기존의 열은 마지막 행에 0을 추가해주면 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/hc-add-matrix.PNG)   

***

### 🌱 Transform Composition
**변환 결합(Transform Composition)**은 하나의 다각형에 **여러 개의 변환을 연속적으로 정의**한 것이다. 한 물체는 여러 번의 변환을 거칠 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/composition-rt.png)   

다음과 같이 90도 회전 후 해당 좌표만큼 이동을 한다고 가정해보자. 식은 아래와 같이 합쳐질 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/composition-rt2.PNG)   

먼저 실행되는 변환의 행렬이 오른쪽에 위치해야한다. 그리고 행렬의 곱셈에서는 **교환 법칙이 성립하지 않는다.** 고로 ``` RT ```와 ``` TR ```의 결과는 일치하지 않으므로 주의해야 한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/composition-tr.png)   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/composition-tr2.PNG)   

> 💡 기존의 회전은 **원점을 기준**으로 한 것이다. 만약 원점이 아닌 **임의의 점을 기준으로 회전**을 한다면 변환 방법은 다음과 같다.   
> 1. 임의의 점을 원점으로 옮기는 T 변환을 실행한다.
2. R 변환을 기존대로 실행한다.
3. 기준이 되는 점을 다시 복구시키는 T 변환을 실행한다.   
>
> 이렇게 총 세 단계를 거치게 되면 동일한 결과를 얻을 수 있다.
>
> ![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/trt.png)   

***

## 👻 아핀 변환
앞서 보았던 축소확대, 회전 변환은 **선형 변환(Linear Transform)**에 속한다. 하지만 이동 변환은 그렇지 않다. **아핀 변환(Affine Transform)**은 이러한 선형 변환과 이동을 포함하는 모든 범주의 변환을 의미한다. 2차원 아핀 변환은 ``` 3 × 3 ``` 행렬로 표현되므로, 몇 개의 아핀 변환이 주어지건 이는 모두 **하나의 행렬로 결합이 가능**하다.

마지막 행 ``` (0 0 1) ```은 무시하고 나머지 ``` 2 × 3 ``` 행렬을 ``` [L|t] ```로 표기하자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/affine-lt.png)   

빨간색으로 표시된 ``` 2 × 2 ``` 행렬이 ``` L ```을, 파란색으로 표시된 3번 열이 ``` t ```를 의미한다.

> - ``` L ``` : Combined Linear Transform(이동을 제외한 선형 변환 연산의 결과)
- ``` t ``` : Combined Translation(이동을 포함한 아핀 변환 연산의 결과)

``` L ```에는 어떠한 이동 요소도 포함되지 않는다. 반면 ``` t ```는 입력으로 주어진 선형 변환을 포함할 수 있다는 것에 주의해야한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/tr-rt-srt.PNG)   

물체를 ``` [L|t] ```로 변환하는 것을 개념적으로 해석하면, 그 물체에 ``` L ```**을 먼저 적용하고 그렇게 선형 변환된 물체에** ``` t ``` **를 적용하는 것과 같다.** 즉, 폴리곤 메시로 표현된 물체의 각 정점을 ``` p ```라 할 때, ``` Lp+t ``` 방식으로 변환이 이뤄지는 것이다.

> 💡 **Rigid Motion**   
아핀 변환에서 축소확대가 제외된 오로지 **회전과 이동** 변환만이 존재할 때, 이를 **강체 변환(Rigid-Body Motion 혹은 간단히 Rigid Motion)**이라고 한다. 행렬은 ``` [R|t] ```로 표현이 되며 다른 아핀 변환과 동일하게 작용된다( ``` Rp+t ``` ). 아마 ``` [L|T] ```보다 많이 사용될 것이다.

***

## 👻3차원 변환의 행렬 표현
위에서 알아보았던 변환이 3차원 공간에서는 어떻게 적용되는지 개념을 확장해보자.

***

### 🌱 3D Scaling
3차원 축소확대는 다음과 같은 ``` 3 × 3 ``` 행렬로 표현되며 2차원과 동일하다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/3d-scaling.png)   

모든 축소확대 인자가 같으면 **균등(Uniform)**하다고, 그렇지 않다면 **비균등(Non-Uniform)**하다고 부른다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/3d-scaling-example.png)   

***

### 🌱 3D Rotation
2차원 회전은 항상 원점 중심이었다. 3차원 회전은 **축을 중심**으로 하는 회전을 의미한다. 가장 기본적인 회전축은 3차원 좌표계의 주축(x, y, z)을 말할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/3d-rotation.png)   

회전축이 되는 좌표값은 변환하지 않는다. 위 이미지처럼 ``` z ```축을 기준으로 회전을 시킨다면 ``` z ``` 좌표값은 바뀌지 않을 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/3d-rotation-z.png)   

> 💡 **CCW vs CW**   
2차원과 마찬가지로 반시계 방향이면 θ는 양수, 시계 방향이면 θ는 음수가 된다. 또한, 오른손 법칙을 똑같이 적용하면 네 손가락이 향하는 방향이 반시계 방향이 될 것이다.
>
> ![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/ccw-vs-cw.png)   

***

### 🌱 3D Translation
동차 좌표를 이용한 3차원 이동은 ``` 4 × 4 ``` 단위 행렬의 마지막 열에 동차 좌표를 삽입한 것으로 정의할 수 있다. 

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/3d-translation-matrix.png)   

더불어 축소확대, 회전 변환 행렬도 ``` 4 × 4 ``` 행렬로 확장시켜줄 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/3d-hc.png)   

***

## 👻 월드 변환
![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/world-transform.png)   

하나의 물체를 모델링하는 데 사용된 좌표계를 **오브젝트 공간(Object Space)**이라 부른다. 각자의 오브젝트 공간에서 정의된 물체들로 3차원 가상 환경을 구성하려면, 이들을 하나의 좌표계로 통합해야한다. 이 좌표계를 **월드 공간(World Space)**이라 부른다. 각각의 오브젝트는 나름의 **월드 변환(World Transform)**을 통해 월드 공간으로 옮겨진다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/3d-affine.png)   

2차원 공간 아핀 변환의 특성은 3차원에서도 그대로 유지된다. 다만 ``` L ```의 크기가 ``` 3 × 3 ```이 될 것이다.

> **Q. 어떨 때 Translate가 바뀌지 않고 그대로 유지될까?**

***

## 👻 회전과 오브젝트 공간 기저
모델링이 끝난 오브젝트는 **오브젝트 공간과 묶여있어서 같이 움직인다**고 볼 수 있다. 기준점이 고정불변이 되며 분리되지 않는다. 이러한 오브젝트만의 주축을 ``` {u, v, n} ```으로 정의할 수 있고 이를 **오브젝트 공간 기저(Object Space Basis)**라고 한다. 반대로 월드 공간에서의 기저는 **월드 공간 기저(World Space Basis)**로 정의한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/object-space-basis.png)   

월드 공간에 오브젝트를 처음 배치하게되면 월드 공간 기저와 오브젝트 공간 기저는 일치할 것이다. 여기서 오브젝트의 변환이 일어나게 되면 기저는 서로 불일치하게 되고 ``` {u, v, n} ``` 좌표값으로 기저 혹은 회전 행렬을 구할 수 있게 될 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/basis-make.PNG)   

각각의 좌표를 ``` u, v, n ```으로 표시하고 행렬을 합치면 다음과 같은 결과가 나오게 된다. 여기서 단위 행렬과의 곱셈은 자기 자신을 결과로 도출하므로 회전 행렬 ``` R ```은 다음과 같이 정의할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/object-space-basis2.png)   

회전축이 주축일 뿐만 아니라 임의의 축일 때도 동일하게 적용된다.

***

## 👻 역변환
컴퓨터 그래픽스에서 **역변환(Inverse Transform)**은 매우 자주 사용되는 중요한 개념이다. 역변환 연산을 수행하면 기존의 위치로 복구될 것이다. 

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/inverse-matrix.png)   

축소확대, 이동의 역변환은 해당 행렬의 역행렬로 쉽게 구할 수 있다. 이동의 역행렬은 반대 방향 즉, 원소에 음수를 붙이고 축소확대의 역행렬은 스케일만큼 나누는 역수를 사용하면 만들 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/inverse-rotation.png)   

회전의 역행렬은 **전치 행렬**을 사용한다. ``` {u, v, n} ```은 **orthonomal** 성질을 갖고 있다. 자기 자신과의 내적은 1, 다른 원소와의 내적은 0인 배타적인 성질을 갖고 있기 때문에 회전 행렬의 역행렬은 전치 행렬이된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/spaces-and-transforms/inverse-rotation2.png)   

***

## 👻 글을 마치며
이번 시간에는 좌표계를 이용하여 변환을 하는 법을 행렬을 통해 자세하게 알아보았다. 언리얼 엔진만 다뤘을 때는 기능 툴이 있기 때문에 자세히 알지 못했지만 그래픽스를 공부하면서 어떤 식으로 컴퓨터가 이해하고 표현하는지에 대해 알 수 있었다. 더불어 프로그래머 입장으로서 그래픽 디자이너와의 협업이 어떤 방식으로 진행되는지도 대충 흐름을 알아볼 수 있었던 것 같다.

***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (4장-좌표계와 변환)](https://youtu.be/GvQnCYxF1MU)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   