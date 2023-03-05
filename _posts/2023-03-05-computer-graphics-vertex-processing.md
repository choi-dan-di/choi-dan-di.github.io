---
title: "[Computer Graphics] #5. 정점 처리"
excerpt: "한정현 컴퓨터 그래픽스 강의 5장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, vertex processing, vertex, processing]

permalink: /computer-graphics/vertex-processing/

toc: true
toc_sticky: true

date: 2023-03-05 23:16:32+0900
last_modified_at: 2023-03-05 23:16:36+0900
---

## 👻 정점 처리
GPU Rendering Pipeline
드로우콜에 의해 gpu안으로 입력
폴리곤 메시들의 정점은 va에 저장
vertex shader가 정점 하나씩 불러들이면서 연산을 하나씩 수행
rasterizer ia의 정보를 이용해 삼각형을 다시 하나씩 조립한다. 색상을 결정할 정보를 모아서 각 위치에 저장함 -> fragment
-> 예비 픽셀, 후보 픽셀 정도로 이해
fragment shader : fragment color 결정
output merger 결정된 색상을 보여줄건지 말건지 보여주더라도 어떤 형태로 보여줄건지 결정해서 최종 screen에 뿌려ㅑ줌
대표적인 4단계
pipeline architecture 물 흐르듯이 흘러가는 단걔ㅖ . 출력이 입력으로 들어가는

파란색 표시(shader) 지피유에선 프로그램과 같은 말
-> 사용자가 직접 구현해줘야지만 실행됨
오렌지색 : 하드웨어로 고정. 정해진 함수나 기능만 수행할 수 있는 단계(legacy)

요번 시간은 vertex shader가 하는 일
vertex에 적용이 될 연산을 우리가 직접 구현할 수 있음

object space.에 있는 것들을 clip space로 옮기는 것은 반드시 필요한 과정

World Transform - Revisited
vertex positions에만 적용이 되었었음
또한가지 중요한 것 vertex normal
변환을 그대로 적용해도 되는 것인지?
vertex normal이 n일 때 lt에서 Ln+t
벡터행렬곱연산에서 덧셈인 이동t는 무시가능
L이 non-uniform scaling이면?
노멀이 수직이 아닌 결과가 나온다
결과적으로 L의 inverse transpose를 적용해야한다. 역행렬의 전치행렬
L^-T
여기에 기존 n을 곱하고 정규화하면 노멀을 구할 수 있음
유니폼 혹은 Rt이면 그냥 L을 써또 괜찮음 그냥 귀찮으니까 L^-T쓰자

View Transform
EYE : 카메라 시점
내가 찍고 싶은 방향:loot at(AT)
UP vector : 내가 찍고싶은 방향으 ㅣ위쪾 벡터
at to eye vector(eye - at) : 단위벡터 : n
UP x n = u
n x u = v
eye, at, up이 주어질 때 u, v, n이 정해지는 것만 알면 된다
u, v, n, eye가 완벽한 3차원 스페이스를 정의. 카메라 스페이스
u, v, n은 orthonormal
v는 정규화 필요없음. u, n이 이미 길이 1이고 평사넓이도 수직이기 떄문에 1이기 때문(정사각형)
world space -> camera space가 view transform
eye를 원점으로 끌고 오는 단계가 필요, -> 이 translation은 물체에 그대로 적용
그래서 eye와 물체는 적용이 되어ㅣㅇㅆ다. eye가 원점으로 움직이면 물체도 그만큼 이동한다
회전 행렬 이용해서 좌표계 일치 시키기 -> 월드 좌표 = 카메라 공간 좌표

이후는 렌더링 알고리즘을 개발하기 쉬워짐

뷰 매트릭스가 모든 물체에 적용이 되면 월드 공간에 있떤 물체들이 카메라 공간에 놓이게 된다고 볼 수 있따. (공간 이전의 개념)

Right-Hand System vs. Left-Hand System
오른손 좌표계는 내가 보는 방향 그대로 정방향으로ㅓ 나오지만 왼손 좌표계를 이용하면 거울모드로 나오게 된다.
(eye, at, up을 설정하면 당연한 결과)
해결 방법 -> z좌표의 방향만 반대로 해주면됨

View Frstum
줌인 줌아웃은 내부 파라미터. euye, at, up은 외부 파라미터
네ㅐ가지 파라미터로 내부 파라미터를 설정함
filed of view y-axis(fovy) : 시야각
세로는 fovx 종횡비 aspect = width/height(w/h) 보통 16:9 비율
거리도 자른다. n과 f(near plane, far plane) -> truncated pyramid(머리잘린 피라미드) 이걸 그래픽스에ㅐ선 view frustum이라고 한다.
view-frustum culling(솎아버린다) 그래서 구와 실린더는 화면이 나오지 않음./ 주전자만 렌더링 될 것
주전자의 손잡이 일부가 나가면 다 잘라서 나머지 부분만 처리. -> clipped. gpu내부에서 처리하니 상관x
하지만 자르기가 쉽지 않음 -> 절두체(시야)를 정육면체로 변환

Projection Transform
밖으로 나가는 애들을 잘라줘야하는데 카메라 공간 밖으로 잘리는 애들은 자르기 쉽지 않다. 이러한 규칙을 하나 정의하자. 이게 projection transform 그런 다음 물체를 자르자.
이렇게 자른 공간을 clip space라고 부름(왼손 법칙 따름)
하나의 projection plane에 ㅡ투영이 됨(가상의 개념)
투영을 하게 되면 길이가 다른 것도 같게 보인다
나중엔 3차원을 2차원으로 변형시키지 않고 3차원 그대로 유지시키며 투영의 효과 즉 원근법을 달성.
projection transform matrix는 행렬(복잡함) 하지만 초기에 정의해줬던 네 개의 파라미터로 다 표현 가능
실제론 실체가 없다
얘는 강체 변환이 아님. 외형 바뀜
vertex shader가 내보내는 공간의 법칙과 rasterizer가 원하는 공간의 법칙이 반대라면 아까 학습했떤 z 전환필요
-> 나중에 바꾸지 말고 이미 변환 행렬에서 z축에 관한 내용을 음수로 미리 바꾸면 안될까?
-> 세 번째 행만 음수로 바꿔주면 projection transform 행렬이 완성됨

스크린 좌표계가 왼손이다




***

## 👻 글을 마치며


***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (5장-정점 처리)](https://youtu.be/oGCydIALgJg)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   