---
title: "[Computer Graphics] #7. 래스터라이저"
excerpt: "한정현 컴퓨터 그래픽스 강의 7장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, rasterizer]

permalink: /computer-graphics/rasterizer/

toc: true
toc_sticky: true

date: 2023-04-04 20:13:27+0900
last_modified_at: 2023-04-04 20:13:30+0900
published: true
---

## 👻 래스터라이저
GPU 파이프라인이 처리할 기하적인 개체들을 종종 **프리미티브(Primitive)**라고 부른다. 그래픽스 응용에 따라 삼각형 말고도 선(Line)과 점(Point)도 독자적인 프리미티브가 될 수 있는데, 이번 시간엔 삼각형 프리미티브에 대해 다루면서 래스터라이저에 대해 알아볼 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/rasterizer.png)   

GPU 렌더링 파이프라인의 **두 번째 단계**인 **래스터라이저(Rasterizer)** 단계에서는, 정점 쉐이더가 출력한 정점들을 프리미티브로 다시 **조립(Assemble)**된다. 이 프리미티브는 스크린에 그려질 형태로 변환된 후 **프래그먼트(Fragment)**로 분해되는데, 이를 **래스터화(Rasterization)**라고 부른다.

래스터라이저는 크게 5단계로 진행된다.

> 1. 클리핑(Clipping)
2. 원근 나눗셈(Perspective Division)
3. 뒷면 제거(Back-Face Culling)
4. 뷰포트 변환(Viewport Transform)
5. 스캔 전환(Scan Conversion)

***

## 👻 클리핑
**클리핑(Clipping)**은 ``` 2 × 2 × 2 ``` 크기의 **클립 공간 뷰 볼륨** 바깥에 놓인 폴리곤을 **잘라내는 작업**을 말한다. 직관적인 이해를 돕고자 뷰 프러스텀 단계에서의 클리핑이 어떻게 이루어지는지 알아볼 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/clipping.png)   

뷰 프러스텀 내부에 존재하는 삼각형은 **그대로 다음 단계로 넘겨진다.** 반대로 뷰 프러스텀 외부에 존재하는 삼각형은 **제거된다.** 문제는 삼각형의 일부만 뷰 프러스텀에 걸쳐져있는 것이다. 이렇게 반만 걸친 경우, 뷰 프러스텀 외부에 위치한 정점은 제거되고, 뷰 프러스텀 경계에 접한 **새로운 정점**을 만들어 새로운 프리미티브를 만들어내고, 해당 삼각형을 다음 단계로 전달하게 된다. 이러한 과정은 GPU가 알아서 처리해준다.

***

## 👻 원근 나눗셈
![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/m-proj.png)   

위 행렬은 카메라 공간에서 클립 공간으로 변환될 때 사용되는 **투영 행렬**이다. 이 투영 행렬을 카메라 공간의 한 점 ``` (x, y, z, 1) ```에 적용해보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/perspective-division.png)   

투영 행렬에서 0이 아닌 값들을 간단하게 표현하여 연산한 결과는 위와 같다. 투영 행렬의 네 번째 행을 보면 다른 변환 행렬과 다르게 ``` (0 0 -1 0) ```의 값을 가지는 것을 확인할 수 있다. 연산을 하게되면 마지막 행이 1이 아닌 다른 값(``` -z ```)이 나오게 된다. 이 값으로 모든 값을 나누면 **카테시안 좌표**를 구할 수 있다.

여기서 ``` -z ```가 어떤 의미를 가지는지 알아보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/m-proj2.png)   

위와 같은 값을 가지는 투영 행렬이 있다고 생각해보자. 이 행렬을 이용해 클립 공간으로 모든 좌표를 변환시키면 아래와 같이 모든 좌표가 변하게 될 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/proj-transform.png)   

여기서 **Far Plane**에 접해있는 ``` P₁ ```, ``` Q₁ ```과 **Near Plane**에 접해있는 ``` P₂ ```, ``` Q₂ ```를 투영 행렬을 이용해 클립 공간의 좌표로 변환하면 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/point-proj.png)   

여기서 마지막 좌표가 ``` -z ```인 것을 확인할 수 있고, 카테시안 좌표를 구하기 위해 각 좌표를 ``` -z ```로 나누면 최종적으로 클립 공간 좌표를 구할 수 있다. 카메라 공간에서의 뷰 프러스텀의 모든 ``` z ``` 값은 음수를 가지기 때문에 ``` -z ```는 양수의 값을 가진다.

또한 여기서 알 수 있는 점 중 하나는, 카메라 원점으로부터 멀리 떨어질수록 ``` -z ``` 값이 커지고 **카테시안 좌표의 값은 작아지게된다.** 즉, 멀리 떨어져있는 물체는 투영 변환을 진행하면서 상대적으로 작아지게 되고 곧 이것은 **원근법** 구현을 의미한다. 이렇게 ``` -z ```로 나누며 원근법을 구현하는 나눗셈을 **원근 나눗셈(Perspective Division)**이라 부른다.

원근 나눗셈을 진행한 후의 정육면체에서 정의된 카테시안 좌표를 **정규화된 장치 좌표(Normalized Device Coordinates)** 혹은 줄여서 **NDC**라고 부른다. 해당 정육면체 내에 존재하는 모든 ``` x, y, z ``` 좌표는 **[-1, 1]**의 범위를 가지기 때문에 정규화라는 이름이 붙었다.

***

## 👻 뒷면 제거
카메라가 구를 보고 있다고 가정할 때 구 뒤쪽의 안 보이는 삼각형들을 **뒷면(Back Face)**이라고 부른다. 반대로, 카메라를 향하는 폴리곤은 **앞면(Front Face)**이라고 부른다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/camera-space-sphere.png)   

> 💡 삼각형이 투영선과 수평의 관계를 가지면 **선분**으로 보이게 될 것이다. 이러한 폴리곤은 **변만 보이는 면(Edge-On Face)**라고 한다.

***

### 🌱 개념

여기서, 보이지 않는 뒷면에 속하는 폴리곤들은 처리하지 않도록 제거해야하는데, 이 과정을 **뒷면 제거(Back-Face Culling)**이라고 한다.

폴리곤 메시에서, 해당 폴리곤이 뒷면인지 확인하는 방법에 대해 알아보자. 아래 이미지는 카메라 공간과 투영 변환을 통해 변환된 클립 공간을 나타내는 이미지이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/camera-space-ndc.png)   

투영 변환 후의 클립 공간에서는 카메라로 모이는 모든 투영선들이 수평을 이루게 될 것이고, **단일한 투영선(Universal Projection Line)**이라 정의할 것이다. 여기서 이 투영선과 각 폴리곤의 노멀을 **내적**하게 되면 세 가지 경우가 나오게 될 것이다. 투영선과 노멀이 직교를 이루면 내적이 **0**이 되고 이는 곧 **Edge-On Face**를 의미하게된다. 투영선과 노멀이 둔각을 이루면 내적은 **음수**가 되고, 해당 삼각형은 **뒷면**으로 판정된다. 반대로 투영선과 노멀이 예각을 이루면 내적은 **양수**가 되고, 해당 삼각형은 **앞면**으로 판정된다.

***

### 🌱 구현
이제 뒷면과 앞면을 구분하게 되었으니, 뒷면을 제거하려면 어떻게 해야하는지 알아보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/xy-sphere.png)   

다음은 투영 변환이 끝난 후의 폴리곤 메시이다. 3차원에서는 완벽한 구 형태였지만, 투영 변환이 진행되면 멀리 있는 폴리곤은 작게, 가까이 있는 폴리곤은 크게 변환되어 '왜곡된' 구 형태로 변하게 된다. 투영 변환 이후에는 모든 투영선이 ``` z ```축과 나란해지기 때문에, 여기서 ``` z ```축의 값은 잠시 무시하고 ``` xy ``` 평면에 대해서만 생각해보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/back-front.png)   

다음은 위의 왜곡된 구에서 ``` xy ``` 평면으로 투영해 바라본 삼각형 ``` t₁ ```, ``` t₂ ```이다. 아래첨자 순서대로 정점이 정렬되어있다고 했을 때, 삼각형 ``` t₁ ```은 **시계 방향**으로, 삼각형 ``` t₂ ```는 **반시계 방향**으로 정점이 정렬되어있는 것을 알 수 있다. 우리가 구의 외부에서 바라봤을 때 반시계 방향으로 정점을 정렬해두었기 때문에, ``` t₁ ```은 **구의 내부에서 바라보는 것**이라고 생각할 수 있다. 이는 곧 **뒷면**을 의미하게 되고, 컴퓨터에서는 이를 간단한 **행렬식**으로 알 수 있게 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/vector.png)   

위의 식을 이용하면 컴퓨터가 해당 폴리곤의 위치를 알 수 있게 된다. 각 식은 ``` v₁ 👉 v₂```, ``` v₁ 👉 v₃ ``` 벡터를 이용한 것이고, 대각선 방향으로 계산하여 값을 구한다. 0이면 **Edge-On Face**, 양수면 **앞면**, 음수면 **뒷면**을 의미한다.

뒷면 제거 단계는 하드웨어에서 처리하는 것은 똑같지만 앞서 보았던 **클리핑, 원근 나눗셈**과 다르게 우리가 직접 설정할 수 있다. 최적화와 연결되어있으니 반드시 신경을 써주도록 하는 것이 좋다. 다음과 같은 경우에 뒷면 제거를 잘 설정하면 쉽게 표현할 수 있을 것이다.

> - **반투명(Translucent)한 구**를 표현하는 경우, 뒷면이 제거되면 안 되기 때문에 경우에 따라 뒷면을 살릴 수도 있다.
- 앞면을 컬링하는 경우, 구를 반으로 자른 단면을 보는 효과를 얻을 수 있다.

따라서 **뒷면 제거**는 반드시 **뒷면만** 제거하는 것이 아닌, 반대로 앞면을 제거하여 응용할 수 있다. 해당 기능은 GL에서 컨트롤할 수 있도록 ``` glEnable ```, ``` glDisable ``` 함수를 제공한다.

> - **뒷면 제거 활성화**
  - ``` glEnable(GL_CULL_FACE) ```
  - Default Value
    - ``` GL_FALSE ```
- **뒷면 제거 비활성화**
  - ``` glDisable(GL_CULL_FACE) ```
- **뒷면과 앞면 중 어느 것을 제거할 것인지 선택**
  - ``` glCullFace ```
  - modes
    - ``` GL_FRONT ``` : 앞면 제거
    - ``` GL_BACK ``` : 뒷면 제거(Default Value)
    - ``` GL_FRONT_AND_BACK ``` : 앞면, 뒷면 모두 제거
- **앞면의 정점 정렬 순서 설정**
  - ``` glFrontFace ```
  - modes
    - ``` GL_CW ``` : 시계 방향
    - ``` GL_CCW ``` : 반시계 방향(Default Value)

앞면이라고 해서 반드시 보이는 것은 아니며, 또 다른 앞면에 의해 가려질 수 있다. 이러한 앞면들은 **z-버퍼링(Z-Buffering)**이라고 하는 프래그먼트별 연산에 의해 처리된다.

***

## 👻 뷰포트 변환
뒷면 제거까지 끝냈다면, 클립 공간에서의 연산은 끝이난다. 이제, 클립 공간에서 또 다른 공간으로 변환해야한다. 클립 공간의 다음 단계는 **윈도우 공간(Window Space)** 혹은 **스크린 공간(Screen Space)**라고 부른다. 해당 공간은 윈도우가 그 자신의 좌표계를 가지는 공간이다. 이 좌표계는 **윈도우의 왼쪽 아래 모퉁이에 원점**을 가지며, 이 윈도우 내에서도 NDC 정육면체 뷰 볼륨 안의 내용이 **최종적으로 그려질 스크린 영역**을 **뷰포트(Viewport)**라고 한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/viewport.png)   

윈도우에서 뷰포트의 범위는 ``` glViewport ```에 의해 정의되며 해당 함수는 다음과 같은 파라미터를 가진다.

> - **minX** : 뷰포트 왼쪽 아래 모퉁이의 스크린 공간 ``` x ``` 최소 좌표
- **minY** : 뷰포트 왼쪽 아래 모퉁이의 스크린 공간 ``` y ``` 최소 좌표
- **w** : 뷰포트 너비
- **h** : 뷰포트 높이

뷰포트의 **종횡비(Aspect Ratio)**는 ``` w/h ```이며, 뷰 프러스텀 파라미터 ``` aspect ```와 동일하게 설정하는 것이 좋다.

실제 스크린 공간은 위의 그림과 같이 3차원이다. 3차원 스크린 공간의 ``` z ```축은 **스크린 안쪽**을 향한다. 이는 ``` glDepthRange(minZ, maxZ) ```를 호출하여 정의할 수 있다. ``` z ```범위의 기본 값은 **[0, 1]**이다.

> 💡 스크린 공간은 **왼손 좌표계**를 따른다. 그렇기 때문에 클립 공간도 왼손 좌표계 기준으로 설정되어있다. 카메라 공간에서 클립 공간으로 넘어갈 때 오른손 좌표계에서 왼손 좌표계로의 변환도 동시에 실행해주는 이유이기도 하다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/viewport-transform.png)   

클립 공간에서 이러한 뷰포트로의 전환을 **뷰포트 변환(Viewport Transform)**이라고 한다. 뷰포트 변환은 **축소확대에 이은 이동**으로 표현된다. 뷰포트 변환 행렬은 축소확대와 이동 행렬의 결합으로써 정의할 수 있으며, 결합된 행렬은 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/viewport-transform2.png)   

이는 ``` 2 × 2 × 2 ``` 크기의 뷰 볼륨 안에 있는 모든 정점에 적용된다.

대개의 경우 ``` minZ ```와 ``` maxZ ```는 디폴트로 **0**과 **1**을 사용하며 ``` minX ```와 ``` minY ```는 **원점**을 사용한다. 이 값의 의미는 뷰포트가 윈도우 전체와 동일한 크기를 가진다는 것이며, 이러한 경우 **뷰포트 변환 행렬**은 더욱 간단하게 정의할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/viewport-transform3.png)   

> 💡 각 공간은 가상의 육면체들이며, 실제 변환은 **폴리곤 메시에 적용**된다.

***

## 👻 스캔 전환
뷰포트 변환을 수행하면 모든 삼각형들은 스크린 공간으로 옮겨지게 된다. 이후 래스터라이저의 마지막 세부 단계인 **스캔 전환(Scan Conversion)**이 수행된다. 이 단계에서는 정점 쉐이더 단계에서 구한 **정점 노멀**과 **텍스처 좌표**를 마지막으로 처리해주게 된다. 정점 쉐이더에서 정점 노멀과 텍스처 좌표는 월드 공간 단위에서 존재하게 되는데 이러한 값들을 스크린 공간에서 처리할 수 있도록 해주는 변환이다. 임무는 **삼각형 내부를 채우는 프래그먼트를 생성**하는 것이다.

스크린 공간의 각 정점은 정점 노멀과 텍스처 좌표를 가지고 있다. 정점이 만들어낸 삼각형 내부에 **픽셀(Pixel)**들을 채워야하는데, 각 픽셀에 들어갈 값들을 정점 간의 **보간(Interpolates)**을 통하여 정의할 수 있다. 각 픽셀에 데이터를 일일이 저장하면 데이터가 너무 많아지기 때문에 이러한 방식을 사용하는 것이 성능면으로 좋다.

<img src="/assets/images/posts_img/basics/computer-graphics/rasterizer/3d-triangle.png" width="40%" height="" style="margin-right: 2rem;">
<img src="/assets/images/posts_img/basics/computer-graphics/rasterizer/2d-triangle.png" width="40%" height="">

위 왼쪽 이미지는 스크린 공간에서 어떠한 삼각형에 대해 나타낸 것이다. 이 삼각형을 ``` xy ``` 평면에 투영하여 살펴보면 오른쪽 이미지처럼 나타나게 될 것이다. 이 삼각형 내부에 픽셀을 채운다고 가정해보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/interpolates.png)   

삼각형 내부에 총 18개의 픽셀이 들어가게 되는데, 각각의 픽셀을 정점 간의 보간을 이용하여 들어갈 값을 구할 것이다. 이해하기 쉽게 각 정점에 **RGB** 값이 들어가있다고 가정하여 보간해보자. 실제로는 **정점 노멀**과 **텍스처 좌표**에 대해 보간되어 값들이 들어가게 될 것이다.

정점별 애트리뷰트는 우선 삼각형의 변을 따라 **선형 보간(Linear Interpolation)**된다. 위 이미지는 정점 ``` R₁ ```과 ``` R₃ ```가 왼쪽 변을 따라 보간되는 것을 보여준다. 보간을 위해서는 각 변마다 몇 가지 **기울기**를 계산하는 것이 필요하다. 우선, 수직 거리 ``` y ```의 변화에 따른 ``` R ```의 변화율을 나타내는 ``` ΔR/Δy ```이 필요하다. 또한, ``` x ```의 변화율을 나타내는 ``` Δx/Δy ```도 필요하다.

여기서 수평 방향으로 이어진 스크린 픽셀들을 **스캔 라인(Scan Line)**이라 부른다. 위 이미지에서 삼각형과 교차하는 첫 스캔라인의 ``` y ``` 좌표는 **1.5**이며, 여기서 각 기울기가 어떻게 계산되어 초기값을 가지게 되는지 보여준다. 이러한 초기화 단계를 거친 후, 그 다음 스캔라인부터는 각 기울기 값을 더하는 작업을 반복하여 왼쪽 변을 따라 각각의 값을 구할 수 있게 된다. 왼쪽 변을 따라 보간된 결과는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/scan-line.png)   

삼각형의 다른 두 변에 대해서도 같은 작업이 이루어지게 되며, 모든 작업이 끝나게되면 **삼각형의 모든 변**에 대하여 스캔 라인과 교차하는 양 끝 지점의 ``` R ```과 ``` x ```좌표를 알 수 있게 된다. 이제는 스캔 라인을 따라 픽셀에 해당하는 애트리뷰트들을 보간해야하는데, 삼각형 변을 따른 보간과 동일한 기법을 사용할 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/scan-line2.png)   

위 이미지에서, 해당 스캔 라인에 해당하는 모든 픽셀에 대하여 보간을 진행하게 된다. 이미 양 끝 지점의 ``` R ```과 ``` x ```좌표를 알고 있기 때문에 ``` y ```에 대한 ``` R ```의 변화율을 구할 때와 마찬가지로 이번엔 ``` x ```에 대한 ``` R ```의 변화율을 구할 수 있게된다. 그런 다음, 똑같이 첫 지점은 ``` x ``` 좌표의 차이만큼 곱해주어 보간해주고 나머지는 변화율의 덧셈을 통해 보간을 진행하게 된다. 이 작업이 마무리되면 모든 픽셀이 각각의 ``` R ```값을 가지게 될 것이다.

스캔 전환 단계에서 선형 보간은 **두 단계**로, 즉 먼저 변을 따라, 그 다음에는 스캔 라인을 따라 수행되었다. 이를 **겹선형 보간(Bilinear Interpolation)**이라 부른다. ``` G ```와 ``` B ```역시 같은 방법으로 보간되며, 이 과정을 전부 수행하고나면 프래그먼트별 색상을 얻을 수 있다. 다음은 정점의 색상이 보간된 삼각형의 몇 가지 예를 보여준다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/color-interpolation-examples.png)   

GPU 렌더링 파이프라인에서 래스터라이저 다음 단계인 프래그먼트 쉐이더는 이렇게 보간된 정점 노멀과 텍스처 좌표를 이용하여 한 번에 하나씩 프래그먼트를 처리하게 될 것이다. 

> 💡 정점 노멀이 보간된 픽셀들을 **프래그먼트(Fragment)**라고 한다. 컬러 버퍼의 픽셀을 갱신하는 데 필요한 데이터를 총칭하며, 이러한 프래그먼트로 물체를 채우게 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/rasterizer/interpolates2.png)   
<span style="font-size: 0.7rem; color: gray;">위 이미지는 겹선형 보간을 통하여 변환된 한 스캔 라인에 위치한 정점 노멀들을 보여준다.</span>

> 💡 보간된 정점 노멀과 텍스처 좌표를 이용해 실제 색상을 결정하게 된다.

***

## 👻 글을 마치며
이번 시간에는 렌더링 파이프라인의 두 번째 단계인 래스터라이저에 대해 알아보았다. 다렉 공부할 때는 큰 틀만 잡아서 보다보니 정확하게 이해하는 게 힘들었는데 그래픽스 공부하면서 한 단계씩 자세하게 알아보게되니 이해가 잘 되는 것 같다. 다만 내용이 어렵고 방대해서 한 챕터씩 공부하는 게 에너지 소모가 꽤나 큰 것 같다.. ㅠㅠ 천천히 꾸준히 해야할 것 같다. ~~그래픽스는 절대 벼락치기가 불가능한 과목이라는 것을 공부하면 할 수록 깨닫는 것 같다..~~

***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (7장-래스터라이저)](https://youtu.be/wqttEQOY4Ek)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   