---
title: "[Computer Graphics] #15. 쉐도우 매핑"
excerpt: "한정현 컴퓨터 그래픽스 강의 15장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, shadow mapping, shadow, map]

permalink: /computer-graphics/shadow-mapping/

toc: true
toc_sticky: true

date: 2023-05-01 18:12:58+0900
last_modified_at: 2023-05-01 18:13:02+0900
published: true
---

## 👻 쉐도우 매핑
렌더링된 영상의 시각적 사실감을 높이는 데 있어 그림자는 필수불가결한 항목이다. 또한 그림자는 물체들 사이의 공간적 관계를 이해하는 데도 도움을 준다. 이러한 그림자를 만드는 방법에 대해 알아보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/shadow.png)   

***

## 👻 두 단계 렌더링
쉐도우 매핑 알고리즘은 두 번의 **렌더링 패스(Rendering Pass)**를 통해 수행된다. 첫 번째 패스에서는 **쉐도우맵(Shadow Map)**이라는 특수한 텍스처를 생성한다. 두 번째 패스에서는 실제 렌더링을 수행하게 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/rendering1.png)   

위 이미지에서 광원에서 나온 빛이 미치는 표면은 굵은 선으로 표시되었다. 이들 표면을 균일하게 샘플하여, 각 샘플점 p마다 광원까지의 거리(z)를 쉐도우맵에 저장한다. 이는 광원에서 본 3차원 장면의 **깊이**값이다. 따라서, 쉐도우맵은 **광원 기준의 깊이맵(Depth Map)**이라고도 한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/rendering2.png)   

위 이미지는 두 번째 패스를 의미한다. 이 과정에서 쉐도우맵을 사용하여 그림자를 생성한다. 프래그먼트 f<sub>1</sub>, f<sub>2</sub>에 해당하는 월드 공간의 점을 각각 q<sub>1</sub>, q<sub>2</sub>라고 할 때, 광원과 q<sub>1</sub> 사이의 거리 d<sub>1</sub>는 쉐도우맵에 저장된 깊이값 z<sub>1</sub>보다 크다. 이는 광원과 q 사이에 어떤 점이 놓여있어서 빛을 가리고 있다는 것을 의미한다. 따라서 q<sub>1</sub>은 그림자에 속하는 점으로 판정된다. 반면, q<sub>2</sub>의 경우 d<sub>2</sub>와 z<sub>2</sub>가 같으므로 광원에서 보이는 점이 된다. 이는 빛을 받는 점으로 판정된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/sampling1.png)   

이렇게 쉐도우 매핑 알고리즘은 개념적으로 매우 간단하지만 이를 그대로 구현할 경우 위 오른쪽 이미지처럼 완전하게 빛을 받는 지역에 자잘한 그림자가 섞여 있는 것 같은 문제를 포함하여 여러 문제와 마주치게 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/sampling2.png)   

쉐도우맵은 텍스처의 한 종류이므로 이를 어떻게 필터링할 것인지 미리 정해야 한다. **근접점 샘플링(Nearest Point Sampling)**을 사용한다고 가정하자. 쉐도우 매핑이 수행되면서 샘플된 프래그먼트 점과 그 근처에 위치한 라이팅이 비교될 것이다. 이 결과는 정확한 결과를 보장하지 않아 자잘한 그림자가 발생하는 문제가 생긴다.

이 문제를 해결하는 방법은 간단하다. 두 번째 패스에서 샘플한 점들을 **광원 쪽으로 약간 이동**하면 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/sampling3.png)   

즉, 광원까지의 거리 d에서 일정한 값을 빼는 것이다. 이 값을 **바이어스(Bias)**라 한다. 위 이미지에서 보면 d<sub>1</sub>에서 바이어스를 뺀 d<sub>1</sub>'를 만든 후, 이를 z<sub>1</sub>과 비교한다. 이는 d<sub>1</sub>'가 z<sub>1</sub>보다 작으므로 샘플된 점은 빛을 받는 것으로 판정된다. 이러한 방법으로 쉐도우 매핑을 개선할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/sampling4.PNG)   

쉐도우 매핑에서는 적절한 바이어스를 설정하는 것이 중요한데, 바이어스가 너무 작을 경우 불필요한 그림자를 완벽히 제거할 수 없고 바이어스가 너무 크면 그림자가 과도하게 축소될 수 있다. 바이어스는 대개 몇 번의 시험을 거쳐 적절한 값으로 설정된다.

***

## 👻 쉐도우맵 필터링
앞서 보았던 근접점 샘플링을 이용하면 하나의 프래그먼트는 완전히 빛을 받거나 완전히 그늘지거나 둘 중 하나로 판정될 뿐, 다른 여지는 없다. 그 결과 이미지 텍스처링에서 보았던 모자이크화되는 문제가 발생할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/filtering1.png)   

이를 보완하기 위해 **겹선형 보간**을 사용하여 쉐도우맵을 필터링해 보자.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/filtering2.png)   

위 왼쪽 이미지가 겹선형 보간을 이용하여 쉐도우맵을 필터링한 결과이다. 쉐도우맵에 투영된 프래그먼트 q를 둘러싼 네 개 텍셀의 깊이 값을 보간하여 q의 깊이값과 비교하게 된다. 이 결과로 q의 깊이가 더 크다면 그늘진 것으로 판정된다. 이 역시 완전히 빛을 받거나 완전히 그늘지거나 둘 중 하나로 판정되는 문제는 여전하다.

이 문제를 해결하는 방법은 네 개의 텍셀 각각에 대해 q의 **가시성(Visibility)**을 결정한 후 이를 보간하는 것이다. 위 오른쪽 이미지가 이에 해당하는데, 왼쪽 위의 텍셀만 고려하면 q에는 그림자가 맺히는 것으로 판정될 것이다(q의 깊이값이 더 크기 때문이다). 즉, 이 텍셀에 대한 q의 가시성은 0이다. 반면, 나머지 세 개의 텍셀에 대해서는 q는 빛을 받는 것으로 판정되기 때문에 가시성은 1이 될 것이다. 이 네 개의 가시성 값을 겹선형 보간하면 q의 가시성은 0.58로 계산된다. 이 값은 q가 **빛을 받는 정도**를 나타내며 완전히 빛을 받거나 완전히 그늘지거나 둘 중 하나를 택하는 대신 [0, 1] 범위 안의 값을 취하게 되며 0에 가까우면 어둡게, 1에 가까우면 밝게 처리된다. 이 방법을 통해 그림자의 윤곽선을 조금 더 부드럽게 만들 수 있을 것이다.

> 💡 이렇게 가시성을 이용하여 겹선형 보간하는 기법을 **PCF(Percentage Closer Filtering)**라고 부른다.

***

## 👻 쉐도우 매핑을 위한 GL 프로그램과 쉐이더
쉐도우 매핑의 첫 번째 렌더링 패스에서는 쉐도우맵을 생성한다고 했었다. 이를 위해서 카메라를 광원의 위치에 놓고 3차원 장면을 렌더링하여 최종적으로 얻은 z-버퍼를 쉐도우맵으로 취한다. 이렇게 만들어진 쉐도우맵을 이용하여 두 번째 렌더링 패스에서는 실제 카메라 관점에서 통상적인 렌더링을 수행한다.

***

### 🌱 첫 번째 패스의 쉐이더
첫 번째 패스에서는, 모든 3차원 정점이 월드 변환을 통해 월드 공간으로 변환된 후 **카메라 파라미터**가 광원 기준으로 설정된다. 이를 **빛 공간(Light Space)**이라 부른다.

> ![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/shader1.jpg)   
광원 기준으로 설정된 **EYE**, **AT**, **UP**을 이용해 {u, v, n}이 계산되며 빛 공간이 정의된다. 빛 공간의 점은 클립 공간으로 투영 변환되고, 원근 나눗셈에 의해 NDC로 표현된다. 마지막으로 뷰포트 변환에 의해 [0, 1] 범위의 z가 결정된다.

다음은 **쉐도우 매핑을 위한 첫 번째 패스의 정점 쉐이더**이다.

```ini
#version 300 es

uniform mat4 worldMat;
uniform mat4 lightViewMat, lightProjMat;

layout(location = 0) in vec3 position;

void main() {
  // generate shadow map
  gl_Position = lightProjMat * lightViewMat * worldMat * vec4(position, 1.0);
}
```

위 코드에서 새로운 유니폼으로 ``` lightViewMat ```와 ``` lightProjMat ```를 받는 것을 제외하면 일반적인 정점 처리 과정과 동일하다.

> 💡 쉐도우 매핑을 위한 **첫 번째 패스의 프래그먼트 쉐이더**는 아무런 작업을 하지 않는다. 쉐도우 매핑은 첫 번째 패스에서 프래그먼트의 색상을 결정하지 않기 때문이다.

각 프래그먼트의 스크린 공간 좌표는 출력 병합기 단계에서 z-버퍼링을 통해 깊이 값이 저장된 쉐도우맵을 생성하게 된다. 이들 깊이값은 [0, 1]의 범위를 가진다.

***

### 🌱 프레임버퍼 오브젝트
쉐도우 매핑의 첫 번째 패스에서, 렌더링은 실제 스크린 대신 텍스처에 이뤄진다. 이렇게 텍스처에 렌더링(Render-to-Texture)하기 위해서 GL 프로그램은 먼저 쉐도우맵으로 사용할 텍스처를 확보해야 한다. 또한, 텍스처에 렌더링하기 위해서는 프레임 버퍼 대신 **프레임버퍼 오브젝트(Framebuffer Object; 이하 FBO)**를 사용해야 한다.

다음은 프레임버퍼 오브젝트를 위한 GL 프로그램이다.

```ini
#define SHADOW_MAP_SIZE 1024

GLuint texture;
glGenTextures(1, &texture);
glBindTexture(GL_TEXTURE_2D, texture);
glTexImage2D(GL_TEXTURE_2D, 0, GL_DEPTH_COMPONENT16,
    SHADOW_MAP_SIZE, SHADOW_MAP_SIZE, 0, 
    GL_DEPTH_COMPONENT, GL_UNSIGNED_SHORT, NULL);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_CLAMP_TO_EDGE);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_CLAMP_TO_EDGE);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_COMPARE_MODE, GL_COMPARE_REF_TO_TEXTURE);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_COMPARE_FUNC, GL_LEQUAL);

GLuint fbo;
glGenFramebuffers(1, &fbo);
glBindFramebuffer(GL_FRAMEBUFFER, fbo);
glFramebufferTexture2D(GL_FRAMEBUFFER, GL_DEPTH_ATTACHMENT, GL_TEXTURE_2D, texture, 0);
```

> - ```GL_DEPTH_COMPONENT16 ``` : 쉐도우맵의 깊이 값이 16비트로 저장
- ``` GL_DEPTH_ATTACHMENT ``` : 쉐도우맵을 위해 확보한 텍스처 오브젝트(``` texture ```)를 FBO의 깊이 요소에 붙임

***

### 🌱 두 번째 패스의 쉐이더
![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/shader2.jpg)   

두 번째 패스에서는 실제 카메라 관점에서 통상적인 렌더링을 수행한다. 텍스처 좌표를 이용해 NDC 내의 샘플점 좌표를 구할 수 있다. 단, 텍스처 좌표 (s, t)는 [0, 1] 범위에 있고 샘플점 좌표는 [-1, 1] 범위에 있기 때문에 범위 전환이 필요하다. 이렇게 상대적인 위치를 구하게 되면 광원으로부터의 거리 d를 구할 수 있다. 쉐도우맵에 저장된 z값과 광원으로부터의 거리 d값을 비교하여 해당 부분이 빛을 받는지, 그림자가 지는지를 정의할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/shader3.jpg)   

위 식에서 화살표는 원근 나눗셈을 의미한다.

다음은 **쉐도우 매핑을 위한 두 번째 패스의 정점 쉐이더**이다.

```ini
#version 300 es

uniform mat4 worldMat, viewMat, projMat;
uniform mat4 lightViewMat, lightProjMat;
uniform vec3 lightPos;

layout(location = 0) in vec3 position;
layout(location = 1) in vec3 normal;
layout(location = 2) in vec2 texCoord;

out vec3 v_normal, v_light;
out vec2 v_texCoord;
out vec4 v_shadowCoord;

const mat4 tMat = mat4(
  0.5, 0.0, 0.0, 0.0,
  0.0, 0.5, 0.0, 0.0,
  0.0, 0.0, 0.5, 0.0,
  0.5, 0.5, 0.5, 1.0
);

void main() {
  v_normal = normalize(transpose(inverse(mat3(worldMat))) * normal);
  vec3 worldPos = (worldMat * vec4(position, 1.0)).xyz;
  v_light = normalize(lightPos - worldPos);
  v_texCoord = texCoord;

  // for shadow map access and depth comparison
  v_shadowCoord = tMat * lightProjMat * lightViewMat * vec4(worldPos, 1.0);
  gl_Position = projMat * viewMat * vec4(worldPos, 1.0);
}
```

쉐도우맵을 위한 좌표를 생성하는 것 외엔 통상적인 렌더링 과정을 거친다.

다음은 **쉐도우 매핑을 위한 두 번째 패스의 프래그먼트 쉐이더**이다.

```ini
#version 300 es

precision mediump float;
precision mediump sampler2DShadow;

uniform sampler2D colorMap;
uniform sampler2DShadow shadowMap;
uniform vec3 srcDiff;

in vec3 v_normal, v_light;
in vec2 v_texCoord;
in vec4 v_shadowCoord;

layout(location = 0) out vec4 fragColor;

void main() {
  vec3 normal = normalize(v_normal);
  vec3 light = normalize(v_light);

  // diffuse term
  vec3 matDiff = texture(colorMap, v_texCoord).rgb;
  vec3 diff = max(dot(normal, light), 0.0) * srcDiff * matDiff;

  float visibility = textureProj(shadowMap, v_shadowCoord);

  fragColor = vec4(visibility * diff, 1.0);
}
```

쉐도우맵은 일반 텍스처와 구분하기 위하여 ``` sampler2DShadow ``` 타입으로 지정되었다. 또한, 가시성을 구하기 위해 내장 함수 ``` textureProj ```를 사용하는데 이는 투영 방식으로 텍스처를 처리하는 함수이다.

여기에 **바이어스**를 추가한 코드는 아래와 같다.

```ini
... (생략) ...

layout(location = 0) out vec4 fragColor;

const float offset = 0.005;

void main() {
  ... (생략) ...

  // visibility + bias
  vec4 offsetVec = vec4(0.0, 0.0, offset * v_shadowCoord.w, 0.0);
  float visibility = textureProj(shadowMap, v_shadowCoord - offsetVec);

  fragColor = vec4(visibility * diff, 1.0);
}
```

***

## 👻 하드 쉐도우와 소프트 쉐도우
![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/hard1.png)   

위 왼쪽 이미지를 보면, **점 광원(Point Light)**에서 나오는 빛을 받는 지역과 받지 않는 지역이 명확하게 구분되는데, 이렇게 만들어진 그림자를 **하드 쉐도우(Hard Shadow)**라 부른다. 반면, 점 광원과 달리 '면적'을 가진 광원을 **영역 광원(Area Light)**이라고 부르는데, 이는 **소프트 쉐도우(Soft Shadow)**를 생성한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/shadow-mapping/soft1.png)   

위 이미지는 소프트 쉐도우를 보여주는데, 그림-(a)에서 완전하게 빛을 받는 지역과 완전하게 그늘진 지역 사이에 '부분적으로 그늘진' 지역이 있음을 알 수 있다. 이는 반(半)그림자, 영문으로는 penumbra라고 부르는데, 여기에 속하는 표면의 각 점은 광원에서 나오는 빛의 일부만을 받는다. 따라서 완전하게 빛을 받는 지역과 완전하게 그늘진 지역을 부드럽게 이어주는 역할을 한다.

반그림자 지역에 속하는 표면 점에 대해서는 '밝기 정도'를 계산해야 하는데, 이는 보통 그 점에서 영역 광원이 얼마나 많이 보이는지 측정하여 결정한다. 그림-(b)와 그림-(c)를 비교하면, 상대적으로 가시 영역이 더 작은 (b)의 경우 (c)보다 어둡게 처리된다.

> 💡 PCF가 적용된 그림자는 소프트 쉐도우가 아닌 **안티앨리어싱 된 하드 쉐도우**이다.

*** 

## 👻 글을 마치며
이번 시간에는 쉐도우 매핑에 대해 알아보았다. 그래픽스를 자세히 공부하면서 다양한 매핑 기법이 있다는 것이 신기하고 재미있는 것 같다. 평소에 그림자는 어떻게 구현하는건지 궁금했는데 이것 또한 수학적인 방법을 이용해서 만들게 된다는 것을 알았고 수학 기초를 탄탄히 다져야겠다는 생각을 다시 한 번 더 하게 되는 것 같다😭

***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (15장-쉐도우 매핑)](https://youtu.be/kCuEtQh91U8)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   