---
title: "[Computer Graphics] #14. 노멀 매핑"
excerpt: "한정현 컴퓨터 그래픽스 강의 14장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, normal mapping, normal, map]

permalink: /computer-graphics/normal-mapping/

toc: true
toc_sticky: true

date: 2023-04-25 19:45:56+0900
last_modified_at: 2023-04-25 19:46:00+0900
published: true
---

## 👻 노멀 매핑
실세계의 많은 물체는 오돌토돌한 표면을 가지고 있다. 이러한 물체를 사실적으로 표현하려면 폴리곤이 많은 고해상도 메시를 사용해야 할 것이다. 여기에 텍스처를 입히고 라이팅을 적용한다고 생각해보자. 퐁 라이팅의 디퓨즈 항은 각 정점의 노멀과 빛 벡터를 내적하여 빛의 강도와 색상을 결정한다. 고해상도 메시의 수많은 정점에 대해 처리하려면 많은 시간이 소요된다는 것을 자연스럽게 알 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/mesh1.png)   

빠른 연산을 위하여 고해상도 메시의 폴리곤 수를 최소로 줄여서 비교해보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/mesh2.png)   

2개의 삼각형만으로 구성된 메시에 똑같이 이미지 텍스처를 입히고 라이팅을 적용했을 때, 연산은 아주 빠르겠지만 오돌토돌한 기존의 표면을 그대로 구현할 수 없다. 기존의 고해상도 메시에서, 정점에 따라 각기 다른 노멀 값을 가지는 것도 저해상도 메시로 넘어오면서 모두 동일한 노멀값을 가지는 것을 확인할 수 있다.

이러한 문제를 해결하기 위한 한 가지 방법은, 고해상도 메시의 노멀을 미리 계산하고, 이를 **노멀맵(Normal Map)**이라고 하는 특수한 텍스처에 저장한 후, 런타임에는 저해상도 메시를 처리하되 노멀맵으로부터 노멀을 읽어서 이를 라이팅에 사용하는 것이다. 이러한 특수한 텍스처링 기법을 **노멀 매핑(Normal Mapping)** 혹은 **범프 매핑(Bump Mapping)**이라 부른다.

***

## 👻 하이트맵
![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/height1.png)   

가장 처음 나왔던 고해상도 메시의 표면은 위의 왼쪽 이미지처럼 이른바 **하이트 필드(Height Field)**로 표현할 수 있다. 이는 2차원 좌표 (x, y)가 주어졌을 때 높이(Height) 혹은 z값을 반환하는 함수 h(x, y)이다. 이러한 **높이 값을 저장한 2차원 텍스처**를 **하이트맵(Height Map)**이라고 한다.

높이 값을 **회색조(Gray-Scale)** 색상으로 해석하면 하이트맵은 회색조 이미지로 그릴 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/height2.png)   

위의 (a)는 이미지 텍스처, (b)는 이미지 텍스처로부터 반자동으로 생성된 하이트맵, 그리고 (c)는 노멀맵을 나타낸다. 이미지 텍스처가 주어지면, 각 픽셀의 RGB 값을 회색조로 바꾸고 필요에 따라 이를 수작업으로 편집하여 하이트맵을 얻을 수 있다. 이렇게 이미지 텍스처를 이용해 생성된 하이트맵은 원본 텍스처와 **동일한 해상도**를 가진다.

> 💡 이미지 텍스처에서 각 픽셀의 RGB 값을 회색조로 바꾸는 가장 간단한 방법은 **R, G, B 세 개 값의 평균**을 택하는 것이다.

하이트맵은 **지형(Terrain)**을 표현하는 대표적인 기법이며, 물과 같은 유체의 표면을 표현하는 데에도 많이 사용된다.

***

## 👻 노멀맵
노멀맵을 만드는 방법 중 하나는 하이트맵을 사용하는 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/normal1.png)   

위의 그림은 텍셀 9개를 이용해 사각형 메시를 만들고 높이 값을 저장한 하이트맵이다. 이 메시 표면의 가운데 점 (x, y, h(x, y))의 노멀을 계산해보자. 여기에서 h(x, y)는 하이트 필드 함수를 의미한다.

우선, (x - 1, y, h(x - 1, y))와 (x + 1, y, h(x + 1, y))를 잇는 벡터를 (2, 0, δh<sub>x</sub>)로 표기하자. 여기에서 δh<sub>x</sub>는 h(x + 1, y) - h(x - 1, y)이다. 마찬가지로, y축과 평행하고 (x, y, h(x, y))를 지나는 벡터를 구하면 (0, 2, δh<sub>y</sub>)로 표기할 수 있다. 이 두 벡터는 (x, y, h(x, y))에서의 **탄젠트 벡터**로 생각할 수 있는데, 이 둘의 벡터곱을 해당 점의 노멀로 취하면 된다. 이를 정규화하여 노멀맵에 저장하는데, 모든 정점에 대해 이 연산을 수행하면 노멀맵은 다음과 같이 완성된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/normal2.png)   

노멀맵을 계산하는 보다 복잡한 알고리즘의 경우, 하이트맵의 한 점에서 노멀을 계산하기 위해 **네 개 이상의 이웃 점들을 사용**하기도 한다.

> 💡 포토샵을 비롯한 2차원 그래픽스 패키지들은 하이트맵에서 자동으로 노멀맵을 생성하는 플러그인을 제공한다.

정규화된 노멀 (n<sub>x</sub>, n<sub>y</sub>, n<sub>z</sub>)의 각 좌표는 모두 **[-1, 1]** 범위의 실수값이다. 반면, 이런 노멀을 저장할 텍스처의 RGB 채널은 모두 **[0, 1]** 범위에 있기 때문에 아래와 같은 **범위 전환**이 필요하다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/normal3.png)   

노멀맵은 잘 생각해보면 (0, 0, 1)을 이쪽저쪽으로 조금씩 **'흔들어 놓은'** 노멀들의 집합으로 이해할 수 있다. 따라서 n<sub>z</sub>가 n<sub>x</sub>와 n<sub>y</sub>에 비해 상대적으로 **클 것**이고, 이는 결국 노멀맵을 이미지 텍스처로 취급하여 그리게 되면 **전체적으로 파란 색조**를 띤다는 것을 알 수 있다. (_[여기 두 번째 그림의 (c) 참고!](/computer-graphics/normal-mapping/#-하이트맵)_)

***

## 👻 노멀 매핑 쉐이더
노멀 매핑에 입력으로 제공되는 저해상도 메시를 **기초 표면(Base Surface)**이라고 부르자(공식적으로는 **매크로구조(Macrostructure)**라고 부른다). 이 기초 표면은 래스터라이저에 의해 프래그먼트로 분해되고, 각 프래그먼트의 라이팅을 담당하는 프래그먼트 쉐이더는 **노멀맵으로부터 노멀을 얻게 된다.**

다음은 **노멀 매핑을 위한 정점 쉐이더 코드**이다.

```ini
#version 300 es

uniform mat4 worldMat, viewMat, projMat;
uniform vec3 eyePos;

layout(location = 0) in vec3 position;
layout(location = 2) in vec2 texCoord;

out vec3 v_view;
out vec2 v_texCoord;

void main() {
    vec3 worldPos = (worldMat * vec4(position, 1.0)).xyz;
    v_view = normalize(eyePos - worldPos);
    v_texCoord = texCoord;
    gl_Position = projMat * viewMat * vec3(worldPos, 1.0);
}
```

이전의 정점 쉐이더와 달리 **정점 노멀 입력이 없음**에 주목해야한다. 반면, 이전과 마찬가지로 texCoord는 v_texCoord에 그대로 복사된다. 이는 래스터라이저에서 보간되어 프래그먼트 쉐이더로 넘어가게 되고, 보간된 텍스처 좌표와 노멀맵을 이용하여 노멀을 구하게 된다.

다음은 **노멀 매핑을 위한 프래그먼트 쉐이더 코드**이다.

```ini
#version 300 es

precision mediump float;

uniform sampler2D colorMap, normalMap;
uniform vec3 matSpec;   // Ms
uniform float matSh;    // shininess
uniform vec3 srcDiff, srcSpec, srcAmbi; // Sd, Ss, Sa
uniform vec3 lightDir;  // directional light

in vec3 v_view;
in vec2 v_texCoord;

layout(location = 0) out vec4 fragColor;

void main() {
    // normal map fil;tering
    vec3 normal = normalize(2.0 * texture(normalMap, v_texCoord).xyz - 1.0);

    vec3 light = normalize(lightDir);
    vec3 view = normalize(v_view);

    // diffuse term
    vec3 matDiff = texture(colorMap, v_texCoord).rgb;
    vec3 diff = max(dot(normal, light), 0.0) * srcDiff * matDiff;

    // specular term
    vec3 refl = 2.0 * normal * dot(normal, light) - light;
    vec3 spec = pow(max(dot(refl, view), 0.0), matSh) * srcSpec * matSpec;

    fragColor = vec4(diff + spec, 1.0);
}
```

퐁 라이팅이 구현된 프래그먼트 쉐이더 코드이다. 여기서 텍스처 좌표와 노멀맵을 이용하여 노멀을 구하는 것을 알 수 있다. 여기서 내장 함수 ``` texture ```는 RGB 색상을 출력하기 때문에 이를 다시 [-1, 1] 범위로 전환해야한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/normal4.png)   

이미지 텍스처링만 적용했을 때와 노멀 매핑까지 적용했을 때의 결과를 비교해보면 아래와 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/normal5.png)   

그런데, 노멀 매핑은 기초 표면 자체를 오돌토돌하게 바꾸는 게 아니라 노멀을 사용하여 오돌토돌한 표면을 **흉내**내는 것이다. 그래서 메시 테두리의 경우 고해상도 메시 자체를 적용했을 때와 달리 일직선임을 알 수 있는데, 이 같은 한계는 피할 수 없다.

***

## 👻 탄젠트 공간 노멀 매핑
텍스처링은 물체 표면에 벽지 바르듯이 텍스처를 입히는 것으로 생각할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/tan1.png)   

위의 그림에서 왼쪽은 노멀맵에 저장된 노멀을 옆에서 본 것이고 오른쪽은 이러한 노멀맵을 물체 표면에 벽지 바르듯이 입힌 결과이다. 이와 같은 노멀 매핑을 위해서는 이미지 텍스처링에서는 불필요했던 한 가지 작업을 더 수행해야 한다.

***

### 🌱 탄젠트 공간 노멀
물체 표면의 한 점에서의 노멀을 N이라고 할 때, N에 수직인 **접면(Tangent Plane)**에는 두 개의 서로 **수직**인 단위 벡터 T와 B를 정의할 수 있다(T는 Tangent를, B는 BiTangent를 의미한다).

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/tan2.png)   

위의 그림에서, 점들에 수직인 노멀 N과 접면 즉, 탄젠트 평면에 위치하고 서로 수직인 T와 B를 보여준다. 이렇게 한 점에서의 {T, B, N}은 해당 점과 함께 **탄젠트 공간(Tangent Space)**을 구성한다. 표면의 각 점은 자신만의 탄젠트 공간을 가지므로 각 점의 탄젠트 공간은 서로 다른 공간이다.

노멀맵에서 가져온 노멀은 탄젠트 공간에 속한 것이 아니지만, 이 노멀도 탄젠트 공간의 N을 **'흔들어 놓은'** 것으로 해석할 수 있다. 고로, 물체 표면의 어떤 점에 노멀 매핑을 적용하건 노멀맵에서 읽어온 노멀은 바로 그 점의 탄젠트 공간에서 정의된 벡터로 볼 수 있다. 이런 관점에서 **탄젠트 공간 노멀맵**이라는 용어를 사용한다.

***

### 🌱 탄젠트 공간 노멀 매핑 쉐이더
이전에 본 프래그먼트 쉐이더 코드에서 **노멀과 빛 벡터를 내적 연산**하는 부분이 존재했다(``` dot(normal, light) ```). 하지만 이는 틀린 코드이다. 다만, 이전에 예시로 든 기초 표면이 기울어지지 않고 평평한 표면이었기 때문에 우연히 성공한 것이다. 만약 이 기초 표면이 기울어진다면, 탄젠트 공간과 월드 공간의 **기저는 달라지고** 내적 연산은 예기치 않은 렌더링 결과를 산출할 것이다.

여기서 normal은 탄젠트 공간 벡터인 반면 light는 월드 공간 벡터이다. 서로 다른 공간에서 정의된 벡터들에 대해 내적 연산을 수행할 수는 없다. 이를 해결하기 위해 normal을 월드 공간으로 변환하거나 light를 탄젠트 공간으로 변환해야 한다. 후자를 택하게 되면, light를 탄젠트 공간으로 변환하는 행렬은 **각 프래그먼트마다 새로 계산**되어야 한다.

각 정점은 {T, B, N}을 가지고, 이 기저는 전처리 단계에서 정점별로 계산되고 **정점 배열에 저장되어 정점 쉐이더로 넘겨진다.** T, B, N은 **오브젝트 공간에서 정의**된 벡터들이다. 정점 쉐이더는 여기에 먼저 **월드 변환**을 적용하여 이들을 월드 공간으로 옮긴다. 그 다음, 월드 공간 벡터 T, B, N을 행으로 가지는 3 × 3 회전 행렬을 구성한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/tan3.png)   

이는 바로 월드 공간 벡터를 탄젠트 공간으로 변환하는 **기저 이전 행렬**이다.

다음은 **탄젠트 공간 노멀 매핑을 위한 정점 쉐이더 코드**이다.

```ini
#version 300 es

uniform mat4 worldMat, viewMat, projMat;
uniform vec3 eyePos, lightDir;

layout(location = 0) in vec3 position;
layout(location = 1) in vec3 normal;
layout(location = 2) in vec2 texCoord;
layout(location = 3) in vec3 tangent;

out vec3 v_lightTS, v_viewTS;
out vec2 v_texCoord;

void main() {
    vec3 worldPos = (worldMat * vec4(position, 1.0)).xyz;

    vec3 Nor = normalize(transpose(inverse(mat3(worldMat))) * normal);
    vec3 Tan = normalize(transpose(inverse(mat3(worldMat))) * tangent);
    vec3 Bit = cross(Nor, Tan);
    mat3 tbnMat = transpose(mat3(Tan, Bit, Nor));   // row major

    v_lightTS = tbnMat * normalize(lightDir);
    v_viewTS = tbnMat * normalize(eyePos - worldPos);

    v_texCoord = texCoord;
    gl_Position = projMat * viewMat * vec4(worldPos, 1.0);
}
```

여기서 입력값 normal은 N, tangent는 T를 의미하며 B는 이 둘의 벡터곱으로 구한다. 벡터곱은 내장 함수인 ``` cross ```로 구현되며, 이는 또 다른 애트리뷰트로 제공할 수 있지만 정점 배열이 커지는 단점이 있기 때문에 직접 제공하진 않았다(선택은 자유이다).

tbnMat의 경우 GL 특성상 **열 우선(Column Major)**으로 행렬이 만들어지기 때문에 이를 행 우선으로 변경하기 위해 transpose를 취해주었다.

한편, 라이팅 계산에는 노멀과 빛 벡터 말고도 **시선 벡터**가 필요한데, 이 역시 탄젠트 공간으로 변환된다.

마지막으로 입력된 normal은 출력되지 않는다. 프래그먼트 쉐이더에서는 노멀맵을 사용하여 노멀값을 구할 수 있기 때문이다.

다음은 **탄젠트 공간 노멀 매핑을 위한 프래그먼트 쉐이더 코드**이다.

```ini
#version 300 es

precision mediump float;

uniform sampler2D colorMap, normalMap;
uniform vec3 matSpec;   // Ms
uniform float matSh;    // shininess
uniform vec3 srcDiff, srcSpec, srcAmbi; // Sd, Ss, Sa

in vec3 v_lightTS, v_viewTS;
in vec2 v_texCoord;

layout(location = 0) out vec4 fragColor;

void main() {
    vec3 normal = normalize(2.0 * texture(normalMap, v_texCoord).xyz - 1.0);

    vec3 light = normalize(v_lightTS);
    vec3 view = normalize(v_viewTS);

    vec3 matDiff = texture(colorMap, v_texCoord).rgb;
    vec3 diff = max(dot(normal, light), 0.0) * srcDiff * matDiff;

    vec3 refl = 2.0 * normal * dot(normal, light) - light;
    vec3 spec = pow(max(dot(refl, view), 0.0), matSh) * srcSpec * matSpec;

    fragColor = vec4(diff + spec, 1.0);
}
```

아까완 달리 새 변수로 v_lightTS와 v_viewTS를 받아들여 정규화한다. 그 결과 각각 탄젠트 공간 벡터가 만들어진다. 이제 앞서 만든 모든 벡터들로 라이팅 계산이 가능하다.

- **적용 결과**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/tan4.png)   

***

## 🌱 탄젠트 공간 계산
정점 노멀 N이 주어졌을 때, T와 B를 계산하는 방법에 대해 알아보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/tan-cal.jpg)   

위 그림의 (b)를 보면 이미지 텍스처의 파라미터 공간에서 (s<sub>i</sub>, t<sub>i</sub>)를 보여준다. 이 공간의 s축은 노멀맵의 T축과 일치하며 마찬가지로 t축은 노멀맵의 B축과 일치한다. 세 정점에 할당된 텍스처 좌표 (s<sub>i</sub>, t<sub>i</sub>)를 분석해서 T와 B축의 방향을 계산할 수 있다.

그림 (c)는 삼각형 ＜p<sub>0</sub>, p<sub>1</sub>, p<sub>2</sub>＞와 함께 T와 B축을 보여주는데, 여기에서 T와 B축은 아직 결정되지 않은 상태이다. p<sub>0</sub>와 p<sub>1</sub>을 연결하는 벡터를 q<sub>1</sub>, p<sub>0</sub>와 p<sub>2</sub>를 연결하는 벡터를 q<sub>2</sub>로 표기하면, q<sub>1</sub>과 q<sub>2</sub>는 다음과 같이 정의된다.

> q<sub>1</sub> = (s<sub>1</sub> - s<sub>0</sub>)T + (t<sub>1</sub> - t<sub>0</sub>)B   
q<sub>2</sub> = (s<sub>2</sub> - s<sub>0</sub>)T + (t<sub>2</sub> - t<sub>0</sub>)B

여기서 q<sub>1</sub>, q<sub>2</sub>, T, B는 모두 3차원 벡트이므로 총 여섯 개의 미지수와 여섯 개의 방정식을 가지고 있다. 따라서 T와 B를 계산할 수 있다.

위의 예시에서는 정점을 공유하는 하나의 삼각형에 대해서만 계산하였다. 이것 말고도 여러 개의 삼각형이 해당 정점을 공유하므로, 각 삼각형마다 T와 B를 따로 계산해야 한다. 이렇게 해서 얻은 모든 T들의 합을 T'로, 모든 B들의 합을 B'로 표기하면, 이 둘은 단위 벡터가 아닐뿐더러 서로 수직이 아니게 된다. 또한 이들은 N과도 수직이 아닐 것이다. 이러한 {T', B', N}에 **[그람-슈미트(Gram-Schmidt) 알고리즘](https://ko.wikipedia.org/wiki/%EA%B7%B8%EB%9E%8C-%EC%8A%88%EB%AF%B8%ED%8A%B8_%EA%B3%BC%EC%A0%95)**을 적용하면 **직교정규 기저**를 얻을 수 있고, 이것이 바로 해당 정점에서의 **탄젠트 공간의 기저**가 된다.

***

## 👻 노멀맵 제작
평면이나 곡률이 낮은 곡면에 입혀질 노멀맵은 2차원 그래픽스 패키지를 사용해 만들 수 있지만 복잡한 곡면을 위한 노멀맵은 이것으로 제작하기 어렵다. 이 부분은 Pixologic사가 개발한 **지브러쉬(ZBrush)**와 같은 **조각 도구(Sculpting Tool)**를 사용한다. 지브러쉬를 사용하면 폴리곤 메시의 전체 혹은 일부를 쉽게 변형시킬 수 있다. 아래는 지브러쉬를 사용하여 저해상도 모델을 오돌토돌한 표면을 가진 고해상도 모델로 바꾸는 과정을 보여준다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/map1.png)   

앞서 보았던 저해상도 모델은 **기초 표면**, 고해상도 모델은 **참조 표면(Reference Surface)**라 부른다. 전처리 과정에서 지브러쉬는 이 두 면을 사용하여 자동으로 노멀맵을 만들고, 런타임에서는 노멀 매핑을 통해 이 노멀맵이 기초 표면에 입혀질 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/map2.png)   

지브러쉬는 우선 기초 표면을 **파라미터화(Parameterization)**한다. 이 결과, 기초 표면의 모든 정점은 정규화된 텍스처 좌표를 가지며 노멀맵의 해상도를 곱해 새로운 좌표를 정의한다. 그런 다음, 기초 표면의 각 삼각형은 **스캔 전환**되어 다수의 텍셀을 포함하게 된다. 각 텍셀에 노멀을 할당하면 노멀맵이 완성된다.

그림 (c)는 중첩된 상태의 기초 표면과 참조 표면의 단면을 보여준다(기초 표면과 참조 표면을 의도적으로 분리해서 그렸지만 실제로는 서로 겹쳐져 있을 것이다). 텍셀 T에 임시로 할당된 노멀을 n<sub>T</sub>로 표기할 때, 노멀 n<sub>T</sub> 방향으로 p에서 **광선**을 발사해보자. 이는 참조 표면의 삼각형과 교차하는데, 이 교차점이 참조 표면 삼각형에 대해 가지는 **무게중심 좌표**를 계산한다. 이를 이용하여 참조 표면 삼각형의 정점 노멀을 이용하여 **보간**한다(d). 보간된 노멀 n이 바로 참조 표면 고차점에서의 표면 노멀로 채택되어 텍셀 T에 저장될 것이다. 이는 나중에 텍스처 좌표 (s, t)를 이용해 읽혀질 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/normal-mapping/map3.png)   

이제까지 기술한 알고리즘은 **오브젝트 공간에서 노멀을 계산했음**을 이해해야한다. 이러한 노멀을 저장한 텍스처를 **오브젝트 공간 노멀맵**이라 부른다. 디는 그림 (e)에 RGB 색상으로 가시화되었다. 참조 표면의 표면 노멀은 대개의 경우 온갖 방향으로 다양하게 분포되어 있을 것이므로 **오브젝트 공간 노멀맵은 특정 색상이 지배하지 않는다.**

이제 오브젝트 공간의 노멀을 **탄젠트 공간**으로 변환해야 한다. 앞서 보았던 방식으로 변환하여 만들어진 **탄젠트 공간 노멀맵**은 그림 (f)에 가시화되었다. 대부분의 탄젠트 공간 노멀맵과 마찬가지로 **파란색 계열이 지배적인 색상**이 된다. 그림 (g)는 노멀 매핑을 적용한 모델과 하지 않은 모델을 비교한다.

***

## 👻 글을 마치며
이번 시간에는 노멀 매핑에 대해 알아보았다. 처음엔 쉬운 것 같아서 이해가 잘 됐는데 듣다 보니 점점 어려워지고 혼란스러워졌다. 그래도 엔진을 사용하고 텍스처를 사용하면서 보았던 것들이 이제 어떤 내용을 담고있는지 확실히 알 수 있었고 이렇게 강의 내용을 정리하면서 한 번 더 복습하니 개념 정리가 착착 되는 것 같다. 소모임에서도 잠깐 다뤘던 내용이기도 하고.. 뭔가 머릿속에서 둥둥 떠다녔던 개념들이 하나로 뭉쳐지는 기분이 든다! ~~그래도 머리 터질 것 같다😭~~

***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (14장-노멀 매핑)](https://youtu.be/gCcJ6GzZpWY)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   