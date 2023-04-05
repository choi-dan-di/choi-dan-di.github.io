---
title: "[Computer Graphics] #8. 이미지 텍스처링"
excerpt: "한정현 컴퓨터 그래픽스 강의 8장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, image texturing, texturing]

permalink: /computer-graphics/image-texturing/

toc: true
toc_sticky: true

date: 2023-04-05 12:37:30+0900
last_modified_at: 2023-04-05 12:37:33+0900
published: true
---

## 👻 이미지 텍스처링
래스터라이저가 출력하는 프래그먼트별 애트리뷰트는 대체로 **노멀**과 **텍스처 좌표**를 포함한다. 이러한 정보들을 이용하여 프래그먼트 쉐이더는 **프래그먼트의 색상**을 결정한다. 그러기 위해서는 **라이팅(Lighting)**과 **텍스처링(Texturing)**이 필요한데, 이번 시간에는 **텍스처링**에 대해서 알아볼 것이다.

***

## 👻 텍스처 좌표
![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/image-texturing.png)   

텍스처링은 여러가지 방법이 있는데, 가장 기본적인 것이 **이미지 텍스처링(Image Texturing)**이다. 일반적으로 텍스처는 배열 구조를 가지는데, 이 배열이 **색상 정보를 저장**하고 있다면 **이미지 텍스처(Image Texture)**라 부른다.

텍스처는 **텍셀(Texel; Texture Element)**로 구성되며, 이미지 텍스처의 텍셀은 **픽셀(Pixel; Pictur Element)**과 동일한 것이지만 구분상 편의를 위해 이름을 따로 붙였다. 각각의 텍스처는 ``` (x, y) ``` 좌표를 가지고 있으며 텍셀의 경우 **중심점**으로 모든 위치를 표현할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/scan-conversion-and-texturing.png)   

텍스처를 구현하기 위해서는 모델링 단계에서 각 정점마다 **텍스처 좌표** ``` (s, t) ```가 설정되어있어야한다. 래스터라이저의 스캔 전환이 완료되면 각 프래그먼트는 보간된 ``` (s, t) ``` 좌표를 가지게되며, 각 좌표는 원칙적으로 **[0, 1]** 범위 안에 **정규화**되어 있다.

해상도가 **r<sub>x</sub> × r<sub>y</sub>**인 텍스처가 주어졌을 때, 정규화된 텍스처 좌표 ``` (s, t) ```는 아래와 같은 방식으로 구하여 **실제 텍스처 공간으로 매핑**된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/normalized-texcoord.png)   

이를 가리켜, 텍스처 좌표 ``` (s, t) ```가 텍스처 공간의 ``` (s', t') ```로 **투영(Project)**되었다고 표현한다. 우리가 지금 공부하고 있는 것은 이미지 텍스처기 때문에 각각의 좌표에 **RGB**값이 할당되어있고, 이 값을 가져와서 프래그먼트에 입히게된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/normalized-texcoord2.png)   

폴리곤 메시 정점들에 정규화된 텍스처 좌표를 할당하면, 다른 해상도를 가지는 여러 개의 텍스처를 **텍스처 좌표 변경 없이** 사용할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/normalized-texcoord3.png)   

위의 예시는 원통 모양의 폴리곤 메시에 해상도가 다른 두 이미지 텍스처를 입힌 모습이다.

> 💡 텍스처는 2차원으로 국한되지 않는다. **CT**나 **MRI** 등과 같이 층층이 쌓인 영상은 **3차원 텍스처**에 해당하며, 이를 **볼륨 텍스처(Volume Texture)**라고 부른다. 컴퓨터 그래픽스에서는 2차원 텍스처가 압도적으로 많이 사용된다.

***

## 👻 표면 파라미터화
모델링 단계에서 어떻게 정점마다 텍스처 좌표를 할당할까? 3차원인 폴리곤 메시의 각 정점마다 텍스처 좌표를 할당하려면 해당 폴리곤 메시를 2차원으로 변환하는 방법을 생각할 수 있을 것이다. 이렇게 3차원 물체를 2차원으로 변환하여 비율에 맞춰서 각 정점마다 정규화된 ``` (s, t) ``` 좌표를 할당하는 작업을 **표면 파라미터화(Surface Parameterization)** 혹은 단순히 **파라미터화**라고 부른다. 여기서 **Surface**는 **폴리곤 메시**를 뜻한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/surface-parameterization.png)   

> 💡 3차원 폴리곤 메시를 2차원 평면으로 펼치는 과정에서는 대부분 **왜곡 현상**이 발생한다. 이러한 왜곡을 최소화하는 많은 파라미터화 기술이 컴퓨터 그래픽스 분야에서 꾸준히 연구되고 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/chart-and-atlas.png)   

복잡한 폴리곤 메시가 주어졌을 때에는 해당 메시를 분리하여 파라미터화된다. 이렇게 분리되어 생성된 여러 개의 텍스처를 **패치(Patch)**라고 하며, 이러한 텍스처에 색을 입혀 이미지화 시키면 **차트(Chart)**가 된다. 대개의 경우, 효율적인 렌더링을 위해 여러 차트를 하나의 커다란 텍스처에 모은다. 이를 **아틀라스(Atlas)**라고 부른다. 위의 이미지에서 **(a)**는 차트와 패치가 겹쳐진 것을, **(b)**는 아틀라스를 보여주고있다.

***

## 👻 Texturing in GL
GL에서의 텍스처링 정의는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/texturing-in-gl.png)   

텍스처의 구조체는 **텍셀**과 **가로**, **세로** 값을 가진다. 텍셀은 색상값인 **RGBA** 값을 가진다.

> 💡 **A**   
추가적인 정보를 의미하는 **alpha**의 **a**를 사용한 것으로, RGB 색상의 **불투명도(Opacity)**를 결정한다. **[0, 1]** 범위를 가지며 0에 가까울수록 투명하고 1에 가까울수록 불투명하다.

버퍼의 생성과 비슷하게 다음과 같은 3단계로 진행된다.

1. ``` glGenTextures ```
  - **텍스처 오브젝트(Texture Object)** 생성
2. ``` glBindTexture ```
  - 생성된 텍스처 오브젝트를 특정 텍스처에 **바인드(Bind)**
3. ``` glTexImage2D ```
  - 텍스처 오브젝트를 실제 텍스처로 채운다.

> 💡 ``` glTexImage2D ```   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/gl-tex-image-2d.png)   
텍스처 오브젝트가 바인딩된 후에 호출되는 함수들은 기본적으로 바인딩된 텍스처 오브젝트를 사용하기 때문에 따로 텍스처 오브젝트 ID를 필요로 하지 않는다.   

***

## 👻 텍스처 포장
텍스처 좌표는 원칙적으로 정규화된 값으로 **[0, 1]** 범위 안에서 정의되지만, OpenGL은 실제로 해당 범위를 넘어서는 텍스처 좌표도 허용한다. **[0, 1]** 범위 내에 있는 좌표는 그대로 매핑이 가능하지만, 범위 밖의 좌표들은 따로 처리가 필요하다. 이를 **텍스처 포장(Texture Wrapping)**이라고 부르며, 사용자는 이러한 **텍스처 포장 모드(Texture Wrapping Mode)**를 설정하여 그 처리 방식을 조절할 수 있다. 텍스처 포장 모드는 **세 가지**로 구분된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/wrapping.png)   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/wrapping2.png)   

- **경계 고정 모드(Clamped-to-Edge Mode)**
  - **(c)**
  - 가장 간단한 방법이다.
  - **[0, 1]** 범위의 **경계(Edge)**로 **고정(Clamp)**시킨다.
  - 1을 넘어선 값은 모두 1로 지정한다.
  - 별로 좋은 방법은 아니다.
- **반복 모드(Repeat Mode)**
  - **(d)**
  - 이미지 텍스처를 반복하는 방법이다.
  - 범위를 넘어서면 소수점 이하만 허용하여 좌표값을 재설정한다.
  - 1을 넘어선 값은 정수를 지운 소수점만 사용한다.
  - 텍스처 타일 사이의 경계선이 두드러져 보일 수 있다는 단점이 있다.
- **반사 반복 모드(Mirrored-Repeat Mode)**
  - **(e)**
  - 좌표값을 역으로 감소, 증가시키는 방법이다.
  - 1을 넘어선 값은 역으로 소수점만큼 감소시킨다.
    - 1.0, 0.9, 0.8, ...

텍스처 포장 모드는 ``` s ```축, ``` t ```축에 따라 서로 다른 모드를 사용할 수 있다. 위 이미지의 **(f)**는 ``` s ```축 기준 반복 모드, ``` t ```축 기준 반사 반복 모드를 적용시킨 예이다.

GL에서 텍스처 포장 모드는 ``` glTexParameteri ```를 이용하여 지정된다.   

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/gl-tex-parameter-i.png)   

위 이미지의 **(f)**와 같은 결과는 아래와 같이 작성할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/gl-tex-parameter-i-example.png)   

이러한 함수의 특성을 이용하면 하나의 텍스처를 총 6가지 방법으로 포장할 수 있다.

> 💡 **왜 OpenGL은 [0, 1] 범위를 넘어서는 텍스처 좌표를 허용할까?**   
벽돌과 같이 반복되는 이미지의 텍스처를 제작할 경우, 일부만 잘라 복사하면 쉽게 만들 수 있기 때문이다. 범위의 경계인 1을 넘어가는 좌표값은 1을 빼버리면 무한 반복하며 정수 단위로 새로운 이미지가 붙여지게 될 것이다. 이를 **타일링(Tiling)**이라고 부른다.

**

## 👻 텍스처 필터링
프래그먼트의 텍스처 좌표 ``` (s, t) ```는 해상도와 곱해져 텍스처 공간의 ``` (s', t') ```로 투영된다. 해당 픽셀의 정중앙을 항상 찾기란 쉽지 않다. 그러면, ``` (s', t') ``` 주변의 텍셀을 고려해서 해당 프래그먼트의 색상을 결정해야 하는데, 이를 **텍스처 필터링(Texture Filtering)**이라고 부른다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/texture-filtering.png)
![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/image-texture.png)   

다음과 같은 폴리곤 메시에 우측의 이미지를 텍스처링하는 경우를 생각해보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/magnification.png)   

위 이미지는 폴리곤 메시의 크기가 커졌을 때를 나타낸다. 기존의 이미지 텍스처는 지금의 폴리곤 메시보다 크기가 훨씬 작기 때문에 텍스처의 크기도 덩달아 **확대(Magnify)**되어야 한다. 이미지 텍스처의 우측은 픽셀과 텍셀 그리드를 나타내는데, 텍스처가 확대되면서 상대적으로 텍셀의 크기도 커지게 되고, 하나의 텍셀 안에 하나 이상의 픽셀이 자리를 잡게 된다. 즉, **텍셀보다 픽셀의 수가 많아진 것**을 알 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/minification.png)   

반대로, 폴리곤 메시가 축소되었을 때, 이미지 텍스처도 폴리곤 메시의 크기에 맞게 덩달아 **축소(Minify)**되어야 할 것이다. 이렇게 되면 텍스처를 확대했을때와는 반대로 **텍셀이 픽셀보다 상대적으로 많아진 것**을 알 수 있다. 이렇게 픽셀들이 텍스처 공간 내에 듬성듬성 투영된 것을 **Large Jumps**라고도 한다.

이렇게 텍셀과 픽셀의 수를 비교하여 텍스처가 확대되어야 하는지, 혹은 축소되어야 하는지를 알 수 있게 된다.

***

### 🌱 텍스처 확대
텍스처가 확대될 때 사용되는 필터링 방법 중 하나는 **근접점 샘플링(Nearest Point Sampling)**이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/nearest-point-sampling.png)   

자신이 속한 텍셀의 값을 그대로 가져와 사용하며, 각각의 픽셀에 정의된 ``` (s', t') ``` 좌표값 중 **가장 가까운 텍셀**의 RGB값을 가져오는 방법이다. 가장 간단한 기법이지만 네 개의 픽셀이 같은 색을 가지게 되면 **모자이크화(Blocky Image)** 되는 부작용이 있다.

이러한 부작용을 해소하기 위해 사용하는 다른 방법이 바로 **겹선형 보간(Bilinear Interpolation)**을 이용한 기법이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/bilinear-interpolation.png)   

해당 기법은 ``` (s', t') ``` 주위를 둘러싸는 네 개의 텍셀을 겹선형 보간하여 해당 픽셀의 텍스처 색상을 결정하는 방법이다. 각각의 텍셀은 중심점이 존재하고 픽셀 주변을 둘러싼 이 네 텍셀의 중심 좌표와 픽셀의 투영 좌표를 이용하면 투영된 픽셀의 상대적인 위치를 알 수 있게 된다. 너비는 1이며 투영된 픽셀의 상대적 위치를 구하고 텍셀의 겹선형 보간을 이용하여 RGB값을 구할 수 있게 된다.

> 💡 텍스처 확대에 사용되는 겹선형 보간은 ``` x ```축으로 한 번, ``` y ``` 축으로 한 번 수행된다.

이러한 방식을 사용하게 되면, 동일한 텍셀 영역에 존재하는 픽셀들이더라도 보간된 서로 다른 값을 가지게 되므로 Blocky Image 문제를 어느정도 해결할 수 있게 된다. 물론 선호도도 근접점 샘플링 방식보다 겹선형 보간 방식이 더 높다.

***

### 🌱 텍스처 축소
반대로 텍스처가 축소될 때 텍스처 필터링이 어떤 식으로 이루어지는지 알아보자. 다음과 같은 체커보드 이미지의 텍스처가 있다고 가정해보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/checker-board.png)   

이 텍스처를 축소시키게 되면, 텍셀의 수보다 픽셀이 훨씬 더 적어지게 되고 하나의 픽셀이 공유하지 못하는 텍셀도 생기게 될 것이다. 픽셀이 텍스처의 검은 부분에 겹치게 된다면 전부 검은색으로 칠해지고 회색 부분에 겹치게 된다면 전부 회색으로 칠해진 결과가 나오게 될 것이다. 이렇게 잘못된 결과값을 도출하게 되며 기존의 텍스처가 제대로 텍스처링되지 않는 문제가 발생하게된다.

> 💡 이 문제는 **앨리어싱(Aliasing)**의 한 예이다. 앨리어싱은 고주파 신호를 낮은 빈도로 샘플링할 때에 발생하는 오류를 의미하며 신호 처리와 컴퓨터 그래픽스 분야에서 빈번하게 발생하는 문제이다. 이러한 앨리어싱이 일으키는 부작용을 최소화하기 위해선 **안티 앨리어싱(Anti-Aliasing)** 기술이 필요하다.

***

## 👻 밉매핑
텍스처를 축소할 때 발생하는 앨리어싱은 픽셀에 비해 텍셀이 너무 많아서 텍스처링에 참여하지 못하는 텍셀이 발생하는 것이 원인이다. 이 문제를 해결하기 위해선 **텍스처를 작게 만들어 텍셀 수를 픽셀 수에 맞추면 된다.** 바로 이렇게 텍셀 수를 줄이는 기법을 **밉매핑(Mipmapping)**이라고 부른다.

***

### 🌱 밉맵 생성
우선 **밉맵(Mipmap)**을 만들기 위해선 원본 텍스처의 크기를 줄여나가야 한다. 텍스처를 작게 만들기 위해선 가로 세로 두 개의 텍셀들을 묶어 **색상의 평균**을 구하는 작업을 반복해야한다. 그러다보면 원본 텍스처 전체의 평균 색상을 담고 있는 ``` 1 × 1 ``` 크기의 텍스처를 얻을 수 있게 되는데, 이 과정을 **다운 샘플링(Down-Sampling)**이라고 한다. 밉맵을 만드는 가장 간단한 방법이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/mipmap.png)   

다운 샘플링 과정을 계속해서 거치다보면 여러 개의 텍스처를 만들어낼 수 있다. 이렇게 해서 생긴 **피라미드 구조의 텍스처들**을 **밉맵(Mipmap)**이라고 부른다.

원본 텍스처의 해상도가 **2<sup>l</sup> × 2<sup>l</sup>**일 때, 원본 및 다운샘플된 텍스처들은 총 **(l + 1)개**의 **레벨**을 가지게된다. 원본 텍스처는 **레벨 0**에 위치한다.

> 💡 **접두어 Mip**   
접두어 mip은 라틴어 **'multum in parvo'**의 약자로 **작은 공간 안에 있는 많은 것들**을 의미한다.

***

### 🌱 밉맵 필터링
기존에 우리는 픽셀을 하나의 동그라미로 그려왔다. 그리고 그 픽셀은 텍스처 공간의 한 **점**인 ``` (s', t') ```로 투영된다고 설명해 왔다. 하지만 **실제로 픽셀은 스크린 공간의 작은 영역을 덮고 있다.** 이 영역은 ``` (s', t') ``` 좌표를 중심으로 일정한 **영역**을 차지하게 된다. 이 영역을 픽셀의 **발자국(Footprint)**이라고 부른다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/mipmapping.png)   

다소 작위적인 예제를 이용해서 밉맵 필터링이 어떻게 이루어지는지 알아보자. 위 이미지에서, 픽셀의 발자국을 기준으로 다운 샘플링이 진행되며 텍스처의 크기가 줄어들게 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/mipmapping2.png)   

위 이미지는 처음의 폴리곤 메시 크기보다 훨씬 더 작을 때의 밉맵 필터링 과정을 보여준다. 물론 다운 샘플링 과정은 동일하고 큰 이상없이 진행된다.

밉맵 0번 레벨에 있는 원본 텍스처의 RGB값은 그 상위 레벨에 모두 포함되어 있다. 따라서 밉맵의 어느 레벨을 골라 필터링을 하건, 원본 텍스처의 텍셀이 배제되는 경우는 없다. 이렇게 해서 앨리어싱 문제를 해결하는 것이다.

필터링을 수행할 밉맵의 레벨을 통상적으로 **λ(람다; Lambda)**로 표기하는데, 픽셀의 발자국이 레벨 0 텍스처에서 **m × m** 텍셀을 차지하는 경우, λ는 ``` log₂m ```으로 설정된다. 2로 나눠떨어지는 경우 λ는 정수가 되겠지만, 대게의 경우 ``` m ```은 2의 거듭제곱이 아닐 것이다. 따라서 λ는 정수가 아닌 **실수**가 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/mipmapping3.png)   

위 이미지에서, 픽셀의 발자국은 ``` 3 × 3 ``` 텍셀들을 차지하고, ``` log₂3 ```은 **1.585**와 근접한 수를 가진다. 이처럼 정수가 아닌 λ가 계산되면, 간단하게 **λ에 가장 가까운 레벨을 선택**하는 방법을 사용할 수 있다. 이 레벨은 ``` Floor[λ + 0.5] ```로 쉽게 확인되며, 이 레벨 또한 근접점 샘플링 혹은 겹선형 보간을 통해 필터링될 수 있다.

이 방법보다 좋은 방법은 ``` Floor[λ] ```와 ``` Ceil[λ] ``` 두 레벨을 모두 선택해서, **각 레벨을 필터링한 결과를 선형 보간하는 것**이다. 이러한 방법을 **삼선형 보간(Trilinear Interpolation)**이라 부르며, 이 레벨들 또한 근접점 샘플링 혹은 겹선형 보간을 통해 필터링될 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/mipmapping4.png)   

가장 좋은 방법은 **삼선형 보간**을 사용하는 방법이다.

> 💡   
- 겹선형 보간 시 p의 값은 λ의 소수점에만 해당되는 값이다.
- 픽셀마다 발자국의 크기와 모양은 다르다.
- 픽셀 발자국은 정사각형으로 되는 경우가 거의 없다.

***

## 👻 Mipmapping in GL
GL은 스크린 공간 각각의 픽셀(프래그먼트)에 대해 그 발자국을 계산한 후 **텍스처가 확대되어야 하는지 아니면 축소되어야 하는지 결정**한다. OpenGL이 나머지 부분은 알아서 처리해주지만, 사용자는 텍스처의 확대 및 축소가 될 때 텍스처를 어떻게 **필터링**해줄건지 정해줘야한다. 이 부분은 앞서 알아보았던 ``` glTexParameteri ``` 함수를 이용하여 지정할 수 있다.

두 번째 파라미터 ``` pname ```에 들어갈 값은 다음과 같다.

> - ``` GL_TEXTURE_MAG_FILTER ``` : 텍스처 확대
- ``` GL_TEXTURE_MIN_FILTER ``` : 텍스처 축소

세 번째 파라미터 ``` param ```은 텍스처가 확대될 때와 축소될 때 들어갈 수 있는 값이 다르다. 먼저 **확대**는 밉맵을 사용하지 않고 원본 텍스처를 사용하기 때문에 간단하게 두 가지 방법을 사용한다.

- **텍스처 확대일 때** ``` param ``` **에 들어갈 값**

> - ``` GL_NEAREST ``` : 근접점 샘플링
- ``` GL_LINEAR ``` : 겹선형 보간

반대로 텍스처 축소는 밉맵을 사용하는 경우를 고려해야한다. 그래서 ``` param ```에 들어갈 파라미터는 위의 2개에 밉맵을 사용하는 경우 4개가 추가되어 총 6개의 파라미터 값을 가진다.

- **텍스처 축소일 때** ``` param ``` **에 들어갈 값**

> - No Mipmapping
  - ``` GL_NEAREST ```
  - ``` GL_LINEAR ```
- Mipmapping
  - ``` GL_NEAREST_MIPMAP_NEAREST ```
  - ``` GL_NEAREST_MIPMAP_LINEAR ```
  - ``` GL_LINEAR_MIPMAP_NEAREST ```
  - ``` GL_LINEAR_MIPMAP_LINEAR ``` 👍

위 밉맵 필터링을 사용하지 않는 두 방식은 원본 텍스처를 그대로 사용하게 되며 앞서 보았던 앨리어싱 문제 등이 발생할 확률이 높다. 추천하지 않는 옵션이다.

아래 네 개의 파라미터는 밉맵 필터링을 사용하는 방식이다. 모두 ``` GL_a_MIPMAP_b ```의 형식을 가진다. 여기서 ``` b ```는 **레벨을 선택하는 방법**이다. **NEAREST**는 **가까운 레벨 하나를 선택**하겠다는 의미이고, **LINEAR**는 **두 개의 레벨을 선택하여 겹선형 보간**하겠다는 의미이다. ``` a ```는 **필터링 방법**이다.

가장 하단의 ``` GL_LINEAR_MIPMAP_LINEAR ```는 삼선형 보간을 사용하는 가장 좋은 옵션이라고 볼 수 있다.

> 💡 밉맵은 모두 자동으로 만들어주지만 사용자가 밉맵 자체를 생성하라는 Function Call이 필요하다.

***

## 👻 텍스처링을 위한 프래그먼트 쉐이더
위의 모든 과정은 프래그먼트 쉐이더 내에서 수행된다. 하나의 프래그먼트는 하나의 프래그먼트 쉐이더가 실행되면서 처리되며 이 과정은 병렬적으로 처리된다.

정점 쉐이더와 마찬가지로 사용자가 직접 구현해야 하는 부분이며, 똑같이 입력값과 유니폼을 받아 출력값을 다음 단계로 넘겨주는 방식이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/fragment-shader.png)   

정점 쉐이더가 출력값으로 보내준 ``` v_normal ```과 ``` v_texCoord ```를 입력값으로 받게된다. 모든 프래그먼트는 동일한 텍스처를 사용하기 때문에 텍스처는 대표적인 유니폼이다. 프래그먼트 쉐이더의 연산이 모두 끝나게 되면 RGB값을 출력하게 된다. GL 2.0에서는 프래그먼트 쉐이더가 단 한 개의 색상을 출력할 수 있었는데 3.0 버전으로 넘어오면서 **여러 색상 출력이 가능**해졌다.

***

### 🌱 코드 분석
![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/fragment-shader2.png)   

위 이미지는 텍스처링을 위한 프래그먼트 쉐이더를 구현한 코드이다. 맨 첫 줄은 GL 3.0 버전을 의미하므로 다음 줄부터 보도록 하자.

- **정밀도(Precision)** 지정

```ini
precision mediump float;
```

쉐이더의 변수는 저/중/고 정밀도 중 하나를 가질 수 있는데, 이는 각각 ``` lowp ```, ``` mediump ```, ``` highp ```라는 키워드로 지정된다. 정밀도는 개별 GPU에 따라 달라지는데, 예를 들어 ``` mediump ```로 정의된 ``` float ``` 타입 변수에는 2바이트를 할당하고, ``` highp ```로 정의된 변수에는 4바이트를 할당할 수 있다. 여기선 이 프래그먼트 쉐이더의 모든 ``` float ``` 타입 변수가 기본적으로 중간 정밀도를 가진다고 지정한다.

- 유니폼 값 넘겨주기

```ini
uniform sampler2D colorMap;
```

유니폼 값으로 ``` sampler2D ``` 타입의 ``` colorMap ```을 넘겨준다. GL에서 텍스처는 sampler라고도 불리며 ``` sampler2D ```는 2차원 텍스처를 의미한다.

- 입력값 넘겨주기

```ini
in vec2 v_texCoord;
```

래스터라이저가 끝난 후 보간된 텍스처 좌표값들을 입력값으로 받는다. 여기서 노멀 값은 따로 받지 않은 것을 알 수 있는데, 정점 쉐이더의 모든 출력을 프래그먼트 쉐이더가 반드시 취해야 하는 것은 아니다. 물론 이렇게 하면 정점 쉐이더와 래스터라이저는 불필요한 작업을 수행한 것이 되는데, 노멀은 라이팅 단계에서 어떻게 활용되는지 알아보게 될 것이다.

- 출력값 정의하기

```ini
layout(location = 0) out vec4 fragColor;

void main() {
  fragColor = texture(colorMap, v_texCoord);
}
```

``` output 0 ```번으로 4차원 벡터값을 출력하겠다는 의미이다. 여기서 텍스처의 결과값은 RGB 이 세 개가 아닌 불투명도 정도까지 포함된 **RGBA**값이 넘겨가게 된다.

또한, 내장 함수 ``` texture ```를 이용하여 ``` colorMap ```을 ``` v_texCoord ```로 필터링하여 ``` fragColor ```를 결정하게 된다.

여기까지 구현을 하게되면 다음과 같이 결과물을 출력할 수 있게 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/image-texturing/fragment-shader-result.png)   

왼쪽의 이미지 텍스처를 유니폼 값으로 넘겨주고 프래그먼트 쉐이더 단계를 진행하게 되면 오른쪽과 같은 결과물이 출력될 것이다. 중간의 결과는 텍스처링만 적용했을 때의 결과물이며 가장 오른쪽의 결과는 텍스처링과 라이팅 모두 적용했을 때의 결과물이다. 텍스처링 단계에서는 모델 표면이 제대로 **음영처리(Shading)**되지 않은 것을 알 수 있다. 이 부분은 라이팅 단계에서 알아보도록 하자.

***

## 👻 글을 마치며
이번 시간에는 이미지 텍스처링에 대해 알아보았다. 확실히 중요한 부분이라 공부할 양이 워낙에 많았다. 포스팅 길이도 꽤 되고 시간도 엄청 오래 걸렸던 것 같다. 그래도 천천히 예복습 하고 정리를 하는 이 시간이 가장 확실하게 개념이 정리되는 것 같다. 그리고 이제야 조금씩 CG를 이해하게되고 컴퓨터가 어떻게 물체에 색을 입히는지 알게 된 것 같다. 개념을 정리하면서 궁금한 부분도 많이 생겼는데, 그 때마다 메모를 해두었더니 양이 꽤 된다. 이제 혼자 스스로 답을 찾아보는 시간을 가져야지! ~~이런 과정도 **자세하게** 포스팅을 해야할까싶다.~~

***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (8장-이미지 텍스처링)](https://youtu.be/ifdbfMWjGdU)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   