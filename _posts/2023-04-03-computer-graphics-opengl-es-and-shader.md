---
title: "[Computer Graphics] #6. OpenGL ES와 쉐이더"
excerpt: "한정현 컴퓨터 그래픽스 강의 6장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, opengl, opengl es, es, embeded system, shader]

permalink: /computer-graphics/opengl-es-and-shader/

toc: true
toc_sticky: true

date: 2023-04-03 21:33:32+0900
last_modified_at: 2023-04-03 21:33:36+0900
published: true
---

## 👻 OpenGL ES와 쉐이더
GPU 파이프라인에 입력되는 정점 배열은 **정점 위치와 노멀 등의 데이터를 저장**하는데, **정점 쉐이더는 한 번에 한 정점을 처리**한다. GPU는 병렬 처리 구조를 가지고 있기 때문에, 이들 코어는 **다수의 정점을 동시에 처리**한다.

**OpenGL ES**는 OpenGL의 부분집합이다. 현재 다양한 버전이 있으며 이번 공부에서는 3.0 버전을 사용할 것이다. 앞서 알아보았던 것처럼 **정점 쉐이더**와 **프래그먼트 쉐이더**를 직접 구현해줘야한다. 쉐이더는 GPU에 특화된 프로그램이며 **Shading Language**라는 다른 언어를 사용한다. OpenGL ES로 그래픽스를 구현하기 위해선 OpenGL ES가 제공하는 API와 쉐이딩 언어를 알아야한다.

쉐이딩 언어는 일반적으로 C와 비슷하다. 단, 행렬이나 벡터를 자주 사용할 것이기 때문에 이 기능을 지원해주는 특별한 타입들이 추가되어있다.

> - ``` vec4 ``` : 4차원 벡터
- ``` ivec3 ``` : 정수형 타입 밸류를 가지는 3차원 벡터
- ``` mat3 ``` : ``` 3 × 3 ``` 크기의 정사각행렬
- ``` mat4 ``` : ``` 4 × 4 ``` 크기의 정사각행렬
- ``` mat3x4 ``` : ``` 3 × 4 ``` 크기의 행렬
>   > 💡 **행렬의 모든 원소는 float 타입을 가진다.**

***

## 👻 Vertex Shader
**정점 쉐이더(Vertex Shader)**는 **두 가지 종류의 입력**을 받는다.

> - **Attributes** : 정점 배열을 구성하는 종류들(위치, 노멀, 텍스처 좌표)
- **Uniforms** : 모든 정점마다 유니폼하게 적용되는 데이터. 대표적인 예로 월드/뷰/투영 변환이 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/vertex-shader.png)   

각 정점은 **위치(Position), 노멀(Normal), 텍스처 좌표(texCoord)** 애트리뷰트를 가지고 있다. 순차적으로 증가하는 인덱스로 구분한다. 한편, 예를 들어, 정점 배열이 1000개의 정점을 저장하고 있다면, 정점 쉐이더는 1000번 실행되는데, **1000번 실행되는 정점 쉐이더가 모두 공유하는 또 다른 종류의 입력 데이터**가 있다. 대표적인 예가 **월드/뷰/투영 변환**인데, 이들은 모든 정점에 공히 적용된다. 이러한 입력 데이터를 **유니폼(Uniform)**이라 부른다.

이러한 입력들이 정점 쉐이더에 들어가게 되면, 최종적으로 **클립 공간**으로 들어가게되고 **출력값**을 내보낼 것이다. 그 중에서도 **클립 공간 위치(Clip Space Position)**은 **반드시 출력**되어야 하기 때문에 ``` gl_Position ```이라는 **내장 변수(Built-In Variable)**에 저장하여 따로 관리한다. 나머지 값들은 입력과 마찬가지로 별도의 ``` output ```으로 출력된다.

***

### 🌱 예제 코드
![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/vertex-shader-code.png)   

- 3.0 버전 OpenGL ES 사용

```ini
#version 300 es
```

- ``` 4 × 4 ``` 크기의 유니폼 행렬(월드, 뷰, 투영) 설정

```ini
uniform mat4 worldMat, viewMat, projMat;
```

- 입력값으로 넣을 정점 정보(위치, 노멀, 텍스처 좌표)

```ini
layout(location = 0) in vec3 position;
layout(location = 1) in vec3 normal;
layout(location = 2) in vec2 texCoord;
```

각각 입력값 0, 1, 2로 3차원 벡터 정점의 위치, 3차원 벡터 노멀, 2차원 벡터 텍스처 좌표이다. 텍스처 좌표는 항상 2차원 벡터 타입을 가진다.

- 출력값으로 받을 정점 정보(노멀, 텍스처 좌표)

```ini
out vec3 v_normal;
out vec2 v_texCoord;
```

클립 공간 위치는 내장 변수로 관리하기 때문에 따로 변수를 할당하지 않았다.

- 메인 함수

```ini
void main() {
  gl_Position = projMat * viewMat * worldMat * vec4(position, 1.0);
  v_normal = normalize(transpose(inverse(mat3(worldMat))) * normal);
  v_texCoord = texCoord;
}
```

텍스처 좌표는 그대로 출력 변수에 대입하고 클립 공간 위치는 모든 변환 행렬을 정점 위치와 연산을 해주었다. 카테시안 좌표는 현재 3차원의 값을 가지기 때문에 4차원 벡터로 변경해주기 위해 ``` vec4 ``` **생성자**를 사용해주었다.

정점 노멀은 변환 시 아핀 변환에서의 ``` L ``` 즉, 누적된 선형 변환 행렬의 **역전치행렬**을 사용해야 하므로 ``` inverse ```, ``` transpose ``` 연산을 차례로 수행해주었고 마지막으로 ``` normalize ```를 이용해 정규화해주었다. 해당 함수는 GL 내장 함수이다.

> 💡 위의 모든 과정이 끝나면 **총 세 개의 출력값(gl_position, v_normal, v_texCoord)이 래스터라이저에게 전달될 것**이다.

***

## 👻 쉐이더를 위한 OpenGL ES 작업
앞서 보았던 정점 쉐이더 방식은 프래그먼트 쉐이더를 설정할 때도 비슷한 방식으로 작성될 것이다. 프래그먼트 쉐이더 작업이 조금 더 복잡하다.

정점 및 프래그먼트 쉐이더가 렌더링을 위한 세부 작업을 수행한다면, GL 프로그램은 이들 **쉐이더를 관리함과 동시에 쉐이더에 필요한 데이터를 공급하는 역할**을 한다.

GL 프로그램의 **함수는** ``` gl ```로, **데이터**는 ``` GL ```로 시작한다.

***

### 🌱 쉐이더 오브젝트
![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/shader-object.png)   

위 코드는 정점 쉐이더를 관리하는 GL 프로그램의 일부이다. 맨 처음 정점 쉐이더 파일이 주어졌을 때, **쉐이더 오브젝트(Shader Object)**를 만들어야한다.

- 쉐이더 오브젝트 생성

```ini
GLuint shader = glCreateShader(GL_VERTEX_SHADER);
```

쉐이더 오브젝트는 ``` shader ```에 저장되며 타입은 앞 부분의 대문자 ``` GL ```을 제외한 ``` uint ``` 즉, ``` unsigned int ```를 의미한다. ```glCreateShader ``` 함수를 이용하여 쉐이더 소스코드를 저장할 **쉐이더 오브젝트**를 생성하는데, 정점 쉐이더는 ``` GL_VERTEX_SHADER ```, 프래그먼트 쉐이더는 ``` GL_FRAGMENT_SHADER ```를 입력 받아서 **쉐이더 오브젝트의 ID를 리턴**한다.

- 쉐이더 소스코드 저장

```ini
glShaderSource(shader, 1, &source, NULL);
```

그런 다음 GL 프로그램이 쉐이더 소스코드를 로드했고,  ``` char* source ```가 이 소스코드를 가리킨다고 가정하자. 이 소스코드는 ``` glShaderSource ```에 의해 쉐이더 오브젝트에 저장된다.

- 쉐이더 오브젝트 컴파일

```ini
glCompileShader(shader);
```

마지막으로 쉐이더 오브젝트는 ``` glCompileShader ```에 의해 **컴파일**된다.

> 💡 물론 프래그먼트 쉐이더 오브젝트도 위 단계와 동일하게 생성해주면 된다.

***

### 🌱 프로그램 오브젝트
![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/program-object.png)   

쉐이더 오브젝트가 모두 만들어졌다면, **프로그램 오브젝트(Program Object)**에 **붙여(Attach)**져야한다. 그 뒤에 프로그램 오브젝트는 **실행 파일로 링크(Link)**된다.

- 프로그램 오브젝트 생성

```ini
GLuint program = glCreateProgram();
```

쉐이더 오브젝트를 생성했을 때와 마찬가지로 ``` glCreateProgram ``` 함수를 이용하여 빈 프로그램 오브젝트를 생성해준다.

- 프로그램 오브젝트에 쉐이더 붙이기

```ini
glAttachShader(program, shader);
```

``` glAttachShader ``` 함수를 이용하여 ``` shader ```를 ID로 가지는 정점 쉐이더 오브젝트가 프로그램 오브젝트에 붙여진다.

- 프로그램 오브젝트를 실행 파일로 링크(Link)하기

```ini
glLinkProgram(program);
```

``` glLinkProgram ``` 함수를 이용하여 프로그램 오브젝트를 실행 파일로 링크시킨다.

- 프로그램 오브젝트 사용하기

```ini
glUseProgram(program);
```

프로그램 오브젝트를 **렌더링**에 사용하기 위해서 ``` glUseProgram ```이 호출되었다.

***

## 👻 애트리뷰트와 유니폼
쉐이더의 입력인 애트리뷰트와 유니폼에 관련된 코드를 분석해보자.

***

### 🌱 Attributes
![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/attributes.png)   

위 코드는 정점 및 인덱스 배열 처리를 위한 GL 프로그램이다. ``` .obj ``` 파일 등에 저장된 폴리곤 메시 데이터가 GL 프로그램의 정점 및 인덱스 배열로 로드될 수 있도록 구조체를 이용해 구현하였다.

여기서 사용된 ``` glm ``` **OpenGL 수학(Math) 라이브러리**를 의미한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/buffer-object.png)   

현재 **CPU 메모리에 저장**되어 있는 정점 및 인덱스 배열은 **렌더링**을 위해서 **GPU 메모리**의 **버퍼 오브젝트(Buffer Object)**로 옮겨지는데, 정점 배열은 **배열 버퍼 오브젝트(Array Buffer Object)**인 ``` GL_ARRAY_BUFFER ```, 인덱스 배열은 **배열 요소 버퍼 오브젝트(Element Array Buffer Object)**인 ``` GL_ELEMENT_ARRAY_BUFFER ```로 복사된다.

> 💡 ``` ELEMENT ```가 붙여진 오브젝트는 인덱스와 관련있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/attributes2.png)   

위 코드는 배열 버퍼 오브젝트를 위한 GL 프로그램이다. 정점 배열을 위한 버퍼 오브젝트를 **생성하고 채우는 과정**을 보여준다.

- 배열 버퍼 오브젝트 선언 및 생성

```ini
GLuint abo;
glGenBuffers(1, &abo);
```

``` glGenBuffers ```를 호출하여 버퍼 오브젝트를 생성한다. 여기서 ``` Gen ```은 ``` Generate ```의 줄임말이다.

- 버퍼 오브젝트를 정점 배열에 바인드(Bind)하기

```ini
glBindBuffer(GL_ARRAY_BUFFER, abo);
```

``` glBindBuffer ```에 ``` GL_ARRAY_BUFFER ```를 파라미터로 주어 호출하면 이 버퍼 오브젝트를 정점 배열에 바인드(Bind)하게 된다.

- 버퍼 오브젝트 채우기

```ini
glBufferData(GL_ARRAY_BUFFER,
  (GLsizei) objData.vertices.size() * sizeof(Vertex),
  objData.vertices.data(), GL_STATIC_DRAW);
```

``` glBufferData ```를 호출하여 버퍼 오브젝트를 정점 정보들로 채운다.

> 💡 **인덱스 배열도 같은 과정으로 버퍼 오브젝트를 채운다.**   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/index-array.png)   

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/stride.png)   

이렇게 모든 정점과 관련된 정보들을 GPU 상에 있는 버퍼 오브젝트로 옮기면 위 이미지와 같은 구조가 된다. 해당 값들에 접근하기 위해 아래와 같이 코드를 구현한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/attributes3.png)   

- 정점 애트리뷰트 활성화

```ini
glEnableVertexAttribArray(0);
```

``` glEnableVertexAttribArray ``` 함수를 이용하여 해당 인덱스에 있는 정점 애트리뷰트를 활성화시킨다.

- 해당 애트리뷰트에 대한 자세한 정보 제공

```ini
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE,
    sizeof(Vertex), (const GLvoid*) offsetof(Vertex, pos));
```

``` glVertexAttribPointer ```를 이용하면 해당 애트리뷰트에 있는 내용에 대한 자세한 정보를 제공받을 수 있다. 해당 함수의 구조는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/vertex-attrib-pointer.png)   

앞서 설정했던 ``` Vertex ``` 구조체의 크기가 32바이트였으므로, 첫 번째 ``` pos ```와 두 번째 ``` pos ``` 간 간격은 32이바이트이다. 이런 **바이트 간격**을 **스트라이드(Stride)**라 한다. 하나의 ``` pos ```를 얻은 후 **동일한 스트라이드를 반복**하여 앞으로 나가면 모든 ``` pos ```를 얻을 수 있다. 이는 다른 값들에서도 마찬가지이다.

위에서 보았던 ``` glVertexAttribPointer ``` 함수에서 ``` sizeof(Vertex) ```가 스트라이드에 해당한다.

***

### 🌱 Uniforms
![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/uniforms.png)   

위 코드는 월드 행렬을 제공하는 GL 프로그램이다. 뷰 행렬, 투영 행렬도 마찬가지로 설정한다. 단, **가상 공간에서 물체가 움직이는 경우 월드 행렬은 매 프레임 갱신**되어야 한다. 뷰 행렬도 마찬가지지만 투영 행렬은 특별한 경우가 아니면 매 프레임 변환할 필요가 없으니 이 점을 주의하자.

> 💡 투영 행렬은 ``` fovy ```, ``` aspect ```, ``` n ```, ``` f ```이 네 가지 파라미터로만 행렬이 이루어지기 때문에 특별한 경우가 아니면 바뀌지 않는다.

- 매 프레임마다 변경되는 다이나믹 오브젝트 선언

```ini
glm::mat4 worldMatrix;
```

월드 행렬과 같은 크기의 행렬 값을 저장할 ``` worldMatrix ``` 변수를 하나 선언해주었다. 매 프레임 변경되는 월드 행렬의 값을 저장할 것이다.

- 행렬의 위치 찾아내기

```ini
GLint loc = glGetUniformLocation(program, "worldMat");
```

프로그램 오브젝트 내에 존재하는 월드 행렬의 **위치**를 찾아야한다. ``` worldMatrix ```를 정점 쉐이더 유니폼인 ``` worldMat ```에 할당해야 하기 때문이다. 프로그램 오브젝트를 실행 파일에 **링크**했을 때 ``` worldMat ```의 위치는 결정된다.

- 유니폼 행렬 값 변경하기

```ini
glUniformMatrix4fv(loc, 1, GL_FALSE, glm::value_ptr(worldMatrix));
```

``` glUniformMatrix4fv ``` 함수를 이용하여 변경된 월드 행렬값을 정점 쉐이더 내의 월드 행렬값으로 대입한다. 정점 쉐이더의 유니폼 변수에 특정 값을 할당하기 위하여 GL은 ``` glUniform ```으로 시작하는 함수들을 이용하는데, ``` 4 × 4 ``` 행렬인 ``` worldMat ```을 위해 위 함수를 호출한다.

``` glGetUniformLocation ``` 함수와 ``` glUniformMatrix4fv ``` 함수의 구조는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/get-uniform-location.png)   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/uniform-matrix-4fv.png)   

***

## 👻 Draw Call
이제 GPU가 연산을 수행할 준비가 완료되었다. GPU에게 연산을 수행하라고 명령을 호출하는 것을 **드로우 콜(Draw Call)**이라고 한다. ``` glDraw ```로 시작하는 함수를 이용한다. 정점 배열만 이용하는 방법과 인덱스 배열을 함께 이용하는 방법, 총 두 가지 방법이 존재한다.

- 정점 배열만을 이용한 방법

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/draw-arrays.png)   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/draw-arrays2.png)   

위에서 잠깐 언급했듯, ``` ELEMENT ```가 붙은 오브젝트는 인덱스와 관련이 있다. 이는 함수도 마찬가지이다. 인덱스 배열을 사용하지 않고 오로지 정점 배열만을 이용하여 출력하는 방법은 ``` glDrawArrays ``` 함수를 사용한다.

- 인덱스 배열을 포함한 방법

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/draw-elements.png)   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader/draw-elements2.png)   

``` glDrawElements ```를 사용하면 정점 배열과 인덱스 배열을 모두 사용하여 설정한 메시를 그리라는 의미이다.

이 과정이 끝나면 **렌더링**이 끝나게된다.

***

## 👻 글을 마치며
이번 시간에는 OpenGL ES를 이용한 쉐이더 처리 과정에 대해 알아보았다. 약간은 생소한 언어지만 C 기반이라 비슷한 문법이 많아서 이해하는데 그리 오랜 시간이 걸리지 않았다. 이제 내 컴퓨터에 OpenGL ES를 사용할 환경을 설정하고 직접 구현해보며 머릿속에만 있는 가상의 처리 과정 구조를 눈으로 확인할 수 있도록 연습해야겠다.

***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (6장-OpenGL ES와 쉐이더)](https://youtu.be/O3YQQwDyHKo)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   