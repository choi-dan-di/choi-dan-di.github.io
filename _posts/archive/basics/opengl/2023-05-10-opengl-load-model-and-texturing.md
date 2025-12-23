---
title: "[OpenGL] 모델 불러오기 & 텍스처 입히기"
excerpt: "스터디 실습 중간 메모"

categories:
  - OpenGL
tags:
  - [OpenGL, 330, computer graphics]

permalink: /opengl/load-model-and-texturing/

toc: true
toc_sticky: true

date: 2023-05-10 21:00:54+0900
last_modified_at: 2023-05-10 21:00:57+0900
---

## 👻 모델 불러오기
참고로 모델은 저번 소모임 스터디 때 받은 파일로 실습..^^7

- **GL 프로그램**

```c++
// -------------------------- 메인 함수 --------------------------
// .obj 파일 불러오기
vector<vec3> vertices;
vector<vec2> uvs;
vector<vec3> normals;
bool res = loadOBJ("object/tree.obj", vertices, uvs, normals);

// Vertex Buffer 생성
GLuint vertexbuffer;
glGenBuffers(1, &vertexbuffer);
glBindBuffer(GL_ARRAY_BUFFER, vertexbuffer);
glBufferData(GL_ARRAY_BUFFER, vertices.size() * sizeof(vec3), &vertices[0], GL_STATIC_DRAW);

// UV Buffer 생성
GLuint uvbuffer;
glGenBuffers(1, &uvbuffer);
glBindBuffer(GL_ARRAY_BUFFER, uvbuffer);
glBufferData(GL_ARRAY_BUFFER, uvs.size() * sizeof(vec2), &uvs[0], GL_STATIC_DRAW);

// -------------------------- 루프 내부 --------------------------
// 1rst attribute buffer : vertices
glEnableVertexAttribArray(0);
glBindBuffer(GL_ARRAY_BUFFER, vertexbuffer);
glVertexAttribPointer(
	0,                  // attribute
	3,                  // size
	GL_FLOAT,           // type
	GL_FALSE,           // normalized?
	0,                  // stride
	(void*)0            // array buffer offset
);

// 2nd attribute buffer : UVs
glEnableVertexAttribArray(1);
glBindBuffer(GL_ARRAY_BUFFER, uvbuffer);
glVertexAttribPointer(
	1,                                // attribute
	2,                                // size
	GL_FLOAT,                         // type
	GL_FALSE,                         // normalized?
	0,                                // stride
	(void*)0                          // array buffer offset
);

// 그리기 (드로우 콜)
glDrawArrays(GL_TRIANGLES, 0, vertices.size());

// 활성화 했던 정점 애트리뷰트 비활성
glDisableVertexAttribArray(0);
glDisableVertexAttribArray(1);
// -------------------------- 루프 끝 --------------------------

// 버퍼 제거
glDeleteBuffers(1, &vertexbuffer);
glDeleteBuffers(1, &uvbuffer);
```

모델 불러올 때 처음에 정점 정보만 받아오고 uv 좌표랑 노멀을 전혀 가져오지 못 하는 문제가 발생했었다. obj 파일을 아무리 파헤쳐도 이상한 부분이 없었고 됐다 안 됐다 하는 것이 굉장히 힘들었다는..

결국 obj 파일 파싱하는 부분에서 이상한 점을 겨우 찾을 수 있었다.

```c++
bool loadOBJ(
	const char* path,
	vector<vec3> &out_vertices,
	vector<vec2> out_uvs,
	vector<vec3> out_normals
);
```

~~바로 찾으면 천재~~

저 부분 때문에 몇날 며칠 고생했던 거 + 한 줄씩 디버그 한 거 생각하면 너무 슬프지만.. 그래도 찾았을 때 그 뿌듯함이 이루 말할 수 없당 😋 그래도 다음부터는 꼼꼼히 **참조** 붙였는지 확인해야지..

***

## 👻 텍스처 입히기
- **GL 프로그램**

```c++
// -------------------------- 메인 함수 --------------------------
// 텍스처 로드
// GLuint Texture = loadDDS("texture/tree_texture.DDS");
GLuint Texture = loadBMP_custom("texture/tree_default_Albedo.bmp");
// 프로그램에 전달할 ID 설정
GLuint TextureID = glGetUniformLocation(programID, "myTextureSampler");

// -------------------------- 루프 내부 --------------------------
// 텍스처 바인드
glActiveTexture(GL_TEXTURE0);
glBindTexture(GL_TEXTURE_2D, Texture);
glUniform1i(TextureID, 0);
// -------------------------- 루프 끝 --------------------------
// 텍스처 제거
glDeleteTextures(1, &Texture);
```

처음에 DDS로 로드하려 했는데 실패.. 그래서 BMP로도 변환해서 로드해보고 그래도 안 되다가 어찌저찌 고치게 되었다.

```c++
// 필쑤임니다. 주석처리하면 모델사라짐니다.
GLuint VertexArrayID;
glGenVertexArrays(1, &VertexArrayID);
glBindVertexArray(VertexArrayID);
```

바로 요 부분이 주석처리 되어있었는데 밑에 사용되는 부분이 없어서 습관적으로 주석처리 한 것 같다. 코드를 하나씩 뜯어보면서 진행하다보니 이런 문제도 생긴다는 것을 알았다. 그래도 덕분에 어떤 역할을 하는지 확실히 알게 되었으니 뭐 그걸로 만족.

***

## 👻 결과
- **Vertex Shader**

```ini
#version 330 core

layout(location = 0) in vec3 position;
layout(location = 1) in vec2 texCoord;

uniform mat4 MVP;

out vec2 v_texCoord;

void main() 
{
	gl_Position = MVP * vec4(position, 1.0);
	v_texCoord = texCoord;
}
```

- **Fragment Shader**

```ini
#version 330 core

precision mediump float;

in vec2 v_texCoord;

uniform sampler2D myTextureSampler;

out vec3 color;

void main()
{
	color = texture(myTextureSampler, v_texCoord).rgb;
}
```

- **결과물**

![Alt Text](/assets/images/posts_img/basics/opengl/load-model-and-texturing/result.PNG)   

***

## 👻 글을 마치며
최근 2개월 동안 **'OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문'** 교재를 가지고 스터디를 진행했었는데, 중간중간 실습하는 시간을 가졌었다. 라이팅, 노멀 매핑, 쉐도우 매핑 순이었는데 라이팅은 쉽게 할 수 있었다(퐁 모델 쉬움쉬움). 근데 노멀 매핑 부터 감이 1나도 안 잡히는 문제가.. 이러다간 또 미루기만하고 미완성인 채로 남기고 공부했던 것들도 까먹을 것 같아서 열심히 헤딩하고 기록하려고 부랴부랴 글을 썼다. 솔직히 포스팅 내용이 너무 중구난방이라 내 성에 차진 않지만 그래도 기록용으로 대충이라도 남긴다..(다음 번엔 제대로 정리해서 올려야지😭😭) 그래도 어찌저찌 여기까지 했으니 노멀 매핑이랑 쉐도우 매핑은 제대로 정리해봐야겠다(여기까지 만드는 데에도 다 합쳐서 10시간은 걸린 것 같음ㅠㅠ). 힘들었지만 뿌듯하다..

***

_[소스코드 보러가기](https://github.com/choi-dan-di/Study_OpenGL/tree/main/OpenGL/Mapping)_

***

_참고_   
_[OpenGL Tutorial 5 - 텍스처가 입혀진 큐브](http://www.opengl-tutorial.org/kr/beginners-tutorials/tutorial-5-a-textured-cube/)_   
_[OpenGL Tutorial 7 - 모델 불러오기](http://www.opengl-tutorial.org/kr/beginners-tutorials/tutorial-7-model-loading/)_