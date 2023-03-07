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

date: 2023-03-07 18:44:43+0900
last_modified_at: 2023-03-07 18:44:47+0900
---

## 👻 OpenGL ES와 쉐이더
Vertex and Index Arrays(revisited)
texture coordinate도 정점 배열에 저장되어있따.
3개의 구성요소로 각각의 정점이 정의됨
gpu에서는 병렬적으로 정점들이 처리될 것이다.

OpenGL ES
3.0버전으로 코딩할 것
C++ 기반 템플릿 opengl es function call
추가되는 것이 shader(2가지: vertex, fragment). 우리가 구현해야함
gpu에 특화된 프로그램. 다른 언어를 사용. shading language
opengl es가 제공하는 api와 shading language를 알아야한다

OpenGL ES Shading Language
일반적으로 C와 비슷하다.
단, 행렬, 벡터를 자주 사용할 것이기 때문에 이 기능을 지원해주는 특별한 타입들이 추가되어있다.
vec4 : 4차원 벡터
ivec3 : integer 3d vector
mat3, mat4 : 3x3, 4x4 행렬
mat3x4 : 3x4 행렬
행렬의 모든 원소는 float 타입을 가진다

Vertex Shader
기본적으로 2가지 입력을 받는다
1. Attributes : va를 구성하는 종류들(위치, 노멀, texcoord 등)
2. Uniforms : 모든 정점마다 uniform하게 적용이 되는 데이터. 대표적인 예로 world transform, view transform, projection transform
최종적으로 클립 공간으로 들어가게되고, 출력이 될 것이다. clip space position은 반드시 출력되어야 하기 때문에 내정 변수(built-in variable)(gl_Position)로 따로 관리한다. 나머지는 별도의 output으로 출력
~코드 분석~

GL Program
프래그먼트 쉐이더도 정점 쉐이더와 비슷한 방식으로 작성되지만 더 복잡하다.
opengl의 함수는 소문자 gl로 시작
datatype은 대문자 GL로 시작

GL Program - Shader Object
맨 처음 정점 쉐이더 파일이 주어졌을 때, shader object를 만들어야한다 (glCreateShader)
shader object에게 소스코드를 준다(glShaderShource) (char*타입 source)
쉐이더 컴파일 : glCompileShader
-> 프쉐도 똑같은 3단계를 거쳐야함

GL Program - Program Object
두 개의 쉐이더 오브젝트가 만들어지면 program object로 통합
glCreateProgram()
glAttachShader
glLinkProgram
glUseProgram

Attributes
마찬가지로 명시 필요
.obj 파일과 비슷한 구조로 구현
glm:opengl math library
texture coordinates : 2차원 좌표
cpu 메모리에 있는 va, ia를 gpu 메모리로 옮겨줘야한다. -> gpu 메모리에서 buffer object를 만든다
glGenBuffers로 버퍼 제너레이트(생성)
glBindBuffer : 버퍼와 오브젝트 연결
glBufferData : cpu to buffer. 버퍼에 데이터 제공->하면 gpu로 전달 가능
인덱스 어레이도 마찬가지로 작업한다.
버퍼에 있는 데이터를 어떻게 읽을지 정해줘야함
opengl program -> vertex shader에게 알려줘야함
stride는 sizeof(Vertex)크기. 노말의 시작점 혹은 위치의 시작점을 알려줌
glEnableVertexAttribArray(num) : va 활성화. position = attribute [num]
glVertexAttribPointer(num1, num2, type, bool, stride) : num1이 시작점. num2차원 원소이면서 모두 type다

Uniforms
worldMat, viewMat, projMat
dynamic environment 고려해야함
매 프레임마다 변하는 worldMat, viewMat을 opengl에서 우리가 설정해줘야한다. (어떻게? 는 문제. 생각해보기 ㅇ_<)
projMat 특별한 경우가 아니면 바뀌지 않음(fovy, aspect, n, f)
mat의 위치를 찾아내야함. program object 안에 있는 -> glGetUniformLocation

Drawcalls
gpu가 이제 연산을 수행하라고 명령을 호출하는 것
glDrawArrays : ia없이 va로만 그림을 그려라
glDrawElements : va, ia로 그림을 그려라
(element가 들어가면 index 연상하기)

이러면 렌더링~ 끝~





***

## 👻 글을 마치며


***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (6장-OpenGL ES와 쉐이더)](https://youtu.be/O3YQQwDyHKo)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   