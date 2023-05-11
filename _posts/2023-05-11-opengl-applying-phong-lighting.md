---
title: "[OpenGL] 모델에 노멀, 퐁 라이팅 적용하기"
excerpt: "지난 시간에 불러온 모델에 퐁 라이팅 적용하기"

categories:
  - OpenGL
tags:
  - [OpenGL, 330, computer graphics, phong lighting]

permalink: /opengl/applying-phong-lighting/

toc: true
toc_sticky: true

date: 2023-05-11 19:49:46+0900
last_modified_at: 2023-05-11 19:49:50+0900
---

## 👻 복습!
_[👉 퐁 라이팅 개념 보러가기👈](/computer-graphics/lighting/)_

***

## 👻 퐁 라이팅 적용하기
지난 시간에 만들어 두었던 코드에 노멀을 적용한 퐁 라이팅 기능을 추가해보았다.

***

### 🌱 GL 프로그램

- **행렬 핸들러 분해, 뷰 벡터, 빛 벡터 추가**

```c++
// 행렬 핸들러
// GLuint MatrixID = glGetUniformLocation(programID, "MVP");
// - 분해 -
GLuint ProjMatID = glGetUniformLocation(programID, "ProjMat");
GLuint ViewMatID = glGetUniformLocation(programID, "ViewMat");
GLuint WorldMatID = glGetUniformLocation(programID, "WorldMat");
// eyePos 넘겨주기 : 얘는 카메라 원점이랑 같음
GLuint eyePosID = glGetUniformLocation(programID, "eyePos");
// lightDir 넘겨주기 : 퐁 라이팅 할려구
GLuint lightDirID = glGetUniformLocation(programID, "lightDir");
```

라이팅에 적용하려면 뷰 벡터가 필요하기 때문에 MVP를 분해해서 각각 유니폼으로 넘겨주려고 핸들러도 분해했다. 또 뷰 벡터랑 빛 벡터도 추가 등록.

- **Normal Buffer 생성**

```c++
GLuint normalbuffer;
glGenBuffers(1, &normalbuffer);
glBindBuffer(GL_ARRAY_BUFFER, normalbuffer);
glBufferData(GL_ARRAY_BUFFER, normals.size() * sizeof(vec3), &normals[0], GL_STATIC_DRAW);
```

노멀 버퍼 생성, 바인드, 값 넣기

- **행렬 및 벡터 임시 지정**

```c++
mat4 World = mat4(1.0);
vec3 eyePos = vec3(13, 13, 10);
vec3 lightDir = vec3(6, 7, -5);
```

그냥 대충 나무 이쁘게 보여지는 걸로..

- **유니폼 행렬 전달**

```c++
// 유니폼 행렬(MVP) 전달
// glUniformMatrix4fv(MatrixID, 1, GL_FALSE, &MVP[0][0]);
// - 분해 - : 퐁 라이팅 하려구
glUniformMatrix4fv(ProjMatID, 1, GL_FALSE, &Projection[0][0]);
glUniformMatrix4fv(ViewMatID, 1, GL_FALSE, &View[0][0]);
glUniformMatrix4fv(WorldMatID, 1, GL_FALSE, &World[0][0]);
glUniform3fv(eyePosID, 1, &eyePos[0]);
glUniform3fv(lightDirID, 1, &lightDir[0]);
```

``` glUniformMatrix3fv ```는 4 × 4 행렬 전달, ``` glUniform3fv ```는 3차원 벡터 전달

- **VBO 생성**

```c++
glEnableVertexAttribArray(2);
glBindBuffer(GL_ARRAY_BUFFER, normalbuffer);
glVertexAttribPointer(
	2,
	3,
	GL_FLOAT,
	GL_FALSE,
	0,
	(void*)0
);
```

- **버퍼 제거**

```c++
glDeleteBuffers(1, &normalbuffer);
```

***

### 🌱 Vertex Shader

```ini
#version 330 core

layout(location = 0) in vec3 position;
layout(location = 1) in vec2 texCoord;
layout(location = 2) in vec3 normal;

// uniform mat4 MVP;
uniform mat4 ProjMat, ViewMat, WorldMat;
uniform vec3 eyePos;

out vec2 v_texCoord;
out vec3 v_normal, v_view;

void main() 
{
	// 클립 공간 좌표
	gl_Position = ProjMat * ViewMat * WorldMat * vec4(position, 1.0);

	// uv 좌표
	v_texCoord = texCoord;

	// 월드 공간에서의 노멀
	v_normal = normalize(transpose(inverse(mat3(WorldMat))) * normal);

	// view vector 구하려고 worldPos 구하기
	vec3 worldPos = (WorldMat * vec4(position, 1.0)).xyz;
	v_view = normalize(eyePos - worldPos);
}
```

***

### 🌱 Fragment Shader

```ini
#version 330 core

precision mediump float;

uniform sampler2D myTextureSampler;
uniform vec3 lightDir;

in vec2 v_texCoord;
in vec3 v_normal, v_view;

out vec4 color;

void main()
{
	// 광원의 색상
	vec3 lightColor = vec3(1.0);
	// 스페큘러 계수
	vec3 matSpec = vec3(1.0, 1.0, 1.0);
	// 앰비언트 광원 색상
	vec3 srcAmbi = vec3(0.3, 0.3, 0.3);

	// normalization
	vec3 normal = normalize(v_normal);
	vec3 view = normalize(v_view);
	vec3 light = normalize(lightDir);

	// Diffuse Term
	vec3 matDiff = texture(myTextureSampler, v_texCoord).rgb;
	vec3 diff = max(dot(normal, light), 0.0) * lightColor * matDiff;

	// Specular Term
	vec3 refl = 2.0 * normal * dot(normal, light) - light;
	vec3 spec = pow(max(dot(refl, view), 0.0), 30.0) * lightColor * matSpec;

	// Ambient Term
	vec3 ambi = srcAmbi * matDiff;

	color = vec4(diff + spec + ambi, 1.0);
}
```

Emissive Term은 귀찮아서 그냥 안 넣었다(어차피 나무가 발산하지도 않음😋).

***

## 👻 결과
- **World Normal**

```ini
color = vec4(normal, 1.0);
```

![Alt Text](/assets/images/posts_img/basics/opengl/applying-phong-lighting/normal.PNG)   

- **Diffuse Term**

```ini
color = vec4(diff, 1.0);
```

![Alt Text](/assets/images/posts_img/basics/opengl/applying-phong-lighting/diff.PNG)   

- **Specular Term**

```ini
color = vec4(spec, 1.0);
```

![Alt Text](/assets/images/posts_img/basics/opengl/applying-phong-lighting/spec.PNG)   

- **Ambient Term**

```ini
color = vec4(ambi, 1.0);
```

![Alt Text](/assets/images/posts_img/basics/opengl/applying-phong-lighting/ambi.PNG)   

- **최종 결과**

```ini
color = vec4(diff + spec + ambi, 1.0);
```

![Alt Text](/assets/images/posts_img/basics/opengl/applying-phong-lighting/result.PNG)   

***

## 👻 글을 마치며
이번 시간에는 라이팅 적용을 해보았다. 하나씩 만져보면서 하니까 너무 재미있다!!(무엇보다 에러가 안 나니까 짱 재밌다🤗) 퐁 라이팅을 배운지 꽤 돼서 기억이 날까 걱정했었는데, 그래도 기억 나는거 보니 공부를 좀 열심히 했긴 했나보다 ㅎㅎ! 이제 OpenGL에도 슬슬 적응이 되는 것 같다. 코드를 더 빠싹하게 공부해서 멋진 거 하나 만들어보고 싶다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/Study_OpenGL/tree/main/OpenGL/Mapping)_