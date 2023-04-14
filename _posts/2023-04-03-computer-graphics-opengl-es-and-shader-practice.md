---
title: "[Computer Graphics] #6-1. OpenGL ES와 쉐이더 연습문제 풀이"
excerpt: "한정현 컴퓨터 그래픽스 강의 6장 연습문제 풀이 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, opengl, opengl es, es, embeded system, shader, practice]

permalink: /computer-graphics/opengl-es-and-shader-practice/

toc: true
toc_sticky: true

date: 2023-04-03 23:33:32+0900
last_modified_at: 2023-04-03 23:33:36+0900
published: true
---

## 👻 연습문제 풀이
6장 OpenGL ES와 쉐이더 챕터의 연습문제는 총 2개이다.

***

### 🌱 1
- **문제**

> 아래와 같은 GL 프로그램과 정점 쉐이더가 주어졌다. worldMat가 비균등 축소확대를 포함한다는 것을 가정하여 빈칸을 채워라. glVertexAttribPointer의 두 번째 파라미터는 해당 애트리뷰트의 원소 개수를, 다섯 번째 파라미터는 스트라이드(stride)를 말한다.
>
> ```ini
> 1:  #version 300 es
> 2: 
> 3:  uniform mat4 worldMat, viewMat, projMat;
> 4:
> 5:  layout(location = 2) in vec3 position;
> 6:  layout(location = 3) in vec3 normal;
> 7:  layout(location = 7) in vec2 texCoord;
> 8:
> 9:  out vec3 v_normal;
> 10: out vec2 v_texCoord;
> 11:
> 12:  void main() {
> 13:       gl_Position = ❓;
> 14:       v_normal = ❓;
> 15:       v_texCoord = texCoord;  
> 16:  }
> ```
>
> ```ini
> 1:  struct Vertex
> 2:  {
> 3:      glm::vec3 pos;  // position
> 4:      glm::vec3 nor;  // normal
> 5:      glm::vec2 tex;  // texture coordinates
> 6:  };
> 7:  typedef GLushort Index;
> 8:  
> 9:  glEnableVertexAttribArray(❓); // position
> 10: glVertexAttribPointer(❓, ❓, GL_FLOAT, GL_FALSE,
>         ❓, (const GLvoid*) offsetof(Vertex, pos));
> 11: 
> 12: glEnableVertexAttribArray(❓);  // normal
> 13: glVertexAttribPointer(❓, ❓, GL_FLOAT, GL_FALSE,
>         ❓, (const GLvoid*) offsetof(Vertex, nor));
> 14: 
> 15: glEnableVertexAttribArray(❓);  // texture coordinates
> 16: glVertexAttribPointer(❓, ❓, GL_FLOAT, GL_FALSE,
>         ❓, (const GLvoid*) offsetof(Vertex, tex));
> ```

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader-practice/1-solve.jpg)   

***

### 🌱 2
- **문제**

> 아래의 메시는 총 8개의 삼각형으로 구성되어 있다.   
<img src="/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader-practice/2.jpg" width="50%">   
(a) 인덱스를 사용하여 위 메시를 표현했을 때, 우리는 glDrawElements(mode, count, type, indices)를 호출할 것이다. mode와 count 값은 각각 무엇인가?
>
> (b) 인덱스 없이 위 메시를 표현했을 때, 우리는 glDrawElements(mode, count, type, indices)를 호출할 것이다. mode와 count 값은 각각 무엇인가?

- **풀이**

![Alt Text](/assets/images/posts_img/basics/computer-graphics/opengl-es-and-shader-practice/2-solve.jpg)   

***

## 👻 글을 마치며
이번 시간에는 6장 OpenGL ES와 쉐이더 챕터의 연습문제를 풀어보았다. 코드를 적는 부분이라 어렵지 않게 이해하고 넘어갈 수 있었다. 쉐이더 언어가 익숙하지 않아서 헷갈리는 부분이 좀 있지만 그래도 기본적으로 C언어와 비슷해서 금방 외울 수 있을 것 같다.

***

_출처_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   