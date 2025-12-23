---
title: "[OpenGL] 노멀 매핑 해보기"
excerpt: "모델에 노멀 매핑 해보기"

categories:
  - OpenGL
tags:
  - [OpenGL, 330, computer graphics, normal mapping]

permalink: /opengl/applying-normal-mapping/

toc: true
toc_sticky: true

date: 2023-05-11 23:07:57+0900
last_modified_at: 2023-05-11 23:08:01+0900
---

## 👻 노멀 매핑 해보기
지난 시간엔 모델의 obj 파일에 있는 노멀로 라이팅까지 적용해보았다. 이제 노멀맵을 이용해서 더욱 고퀄리티의 모델을 만들어보자!

- **복습!**

_[👉 노멀 매핑 개념 보러가기 👈](/computer-graphics/normal-mapping/)_

***

### 🌱 GL 프로그램
- **노멀맵 핸들러 추가**

```c++
GLuint NormalTexture = loadBMP_stb("texture/tree_default_Normal.bmp");
GLuint NormalTextureID = glGetUniformLocation(programID, "normalMap");
```

원래 _[OpenGL Tutorial](http://www.opengl-tutorial.org/kr/intermediate-tutorials/tutorial-13-normal-mapping/)_ 글에 있는 ``` loadBMP_custom ``` 함수를 사용했었는데 이게 모든 이미지 파일을 불러와서 픽셀별로 매핑을 하다보니 원하는 결과값을 구하지 못했었다. 그래서 원래 텍스처도 잘못 불러온건데 육안으로 구분하기 힘들어서 틀린지도 몰랐던..😭 그래서 **외부 라이브러리**를 사용해서 다시 가져오는 쪽으로 코드를 수정했다.

_[외부 라이브러리 stb_image 사용](https://github.com/nothings/stb/blob/master/stb_image.h)_

> 💡 **수정된 코드**
>
> ```c++
> GLuint loadBMP_stb(const char* imagepath)
> {
> 	printf("STB Reading image %s\n", imagepath);
> 
> 	int width, height, channel;
> 	GLuint textureID;
> 
> 	glGenTextures(1, &textureID);
> 
> 	glBindTexture(GL_TEXTURE_2D, textureID);
> 	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);
> 	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
> 	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
> 	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
> 
> 	// 파일 오픈
> 	unsigned char* data = stbi_load(imagepath, &width, &height, &channel, 0);
> 	if (data)
> 	{
> 		glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
> 		glGenerateMipmap(GL_TEXTURE_2D);
> 	}
> 	else
> 	{
> 		printf("Error");
> 		getchar();
> 		return 0;
> 	}
> 
> 	stbi_image_free(data);
> 
> 	glBindTexture(GL_TEXTURE_2D, 0);
> 
> 	return textureID;
> }
> ```

- **탄젠트 공간 노멀 매핑 위해서 탄젠트 정의**

```c++
vector<vec3> tangents;
getTangent(vertices, uvs, normals, tangents);
```

탄젠트도 앞에서 구했던 정점, uv 좌표, 노멀과 같이 vector로 저장해줘서 버퍼에 바인딩 후 넘겨줄 것이다. 탄젠트 공간을 정의하려면 기저 {T, B, N}이 필요한데 N은 노멀값 그대로 사용, T는 위에서 만든 함수 ``` getTangent ```로 구해서 저장해주고 B는 쉐이더에서 벡터곱으로 정의해주었다.

> 💡 ``` getTangent ```
> 
> ```c++
> void getTangent(vector<vec3>& vertices, vector<vec2>& uvs, vector<vec3>& normals, vector<vec3>& tangents)
> {
> 	// 무조건 3의 배수(폴리곤 메시)라서 에러날 일이 없음
> 	for (unsigned int i = 0; i < vertices.size(); i += 3)
> 	{
> 		// 이름 줄이기
> 		vec3& v0 = vertices[i + 0];
> 		vec3& v1 = vertices[i + 1];
> 		vec3& v2 = vertices[i + 2];
> 
> 		vec2& uv0 = uvs[i + 0];
> 		vec2& uv1 = uvs[i + 1];
> 		vec2& uv2 = uvs[i + 2];
> 
> 		// 탄젠트를 구하려면 노멀은 그대로 사용하고
> 		// 그람-슈미트 사용해야하나..?
> 		// 정점 돌면서 삼각형을 만드는 두 벡터 구하기
> 		vec3 deltaPos1 = v1 - v0;
> 		vec3 deltaPos2 = v2 - v0;
> 
> 		// UV
> 		vec2 deltaUV1 = uv1 - uv0;
> 		vec2 deltaUV2 = uv2 - uv0;
> 
> 		float r = 1.0f / (deltaUV1.x * deltaUV2.y - deltaUV1.y * deltaUV2.x);
> 		vec3 tangent = (deltaPos1 * deltaUV2.y - deltaPos2 * deltaUV1.y) * r;
> 
> 		// 세 정점의 탄젠트 벡터는 모두 동일
> 		tangents.push_back(tangent);
> 		tangents.push_back(tangent);
> 		tangents.push_back(tangent);
> 	}
> }
> ```

- **Tangent Buffer 생성**

```c++
GLuint tangentbuffer;
glGenBuffers(1, &tangentbuffer);
glBindBuffer(GL_ARRAY_BUFFER, tangentbuffer);
glBufferData(GL_ARRAY_BUFFER, tangents.size() * sizeof(vec3), &tangents[0], GL_STATIC_DRAW);
```

- **메인 함수 루프 내 텍스처 바인드 추가**

```c++
glActiveTexture(GL_TEXTURE1);
glBindTexture(GL_TEXTURE_2D, NormalTexture);
glUniform1i(NormalTextureID, 1);
```

- **VBO 추가**

```c++
glEnableVertexAttribArray(3);
glBindBuffer(GL_ARRAY_BUFFER, tangentbuffer);
glVertexAttribPointer(
	3,
	3,
	GL_FLOAT,
	GL_FALSE,
	0,
	(void*)0
);
```

- **정점 애트리뷰트 비활성, 버퍼 제거**

```c++
glDisableVertexAttribArray(3);
glDeleteBuffers(1, &tangentbuffer);
```

참고로 정점 애트리뷰트 비활성은 루프 내에서, 버퍼 제거는 루프가 끝난 후 실행된다.

***

### 🌱 Vertex Shader

```ini
#version 330 core

layout(location = 0) in vec3 position;
layout(location = 1) in vec2 texCoord;
layout(location = 2) in vec3 normal;
layout(location = 3) in vec3 tangent;

// uniform mat4 MVP;
uniform mat4 ProjMat, ViewMat, WorldMat;
uniform vec3 eyePos, lightDir;

out vec2 v_texCoord;
// out vec3 v_view;
out vec3 v_lightTS, v_viewTS;

void main() 
{
	// 클립 공간 좌표
	gl_Position = ProjMat * ViewMat * WorldMat * vec4(position, 1.0);

	// uv 좌표
	v_texCoord = texCoord;

	// view vector 구하려고 worldPos 구하기
	vec3 worldPos = (WorldMat * vec4(position, 1.0)).xyz;
	// v_view = normalize(eyePos - worldPos);

	// 탄젠트 공간 노멀 매핑 위해 TBN 구하기
	vec3 Nor = normalize(transpose(inverse(mat3(WorldMat))) * normal);
	vec3 Tan = normalize(transpose(inverse(mat3(WorldMat))) * tangent);
	vec3 Bit = cross(Nor, Tan);
	// 탄젠트 공간 변환 행렬
	mat3 tbnMat = transpose(mat3(Tan, Bit, Nor));

	// 빛 벡터, 뷰 벡터를 탄젠트 공간으로 변환
	v_lightTS = tbnMat * normalize(lightDir);
	v_viewTS = tbnMat * normalize(eyePos - worldPos);
}
```

***

### 🌱 Fragment Shader

```ini
#version 330 core

precision mediump float;

uniform sampler2D colorMap, normalMap;
uniform vec3 lightDir;

in vec2 v_texCoord;
// in vec3 v_normal;
// in vec3 v_view;
in vec3 v_lightTS;
in vec3 v_viewTS;

out vec4 color;

void main()
{
	// Phong Lighting
	// 광원의 색상
	vec3 lightColor = vec3(1.0);
	// 스페큘러 계수
	vec3 matSpec = vec3(1.0, 1.0, 1.0);
	// 앰비언트 광원 색상
	vec3 srcAmbi = vec3(0.3, 0.3, 0.3);

	// normalization
	// Normal Mapping
	// normal map filtering
	// 범위 전환
	// vec3 normal = normalize(v_normal);
	vec3 normal = normalize(2.0 * texture(normalMap, v_texCoord).xyz - 1.0);
	// 탄젠트 공간 변환
	vec3 view = normalize(v_viewTS);
	vec3 light = normalize(v_lightTS);

	// Diffuse Term
	vec3 matDiff = texture(colorMap, v_texCoord).rgb;
	vec3 diff = max(dot(normal, light), 0.0) * lightColor * matDiff;

	// Specular Term
	vec3 refl = 2.0 * normal * dot(normal, light) - light;
	vec3 spec = pow(max(dot(refl, view), 0.0), 30.0) * lightColor * matSpec;

	// Ambient Term
	vec3 ambi = srcAmbi * matDiff;

	color = vec4(diff + spec + ambi, 1.0);
}
```

***

## 👻 결과
- **NormalMap으로 텍스처링**

```ini
color = vec4(texture(normalMap, v_texCoord).rgb, 1.0);
```

![Alt Text](/assets/images/posts_img/basics/opengl/applying-normal-mapping/normal-mapping.PNG)   

- **최종 결과**

```ini
color = vec4(diff + spec + ambi, 1.0);
```

![Alt Text](/assets/images/posts_img/basics/opengl/applying-normal-mapping/result.PNG)   

➕   
친구한테 보여줬더니 나무를 꾸며주었다 ☺☺☺

<img src="/assets/images/posts_img/basics/opengl/applying-normal-mapping/tree.jpg" width="50%">

***

## 👻 글을 마치며
이번 시간에는 노멀 매핑 적용에 관한 내용을 정리해보았다. 그래도 코드 분석하고 직접 하다보니 이해가 되면서 혼자 할 수 있는 범위가 많아진 것 같다! 문제 해결도 스스로~ ~~스터디원들한테 질문을 엄청 많이 하긴 했지만..~~ 뿌듯하고 신기하고 재미있지만 눈이 너무 아파서 자주 쉬어줘야 할 것 같다 😞

***

_[소스코드 보러가기](https://github.com/choi-dan-di/Study_OpenGL/tree/main/OpenGL/Mapping)_