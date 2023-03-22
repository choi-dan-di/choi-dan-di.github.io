---
title: "[Computer Graphics] #9. 라이팅"
excerpt: "한정현 컴퓨터 그래픽스 강의 9장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, lighting]

permalink: /computer-graphics/lighting/

toc: true
toc_sticky: true

date: 2023-03-21 19:27:38+0900
last_modified_at: 2023-03-21 19:27:40+0900
published: false
---

## 👻 라이팅
Phong Lighting Model
라이팅은 빛과 물체의 상호작용을 다루는 것. 물리학의 고전적인 주제
실시간 그래픽스에 적용할 모델 중 가장 간단한 모델은 퐁 모델이다.
이 모델을 만든 사람이 퐁(베트남 사람) 70년대 초반에 만들어짐. 변형은 되지만 지금까지 쓰이고 있다. 유타대학교?
네 가지 항으로 나눠짐
- diffuse
- specular
- ambient
- emissive

Diffuse Term
광원은 여러가지 종류가 있다. led 같은 광원은 처리하기 복잡함
처리하기 쉬운 광원이 directional light source 쉽게 말해 해 같은 광원. 빛이 세고 물체와 멀리 떨어져있다.
아주 멀리 있기 때문에 물체에 들어오는 광원은 평행하다고 볼 수 있다. -> 계산을 편리하게 해줌
방향성 광원의 특징 : 장면을 구성하는 물체의 모든 점에 입사하는 빛은 같은 방향을 가진다
diffuse reflection : 난반사
입사한 빛에 한해 모든 방향으로 반사됨
어떤 색을 본다는 것은 그 물체에 반사된 빛을 보는것. 난반사를 이용하면 물체에 대한 색상은 어디서 봐도 동일할 것이다.
이러한 표면을 diffuse 표면 혹은 lambertian 표면이라고 한다 (동일한 강도로 빛이 반사됨-> 같은 색을 띰)
빛을 향한 벡터:l
법선 벡터는 n
입사각의 각도가 p가 받는 빛의 양을 결정함
p가 언제 가장 많이 빛을 바등ㄹ까? 입사각이 0도일 때 (최대)
입사각이 90도일 때 빛을 전혀 받지 못함(최소)
입사각과 빛의 양은 반비례 => cos 세타
n과 l을 내적하면 nlcos세타. n과 l의 길이가 1일 때 내적값이 p가 받는 빛의 양을 결정해준다
하지만 값이 음수가 되면 안 되므로 max(n닷l, 0)로 결정해준다
빛의 색상 결정하기
rgb가 111일 때 (흰색) 어떤 물체가 빨간색으로 보인다면 gb는 흡수, r만 반사한다는 의미
즉 반사 계수를 (1, 0, 0)으로 설정
s: light source
m: material
쌍쌍이 곱한다.

Specular Term
정반사
법선 벡터를 기준으로 동일한 반사각을 가지는 반사
카메라가 어느 위치에 있느냐에 따라 정반사를 볼 수도 있고 못 볼 수도 잇음
reflection vector:r
카메라 벡터 :view vector:v
r과 v의 관계
같다면 정반사를 정확히 캡처
카메라의 위치에 따라 좌지우지됨 의존적임
반드시 일치하는 게 아닌 정반사를 볼 수 있는 범위가 있음
로의 각이 커질수록 정반사를 볼 수 있는 범위가 기하급수적으로 좁아지려면
cos로의 제곱을 원하는 결과가 나올 때까지 하면 된다
로의 지수는 sh. 로는 r닷v로 표기가능
sh:shininess 얼마나 물체가 매끈한가!
역시 음수를 막기 위해 (max(r닷v, 0))^sh
난반사와 동일한 방식으로 색상까지 고려해야한다.
sh가 커질수록 더 매끈한 표면을 나타낼 수 있따
매끈할수록 지수가 커질것이다
sd와 ss는 같다. md는 물체에서 가져오고 ms는 gray-scale 값임. (gray-scale : r==g==b)최대 111, 최소 000
왜 정반사는 gray-scale이어야될까
물체 고유의 색은 죽이고 원래 라이트 소스의 색만 내보내줌
(약간 이해안감..)

Ambient and Emissive Terms
Ambient : light source가 어디있는지 특정할 순 없지만 어디선가 들어오는
마치 불 끄고 시간 좀 지나면 보이는 것 같은
배경광반사
특정한 l벡터가 필요없음
노멀 벡터도 필요없음(기준이 될 벡터가 필요가없다)
단순하게 sa곱ma(쌍)
대부분 sa는 엄청 작은 빛
음영 변화가 없는 대체로 어두운 색이 나옴
Emissive : 스스로 빛을 내는 발광체에 해당
me는 rgb

Phong Lighting Model
퐁 모델은 위에서 본 네 가지 값들을 모두 더한 것

Per-fragment Lighting
directional light source도 유니폼(l벡터)
n: v_normal(래스터라이저가 줄 거임)
v: 얘도 정점마다 다르므로 래스터라이저가 줘야함
r:n과 l이 주어지면 게산 가능(자체 계산)
라이팅 계산은 프래그먼트 쉐이더가 수행할것임
맨 처음 노멀은 os에서 정의. 
l: ws(월드 공간)
n, r, v도 월드 공간 벡터여야함
fs는 반드시 월드공간 노멀을 받아야함
반드시 vs가 노멀을 ws로 변환해서 넘겨줘야함
(뷰벡터가 이해가 안가는디? 카메라는 월드 공간에서 정의된다-> 내가 해줘야함!!!)
r = 2n(n닷l)-l

~코드 분석~

<질문 타임>
결과값(d+s+a+e)이 넘어가게 되면 1로 고정시킨다
맘에 안들면 변경해서 다시 렌더링

중간에 보간된 노멀 벡터들은 정규화되어있지 않을 가능성이 있다. 그래서 한 번 더 정규화해주는 것




***

## 👻 글을 마치며


***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (9장-라이팅)](https://youtu.be/_uIjVpAM9l8)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   