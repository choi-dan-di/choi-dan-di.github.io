---
title: "[Computer Graphics] #12. 스크린 물체 조작"
excerpt: "한정현 컴퓨터 그래픽스 강의 12장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, screen space, object manipulation]

permalink: /computer-graphics/screen-space-object-manipulation/

toc: true
toc_sticky: true

date: 2023-04-19 20:13:50+0900
last_modified_at: 2023-04-19 20:13:53+0900
published: true
---

## 👻 물체 선택
렌더링 된 물체를 선택하는 것을 **피킹(Picking)**이라고 한다. 피킹 시에 얻을 수 있는 정보는 스크린 상에서 손가락 혹은 마우스로 선택한 점의 2차원 좌표이다. 물체에 대한 정보가 없기 때문에, 이 2차원 좌표를 사용해서 해당 위치의 물체를 찾아내는 기법을 알아볼 것이다.

***

### 🌱 스크린 공간 광선
![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/ray1.png)   

위의 이미지에서, 마우스 커서의 위치 (x<sub>s</sub>, y<sub>s</sub>)가 주어졌을 때 **시작점**은 (x<sub>s</sub>, y<sub>s</sub>, 0)이고 **방향 벡터**는 (0, 0, 1)인 스크린 공간 **광선(Ray)**을 정의할 수 있다. 이 광선에 의해 처음 부딪히는 물체가 선택될 것이다. 여기서 광선-물체 교차 검사가 수행된다.

하지만, 스크린 공간에는 물체에 관한 정보가 없기 때문에 이 검사를 스크린 공간에서 수행할 수가 없다. 물체의 정보는 오브젝트 공간에 존재한다. 이 광선을 오브젝트 공간까지 변환하여 그 공간에서 광선-물체 교차 검사를 수행하면 물체의 정보를 알아낼 수 있을 것이다.

***

### 🌱 카메라 공간 광선
우선, 스크린 공간에 있는 광선을 카메라 공간까지 변환시켜야 한다. 스크린 공간에서 시작점과 방향 벡터를 정의하였다. 카메라 공간 광선도 마찬가지로 자신의 시작점과 방향 벡터로 정의된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/camera1.png)   

스크린 공간 광선을 카메라 공간으로 옮긴다고 생각해보자. 카메라 공간의 뷰 프러스텀의 **전방 평면**이 스크린에 해당 될 것이다. 따라서 카메라 공간의 x, y값을 각각 x<sub>c</sub>, y<sub>c</sub>라고 할 때, 시작점은 (x<sub>c</sub>, y<sub>c</sub>, -n)이 된다. 전방 평면의 z좌표가 -n이기 때문이다.

방향 벡터를 구하려면 투영선을 생각해야 한다. 뷰 프러스텀의 투영선을 뒤로 연장시키면 모든 투영선들이 원점으로 수렴된다. 이를 이용하면 방향 벡터는 원점과 시작점을 이은 벡터임을 알 수 있다. 투영 변환 행렬을 이용하면 카메라 공간 광선의 방향 벡터를 다음과 같이 구할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/camera2.PNG)   

***

### 🌱 오브젝트 공간 광선
앞에서 구한 카메라 공간 광선을 월드 공간으로, 그 다음에 오브젝트 공간으로의 변환을 하여 오브젝트 공간 광선의 시작점과 방향 벡터를 구할 수 있다. 우선 월드 공간으로의 변환을 위해서는 뷰 변환의 역(Inverse)이 필요하다. 뷰 변환은 이동(T)에 이은 회전(R), 즉 RT로 정의되므로 역변환은 T<sup>-1</sup>R<sup>-1</sup>이 된다. 따라서 뷰 변환의 역은 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/object1.png)   

위의 행렬을 이용하면 아래 이미지의 첫 번째 단계에 해당하는 카메라 공간에서 월드 공간으로의 변환이 완료된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/object2.png)   

각각의 모델은 각자 고유의 오브젝트 공간과 월드 변환을 가지고 있으므로, 모델마다 각기 다른 오브젝트 공간 광선을 가지게 될 것이다. 위 이미지의 두 번째 단계에 해당하는 변환이며 여기서 적용된 광선을 **매개변수 방정식(Parametric Equation)**으로 표현하면 다음과 같다.

> p(t) = s + td

여기에서 s는 시작점을, d는 방향 벡터를 의미하는데, 매개변수 t는 [0, ∞] 범위에 존재한다. 모델마다 s의 좌표와 d는 모두 다르며, 이러한 오브젝트 공간 광선을 사용해 어떤 모델을 선택할 지 선택하는 검사를 진행하게 된다.

***

### 🌱 광선과 바운딩 볼륨 간 교차 검사
원칙적으로는 물체(폴리곤 메시)의 모든 삼각형에 대해 **광선-삼각형 교차 검사**를 실시해야 한다. 광선과 부딪히는 삼각형이 하나 이상 존재할 때, 그 물체는 광선과 교차한다고 판정할 수 있다. 하지만 이러한 검사는 물체의 LOD가 높을 수록 많은 계산 시간을 필요로 한다. 이보다는 부정확하지만 **훨씬 빠른 방법**으로, 각 메시를 완벽히 감싸는 **바운딩 볼륨(Bounding Volume)**을 구한 뒤 이를 광선과의 교차 검사에 사용하게 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/volume1.png)   

위 이미지는 가장 많이 사용되는, 대표적인 바운딩 볼륨 두 가지를 보여준다. 하나는 좌표축과 나란한 **직육면체**로 **AABB(Axis-Aligned Bounding Box)**라 부르며, 다른 하나는 **바운딩 구(Bounding Sphere)**라고 부른다. 각각의 바운딩 볼륨은 세 축별 x, y, z의 최소, 최대 범위, 또는 중심 좌표와 반지름 길이로 정의된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/volume2.png)   

3차원 광선은 다음과 같은 3개의 매개변수 방정식으로 정의된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/volume3.PNG)   

한편, 바운딩 구를 사용한다 가정했을 때, 중심이 (C<sub>x</sub>, C<sub>y</sub>, C<sub>z</sub>)로, 반지름을 r로 표기할 때 아래와 같이 구를 정의할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/volume4.png)   

위 식의 x, y, z에 각각 x(t), y(t), z(t)를 대입하면 아래와 같은 형태의 2차 방정식을 얻게 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/volume5.png)   

여기에서 a, b, c값은 알고 있지만, t는 미지수이다. 근의 공식을 이용하여 미지수 t의 값을 구할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/volume6.png)   

이렇게 계산된 근이 바로 광선과 바운딩 구 간 교차점에서의 매개변수가 된다.

추가적으로, **판별식**이 양수라면 서로 다른 두 실근을 얻게 되는 뜻이므로, 광선과 바운딩 구가 총 두 번 만난다는 의미이며 판별식이 0이면 중근 즉, 광선과 바운딩 구가 접하고 판별식이 음수이면 마주치지 않는다는 의미이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/volume7.png)   

광선과 바운딩 구가 마주치는 부분이 2개 이상이라면 더 적은 t값을 선택한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/volume8.png)   

모든 오브젝트 별로 광선-물체 교차 검사를 실시하게 되고, 하나의 물체에 실시했던 것처럼 매개변수 t값 중 가장 작은 값을 최종적으로 선택하게 된다. t값이 작을수록 더 앞 쪽에 있다는 의미가 되기 때문이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/volume9.png)   

하지만, 광선과 바운딩 볼륨 간 교차 검사는 종종 **부정확한 결과를 산출**한다. 바운딩 볼륨과 교차하지만 실제 폴리곤 메시와 부딪히지 않을 가능성이 생기기 때문이다. 따라서 정확한 계산이 필요할 때에는 반드시 **폴리곤 메시의 모든 삼각형과 광선 간 교차 검사를 수행**해야 한다. 하지만 이렇게 광선-삼각형 교차 검사를 하더라도 **전처리 단계에서는 광선과 바운딩 볼륨 간 교차 검사를 수행**한다. 바운딩 볼륨에 광선이 부딪히지 않았다면 물체와 절대 부딪힐 일이 없다는 것을 보장할 수 있기 때문에 계산 시간을 효율적으로 줄일 수 있다.

***

### 🌱 광선과 삼각형 간 교차 검사
삼각형 한 개와 광선 간 교차 검사를 살펴보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/tri1.png)   

광선이 삼각형 내부의 p 지점을 통과했다고 가정할 때, p는 위와 같이 **세 정점의 가중치 합으로 정의**된다. 여기서 (u, v, w)를 삼각형 <a, b, c>에 대한 p의 **무게중심 좌표(Barycentric Coordinates)**라고 부르며 다음과 같은 의미가 될 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/tri2.png)   

u, v, w는 모두 [0, 1] 범위에 있으며 u + v + w = 1이 된다. 따라서 w는 1 - u - v로 대체될 수 있고, p를 다시 정의할 수 있다.

> p = <span style="color: red;">u</span>a + <span style="color: red;">v</span>b + (1 - <span style="color: red;">u</span> - <span style="color: red;">v</span>)c

한편, 광선은 매개변수 방정식 s + td로 표현한다고 했었다. 이 광선과 삼각형 <a, b, c> 간 교차점 계산은 다음과 같은 방정식을 푸는 것과 같다.

> s + <span style="color: red;">t</span>d = <span style="color: red;">u</span>a + <span style="color: red;">v</span>b + (1 - <span style="color: red;">u</span> - <span style="color: red;">v</span>)c

이를 다시 쓰면 다음과 같다.

> <span style="color: red;">t</span>d + <span style="color: red;">u</span>(c - a) + <span style="color: red;">v</span>(c - b) = c - s

여기에서 c - a, c - b, c - s를 각각 A, B, S료 표기하자.

> <span style="color: red;">t</span>d + <span style="color: red;">u</span>A + <span style="color: red;">v</span>B = S

그런데, d, A, B, S 모두 3차원 좌표이므로 식을 풀면 다음과 같은 선형 시스템으로 정리된다.

> <span style="color: red;">t</span>d<sub>x</sub> + <span style="color: red;">u</span>A<sub>x</sub> + <span style="color: red;">v</span>B<sub>x</sub> = S<sub>x</sub>   
<span style="color: red;">t</span>d<sub>y</sub> + <span style="color: red;">u</span>A<sub>y</sub> + <span style="color: red;">v</span>B<sub>y</sub> = S<sub>y</sub>   
<span style="color: red;">t</span>d<sub>z</sub> + <span style="color: red;">u</span>A<sub>z</sub> + <span style="color: red;">v</span>B<sub>z</sub> = S<sub>z</sub>

이는 **크레이머 법칙(Cramer's Rule)**으로 풀 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/tri3.png)   

이렇게 얻은 u와 v를 ``` p = ua + vb + (1 - u - v)c ``` 식에 대입하면 교차점을 얻을 수 있지만, 이는 삼각형이 광선과 교차하는 점이 아니라 **삼각형이 놓인 무한한 넓이의 평면**이 광선과 교차하는 점이다. 따라서, 교차점이 삼각형 안에 놓인다는 보장은 없다. 교차점이 삼각형 안에 있으려면 u, v, w가 모두 0 이상인 조건을 만족해야 한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/tri4.png)   

해당 조건을 만족하며 t의 값이 가장 작은 양수값을 가지는 교차점을 최종적으로 선택하게 된다.

***

## 👻 물체 회전
터치스크린을 생각해보면 손가락으로 화면을 터치하여 물체를 회전시킬 수 있을 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/rotation1.png)   

해상도 w × h의 터치스크린의 점 p에서 p<sub>1</sub>으로 손가락을 움직였다고 가정할 때, 물체의 회전을 구현하려면 우선 스크린을 2 × 2 크기의 정사각형으로 변환해야 한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/arcball1.png)   

기존의 좌측 하단에 위치한 원점은 2 × 2 정사각형 스크린의 정중앙으로 변환되며 총 크기가 2 × 2 × 2인 정육면체 공간이 스크린 뒤에 존재한다고 가정한다. 해당 정육면체 내엔 1의 반지름을 가지는 **아크볼(Arcball)**이라는 가상의 구가 존재한다. 터치스크린 위의 점 p와 정사각형 스크린 위의 점 q의 좌표를 각각 (x, y), (x', y')로 표기한다면, 두 좌표의 관계는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/arcball2.png)   

또한, 정사각형 스크린의 q를 -z축을 따라 아크볼 표면에 투영하고, 좌표계 원점과 이 투영점을 이은 벡터를 v라 할 때, 이 벡터의 z좌표는 다음과 같이 구할 수 있다.

v<sub>z</sub> = ![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/arcball3.png)

v는 단위 벡터이고, v<sub>x</sub> = q<sub>x</sub>, v<sub>y</sub> = q<sub>y</sub>일 때, 구를 정의하는 식에 대입을 하면 위의 식을 유도할 수 있다.

단, 이러한 방식으로 계산할 수 없는 경우, 1차적으로 q를 3차원 좌표계의 xy평면으로 투영한 후, 이 투영점을 정규화하면 아크볼 표면으로 2차 투영하는 결과를 낳는다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/arcball4.PNG)   

이와 같은 방식으로 p에서 q, q에서 v로 옮겨가며 v<sub>i</sub>와 v<sub>i+1</sub>을 계산할 수 있다. 이러한 회전은 두 좌표에 모두 수직인 **회전축**과 그 사이의 **회전각**에 의해 정의된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/screen-space-object-manipulation/arcball5.png)   

회전축은 원점으로부터 각 점들까지의 벡터를 구하여 그 두 벡터의 **벡터곱**을 계산하면 구할 수 있다. 반대로 회전각은 두 벡터의 **내적**을 이용하여 구할 수 있다.

이러한 회전축과 회전각을 이용해 회전된 물체는 GPU 파이프라인을 통해 **즉각 스크린에 렌더링**되어야 한다. 따라서, 실제 회전은 GPU 파이프라인의 첫 연산인 **월드 변환보다 앞서 적용**되어야 한다. 앞서 구한 회전축은 아크볼 공간에서 구한 축이기 때문에, 이를 오브젝트 공간으로 변환해야 한다. 회전각은 그냥 사용이 가능하다.

아크볼 공간의 회전축을 카메라 공간으로 그대로 옮긴 후 월드 공간, 월드 공간에서 오브젝트 공간으로 옮기면 오브젝트 공간에서의 회전축을 쉽게 구할 수 있다. 이제 오브젝트 공간으로 변환된 회전축을 이용해서 **회전 행렬**을 구해야 하는데, 이는 OpenGL의 ``` glm ```을 이용하면 된다.

> 💡 **아크볼 공간에서 정의된 회전축을 오브젝트 공간으로 옮긴 뒤, 오브젝트 공간에서의 물체 회전 행렬을 구하는 함수**
>
> ```ini
> glm::rotate(angle, axis)
> ```

만약 ``` glm ```을 사용하지 않는다면, 쿼터니언을 정의하고 쿼터니언을 이용한 회전 행렬식을 이용해 구하면 된다.

현재 월드 변환 행렬을 M이라고 하고, 오브젝트 공간으로 변환된 회전축을 이용해 계산한 회전 행렬을 R이라 할 때, MR이 정점 쉐이더에게 새로운 월드 변환 행렬로 주어져야한다. 이렇게 해야 오브젝트 공간에서 회전이 먼저 진행된 후 최종적으로 월드 변환이 수행될 것이다.

물체를 한 번 더 회전시킨다면 기존의 월드 변환의 역행렬을 이용하여 다시 오브젝트 공간으로 이동한 다음 회전 변환이 이뤄져야 할 것이다. 기존의 월드 변환인 MR을 M'라고 할 때, 이 행렬을 적용하여 오브젝트 공간으로 다시 이동한 뒤에 R<sub>i</sub>가 추가되며 이 모든 행렬을 합친 M'R<sub>i</sub>이 최종 월드 변환 행렬로 전달될 것이다. 물체가 이동할 때마다 위의 과정이 반복되며 물체의 회전이 수행된다.

***

## 👻 글을 마치며
이번 시간에는 스크린에서 물체를 조작하고 이는 어떤 식으로 적용되는지 알아보았다. 평소에 물체를 클릭하면 어떤 식으로 선택이 되는지 궁금했었는데 이번 챕터를 공부하면서 궁금증의 일부를 해소 할 수 있었던 것 같다. 다만, 공간의 변환이 많고 변환 행렬식이 복잡해서 정신이 약간 없었지만 개념 자체는 크게 어려운 것이 없었기 때문에 수업 진도를 잘 따라갈 수 있었다. 이제 터치스크린에서도 어떤 식으로 그래픽이 구현되는지 조금은 알게 된 것 같아서 기분이 좋다☺☺☺

***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (12장-스크린 물체 조작)](https://youtu.be/c7la2Tt_cOc)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   