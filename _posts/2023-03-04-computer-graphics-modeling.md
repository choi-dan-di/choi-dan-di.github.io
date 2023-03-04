---
title: "[Computer Graphics] #3. 모델링"
excerpt: "한정현 컴퓨터 그래픽스 강의 3장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, modeling]

permalink: /computer-graphics/modeling/

toc: true
toc_sticky: true

date: 2023-03-04 18:00:59+0900
last_modified_at: 2023-03-04 18:01:02+0900
---

## 👻 모델링
그래픽스에서 렌더링 할 물체를 만들어내는 것.
gpu가 음함수를 처리하기에 적합하게 설계되어있지 않다. 음함수 표현 대신 부드러운 표면의 점들을 샘플하면 정점이 나오게 된다. 정점을 이으면 폴리곤 메시가 된다. opengl은 삼각형 메시만 처리한다. 꼭지점 개수의 두배만큼의 삼각형 개수가 존재한다.
맥스나 마야는 사각형 메시를 사용하는 것이 유용하다.

정점의 개수를 어느정도로 할 것인가?
정점의 개수 = 해상도 = LOD(level of detail)
실제로 화면상에 물체가 어느정도의 크기로 나올것인지에 따라 달라질 수 있다.
해상도 늘리기 refinement
줄이기 simplification

폴리곤 메시가 컴퓨터 안에서 어떤식으로 표현이 될까?
정점의 정보를 배열에 저장한다. (삼각형 단위로) (vertex array)
중복된 데이터가 많아지는 문제점이 있다. 좋은 표현법은 아님.
-> 정점을 중복 없이 나열하는 해결 방법
vertex array에는 정점의 정보만 저장해두고 index array를 따로 만들어 정점의 인덱스 정보만을 담는다.
-> 삼각형 메시를 표현하는 가장 일반적인 형태이다.
낭비인 것 같지만 그렇지 않다. index array는 2byte나 4byte로 표현할 수 있을 정도의 작은 크기를 가진다.
vertex array의 한 셀은 용량이 크다. (정점에 대한 모든 정보가 들어가있기 때문)

surface normals
정점의 포지션 외의 정보 중 하나가 normal이다. 법선 벡터라고도 한다. 한 평면에 대해 수직인 벡터를 법선 벡터라고 한다.
오른손 법칙을 적용하여 벡터곱을 구하고 정규화를 하면 삼각형 normal을 구할 수 있다.
정점의 순서는 시계 반대 방향으로. counter-clockwise(CCW)
정점의 순서가 바뀌면 normal의 방향도 바뀌게 된다. -> 그래픽스에서는 모든 노멀이 물체의 밖을 향하는 것이 기본적인 관례이다. => 그래서 정점의 순서가 반시게 방향으로 나열될 수 있도록 주의해야한다.

정점 노멀
부드러운 표면에서 tangent plane을 구할 수 있고, 여기에 수직인 벡터를 정점 노멀이라한다. 부드러운 표면이 주어지지 않고 오로지 폴리곤 메시만 존재한다면 정점을 공유하는 삼각형 노멀의 평균을 내는 방식으로 구할 수 있다. -> 프로그램(맥스)이 해준다.

정점의 포지션과 노멀.
맥스에서 모델링하면 수동으로 만들어도 노멀은 자동으로 만들어진다.
정점 노멀은 vertex array가 반드시 가져야 할 정보이다. (+포지션)

export and import
application 1 : Max
-> export
file
-> import
application 2 : Unity
MAXScript를 이용해 간단하게 exporter를 짤 수 있다. 혹은 메뉴에서 export 선택 가능
.obj 파일 형식이 가장 많이 쓰인다.

정점 노멀의 값이 동일하면 하나의 노멀 정보만 저장하면 된다. 구 같은 경우 모두 다른 정점 노멀값을 갖기 때문에 정점의 개수만큼 만들어지지만, 박스 같은 경우 노멀 정보가 겹칠 수도 있다.

+ 실제 삼각형 정보
f로 시작. face의 약자.(면)
각 꼭지점의 정보를 알려줘야함. 슬래쉬 두 개로 구분이 된 인덱스. (v//vn)

파일이 import되면 va나 ia가 파일 정보를 바탕으로 생성이 될 것이다.


***

## 👻 글을 마치며


***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (3장-모델링)](https://youtu.be/CAfdIW8M6HA)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   