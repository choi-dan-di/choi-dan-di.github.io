---
title: "[Computer Graphics] #13. 캐릭터 애니메이션"
excerpt: "한정현 컴퓨터 그래픽스 강의 13장 정리"

categories:
  - Computer Graphics
tags:
  - [Computer Graphics, graphics, cg, character animation, character, animation]

permalink: /computer-graphics/character-animation/

toc: true
toc_sticky: true

date: 2023-04-24 17:27:32+0900
last_modified_at: 2023-04-24 17:27:35+0900
published: true
---

## 👻 캐릭터 애니메이션
그래픽스에서 애니메이션을 적용할 가장 중요한 대상은 캐릭터이다. 특히 사람 캐릭터가 중요한데, 사실적이고 자연스러운 캐릭터 움직임을 만들어내기 위한 실시간 캐릭터 애니메이션의 기초적인 기법에 대해 알아볼 것이다.

***

## 👻 캐릭터 골격과 공간 이전
실시간 캐릭터 애니메이션을 위해서는 대체로 캐릭터의 **골격(Skeleton)**을 이용한다. 캐릭터의 골격은 다수의 **뼈(Bone)**로 구성된 **관절체(Articulated Body)**이다.

***

### 🌱 골격
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/skeleton1.png)   

위 이미지에서, (a)와 같은 폴리곤 메시가 제작되었을 때, 양팔을 벌린 기본 자세를 **드레스 포즈(Dress Pose)**라 한다. 

> 💡 **드레스 포즈**의 영문 동의어로 **Default Pose**, **Rest Pose**, **Bind Pose** 등이 있다.

대부분의 3차원 모델링 패키지는 사람 캐릭터에 적합한 기본적인 골격을 제공한다. 위 이미지의 (b)는 3ds Max에서 제공하는 **바이페드(Biped)**라는 기본 골격이다. 이는 편집 가능하며, 이러한 골격과 폴리곤 메시를 결합하는 과정이 필요하다. 일단 골격이 드레스 포즈에 맞춰지면(f), 각 뼈마다 하나의 **변환 행렬**이 자동으로 계산된다. 골격의 애니메이션을 표현하는 데 이 행렬들을 이용한다.

***

### 🌱 뼈와 공간 이전
골격을 구성하는 뼈들은 **계층적으로 부모-자식(Parent-Child) 관계**를 형성하며, 캐릭터 애니메이션에서는 관례적으로 **골반(Pelvis)**을 루트 노드로 정한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/skeleton2.png)   

뼈는 **관절(Joint)**로 연결되어 있다. 다음은 캐릭터 골격 중에서 세 개의 뼈(위팔, 아래팔, 손)와 세 개의 관절(어깨, 팔꿈치, 손목)을 보여주는데, 쉬운 이해를 위하여 2차원 그림을 사용하여 개념을 정리할 것이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/skeleton3.png)   

캐릭터 메시의 모든 정점이 정의되어 있는 공간을 **캐릭터 공간(Character Space)**이라 부른다. 위 그림에서 정점 v<sub>u</sub>, v<sub>f</sub>, v<sub>h</sub>는 각각 위팔, 아래팔, 손에 속한다. 한 뼈가 움직이면 그 뼈에 속한 정점도 움직이게 된다. 이를 구현하려면 v<sub>f</sub>를 예로 들었을 때 해당 정점이 아래팔의 **오브젝트 공간(Object Space)**에 속해야한다. 드레스 포즈에 골격이 맞춰지면, 캐릭터 공간의 각 정점은 자신이 속한 뼈의 오브젝트 공간으로 변환되어야 하는데, 이를 **뼈 공간(Bone Space)**이라 부른다. 캐릭터의 모든 정점은 뼈 공간을 기준으로 좌표를 가지게 된다.

우선, 드레스 포즈에 골격이 맞춰지면, 부모와 자식 뼈 사이의 상대적인 위치와 방향이 결정된다. 한 뼈에 속한 정점을 그 부모의 뼈 공간으로 변환하는 것을 **부모 변환(To-Parent Transform)**이라 한다. 아래팔의 부모 변환 행렬은 M<sub>f,p</sub>로 표기한다. 여기서 아래첨자 f는 아래팔의 forearm을, p는 parent를 의미한다. 이렇게 아래팔에서 위팔로의 **공간 이전(Space Change)**을 의미하는 변환 행렬은 위팔 좌표계를 아래팔 좌표계에 **포개는 행렬**과 같다.

골격을 구성하는 각 뼈는 고유의 길이를 갖고 있으며, 자신의 뼈 공간의 x축을 따라 놓이는 것이 관례이다. 이를 이용하여 변환 행렬을 쉽게 구할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/space1.png)   

위팔의 길이는 4이고, 아래팔의 정점 v<sub>f</sub>를 위팔 좌표계 기준으로 변경한다 생각하면 x축을 따라 4만큼 이동하는 행렬로 표현할 수 있다. 이를 정점에 적용하면 부모 변환이 완료된다.

손을 생각해보면 아래팔의 길이는 3이므로 x축으로 3만큼 이동하는 행렬이 부모 변환 행렬일 것이고, 여기에 위팔로 공간 이전을 하면 방금 구한 행렬을 다시 한 번 더 적용할 수 있다. 이렇게 여러 행렬이 결합되면 손에 속하는 정점을 **조부모**인 위팔 공간까지도 변환이 가능하다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/space2.png)   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/space3.png)   
![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/space4.png)   

***

### 🌱 캐릭터 공간에서 뼈 공간으로의 변환
각각의 뼈 공간에서 캐릭터 공간으로의 공간 이전 행렬을 M<sub>i,d</sub>라고 하자. 여기서 i는 뼈의 일련번호를, d는 드레스 포즈를 의미한다. 위에서 적용해 본 변환 행렬을 사용하면 최종 루트 노드인 골반까지 모든 뼈 공간을 이전할 수 있다. 골반 공간이 캐릭터 공간과 동일할 때, M<sub>1,d</sub>는 **단위 행렬(I)**이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/bone1.png)   

부모-자식 관계는 계층 구조를 가지기 때문에 각 뼈 공간의 캐릭터 공간까지의 변환 행렬은 다음과 같이 정의할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/bone2.png)   

이해하기 쉽게 그림으로 나타내면 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/bone3.png)   

뼈 공간에서 캐릭터 공간으로의 변환은 정의하였지만, 관절체 애니메이션에서 필요한 것은 그 **역변환**인 M<sub>i,d</sub><sup>-1</sup>이다. 역변환은 아래와 같다. 관절을 움직이기 위해선 자유롭게 해당 관절로의 접근이 필요하기 때문이다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/bone4.png)   

역변환의 관계는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/bone5.png)   

> 💡 **요약하자면,**   
드레스 포즈 골격이 주어지면, 각 뼈의 부모 변환 행렬 M<sub>i,p</sub>가 즉시 결정되며 이는 그의 역행렬이 즉시 결정된다는 뜻이기도 하다. 루트 노드인 골반 공간의 변환 행렬은 단위 행렬이므로 이를 이용하여 계층 구조를 따라 **위에서 아래로 내려가면서** 모든 뼈의 역행렬을 구할 수 있다.

***

## 👻 정기구학
앞에서 본 역행렬 M<sub>i,d</sub><sup>-1</sup>은 드레스 포즈 골격의 캐릭터 공간 정점을 i번째 뼈 공간으로 변환한다. 이제 관절체 애니메이션을 생각해 보자. 아래의 그림을 예로 들면, v<sub>5</sub>는 원래 캐릭터 공간에 정의되었지만, M<sub>5,d</sub><sup>-1</sup>에 의해 변환되어 아래팔 공간에서 (2, 0)의 좌표를 가지게 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/kinematics1.png)   

여기서 아래팔이 회전하면 정점 v<sub>5</sub>도 회전할 것이다. 애니메이션이 완료된 정점을 렌더링에 사용하기 위해서는 **이를 다시 캐릭터 공간으로 변환**해야 한다. 이렇게 해야 애니메이션 포즈의 캐릭터 전체가 GPU 파이프라인을 따라 월드 변환, 뷰 변환 등을 거쳐 최종적으로 스크린에 렌더링될 것이다. 여기서 다음과 같은 과정을 거치는 변환 행렬이 필요하다.

> 1. v<sub>5</sub>에 애니메이션을 적용
2. 결과를 캐릭터 공간으로 옮기는 변환 적용

이는 M<sub>5,a</sub>로 표기할 수 있다. 여기에서 a는 animation을 의미한다.

위 그림에서 아래팔은 90˚ 회정했다. 이는 아래팔의 뼈 공간의 원점인 **팔꿈치**를 중심으로 한 회전이다. 이러한 지역적인(local) 특성을 반영하여 이를 **지역 변환(Local Transform)**이라 부른다. 두 번째 아래첨자 l을 사용하여 M<sub>5,l</sub>와 같이 표기 가능하다. 2차원 동차 좌표계에서의 회전 행렬을 이용하면 지역 변환 행렬은 다음과 같이 구할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/local1.png)   

이를 정점 v<sub>5</sub>에 적용하면 변환 후의 좌표값을 구할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/local2.png)   

이렇게 애니메이션이 적용된 정점은 캐릭터 공간으로 변환되어야 하므로 우선 부모 변환이 수행되어야 한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/local3.png)   

즉, 지역 변환을 이은 부모 변환이 바로 애니메이션 변환이 된다. 부모-자식 관계의 계층 구조를 이용하면 다음과 같이 i번째 뼈 공간에서의 애니메이션 변환을 정의할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/animation1.png)   

계층 구조를 그림으로 나타내면 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/animation2.png)   

역시나 루트 노드인 골반 공간의 애니메이션 변환 행렬은 단위 행렬임을 알 수 있고, 이를 이용하여 위에서 아래로 내려가면 모든 애니메이션 변환을 계산할 수 있다.

> 💡 **요약하자면,**   
**드레스 포즈** 골격이 결정되면, 골격 계층 구조를 따라 **위에서 아래로 내려가면서** 각 뼈마다 M<sub>i,d</sub><sup>-1</sup>을 계산한다. 이는 단 한 번 계산된다. 한편, **애니메이션 포즈**가 결정되면, 이 역시 골격 계층 구조를 따라 **위에서 아래로 내려가면서** 각 뼈마다 M<sub>i,a</sub>를 계산한다. 이는 모든 애니메이션 포즈에서, 즉 애니메이션의 **각 프레임마다 반복**된다.

드레스 포즈에서 정점의 캐릭터 공간 좌표를 v<sub>d</sub>로 표기할 때, 이는 M<sub>i,d</sub><sup>-1</sup>에 의해 해당 뼈 공간으로 변환되고, 이후 M<sub>i,a</sub>에 의해 애니메이션 된 후 다시 캐릭터 공간으로 변환된다. 이 정점을 v<sub>a</sub>라 표기하면 다음과 같이 정의할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/animation3.png)   

이렇게 얻어진 정점들을 모다 렌더링하면 **애니메이션 포즈의 캐릭터**를 얻게 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/animation4.png)   

**기구학(Kinematics)**은 **질량이나 힘을 고려하지 않고 물체의 움직임을 기술하는 학문 분야**이다.

> 💡 **동역학(Dynamics)**은 **움직임을 만들어 내는 데 필요한 힘(Force)**에 대해 다룬다.

앞서 보았던 것처럼 **모든 뼈의 변환을 계산하여 관절체 전체의 모습을 결정하는 것**을 **정기구학(Forward Kinematics 혹은 Direct Kinematics)**이라고 부른다. 이의 반대는 **역기구학(Inverse Kinematics)**이라고 부르며, 이는 **관절체의 말단 노드**의 위치와 방향이 주어졌을 때 이를 위하여 **관절체의 뼈에 어떠한 변환을 적용해야 하는 지 결정**한다.

***

## 👻 스키닝
캐릭터의 폴리곤 메시를 **피부(Skin)**라고도 부르는데, 골격 움직임에 따라 어떻게 피부를 부드럽게 변형하는지 알아볼 것이다.

***

### 🌱 정점 블렌딩
이제까지 한 정점은 오직 하나의 뼈에 속한다고 가정했다. 다음은 팔꿈치에 연결된 위팔과 아래팔을 보여준다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/blending1.png)   

팔꿈치 주위의 세 정점 a, b, c가 있다고 가정할 때, a는 위팔, b와 c는 아래팔에 속한다고 볼 수 있다. 여기서 팔꿈치를 회전하면 각 정점은 오른쪽 그림과 같이 변환될 것이다. 피부가 부드럽지 않게 변형된 것을 확인할 수 있다.

한 정점은 오직 하나의 뼈에 속한다는 제약 때문에 이러한 문제가 발생한 것인데, **여러 개의 뼈가 한 정점에 영향**을 주도록 하고 그 결과를 **블렌딩(Blending)**하면 이 문제는 상당히 완화될 수 있을 것이다. 이를 위해서는 **각 뼈가 한 정점에 얼마나 영향을 주는지** 미리 정해줘야 하는데, 이를 **블렌딩 가중치(Blending Weight)** 혹은 간단히 **가중치**라 부른다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/blending2.png)   

위 이미지는 위팔과 아래팔에 각각의 가중치를 추가하여 변형을 적용한 모습이다. a의 경우 위팔은 70%, 아래팔은 30% 정도의 가중치를 가지며 이는 a가 위팔에 더 많은 영향을 받는다는 의미이다. 이렇게 하나의 정점이 두 개 이상의 뼈 공간의 영향을 받게 되면 각각의 뼈 공간으로 변환되어 애니메이션 포즈를 모두 적용하게 된다.

> 💡 정점의 이동은 **캐릭터 공간** 👉 **뼈 공간** 👉 **캐릭터 공간**이며, 이는 블렌딩 가중치가 적용된 모든 뼈 공간에서 개별적으로 수행된다.

이렇게 되면 다시 캐릭터 공간으로 돌아온 정점 a는 위팔과 아래팔 기준 각각 두 개의 위치로 변환된다. 여기서 이 가중치를 이용해 **선형 보간**된다. 이 결과로 생성된 피부는 이전보다 훨씬 부드럽게 표현되며 이러한 기법을 **스키닝(Skinning)** 혹은 **정점 블렌딩(Vertex Blending)**이라 부른다.

일반적으로, 한 정점에 영향을 주는 뼈들과 그 가중치는 애니메이션 과정 전체에 걸쳐 일정하게 유지되는데, **m개의 뼈**가 한 정점에 영향을 준다고 가정하자(게임과 같은 실시간 그래픽스 응용에서는 대체로 m은 4로 고정된다. 물론 m개의 뼈의 가중치 합은 1이다). 그러면 블렌딩 가중치를 이용하여 애니메이션 포즈가 적용된 정점은 다음과 같이 일반화된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/blending3.png)   

M<sub>i,a</sub>M<sub>i,d</sub><sup>-1</sup>을 간단히 M<sub>i</sub>로 표기하면 다음과 같이 정리된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/blending4.png)   

이를 이용하여 각 뼈마다 애니메이션 포즈를 정하면 된다. 애니메이션 포즈는 매 프레임의 각각의 뼈마다 적용이 된다고 했었다. 즉, 20개의 뼈를 가지는 캐릭터라면 애니메이션 과정의 매 프레임마다 20개의 M<sub>i</sub>를 갱신해야 한다. 이는 **행렬 팔레트(Matrix Pallette)**라 불리는 테이블에 저장된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/skinning1.png)   

위의 그림에서 하나의 정점은 4개의 뼈에 의해 영향을 받게 된다. 팔레트 인덱스는 뼈의 인덱스를 의미하며 총 네 개의 행렬과 연산을 수행하여 블렌딩된다.

일반적으로 스키닝 알고리즘은 정점 쉐이더로 구현된다. 정점 쉐이더에게 행렬 팔레트는 **유니폼**으로 제공된다. 반면, 팔레트 인덱스와 블렌딩 가중치는 정점 위치, 노멀, 텍스처 좌표 등과 함께 정점 배열에 저장되어 정점 쉐이더에게 **애트리뷰트**로 제공된다.

***

### 🌱 키프레임 애니메이션에서의 스키닝
스키닝을 키프레임 애니메이션에 적용한다고 생각해보자. 애니메이션 포즈는 **키프레임**에서만 정의된다. 이러한 키프레임에서 정의된 M<sub>i,a</sub>를 보간하여 **중간 프레임**에 사용해야 한다.

사람의 관절은 회전만 가능하기 때문에 특별한 경우가 아니라면 지역 변환은 회전 행렬로 국한된다. 부모 변환은 회전과 이동이 결합된 공간 이전 행렬임을 알 수 있고, 따라서 M<sub>i,p</sub>M<sub>i,l</sub>은 **강체 변환**임을 알 수 있다. 키프레임에서 각각의 뼈에는 R이 쿼터니언으로 변환되어 저장되고, t는 3차원 벡터로 저장된다. 이것이 바로 뼈의 키 데이터를 구성한다. 20개의 뼈를 가진 캐릭터라면, 하나의 키프레임은 쿼터니언과 t의 쌍을 20개 가진다.

다음은 키프레임 기반 스키닝 알고리즘의 슈도코드이다.

```ini
for each bone   // default pose
    compute Md-

for each frame
    for each bone i // animated pose
        interpolate key data
        compute Ma
        combine Ma with Md- to define Mi
        store Mi in the matrix palette
    invoke vertex shader for skinning
```

위 코드는 정식적인 코드가 아닌, 코드의 흐름만을 보여주기 위해 만들어진 코드이다. 해당 슈도코드를 통해 키프레임 애니메이션에서의 스키닝 알고리즘이 어떤식의 흐름으로 진행되는지 알 수 있다.

***

## 👻 역기구학
로봇 팔의 부착되어 물체를 잡고 다루는 등의 용도로 쓰이는 기구를 **말단 장치(End Effector)**라 한다. 사람으로 비유하면 손과 발, 머리와 같은 끝부분을 의미한다. 앞서 보았던 정기구학은 로봇 팔의 관절 각도를 입력으로 받아 말단 장치의 위치 및 방향을 결정한다. 이러한 작업의 역(逆)순을 **역기구학(Inverse Kinematics; 이하 IK로 약칭)**이라 한다. 이는 목표로 하는 말단 장치의 위치 및 방향이 입력으로 주어지면, 이 목표를 달성하기 위해 **필요한 각 관절의 각도를 계산**한다.

로봇 분야에서는 미분 방정식을 풀어서 IK 해를 구하지만 이는 대체로 복잡한 계산 과정을 거치고 연산 시간이 많이 든다는 단점이 있다. 실시간 응용 분야에서는 이를 대신하는 간단한 알고리즘이 널리 사용되는데, 그 중 두 가지 기법에 대해 알아보자.

***

### 🌱 해석적 기법
물체의 상태를 정의하는 독립적인 변수의 개수를 **자유도(Degrees of Freedom; DOF)**라 부른다. 로봇 팔꿈치를 예로 들면, 기구학적으로 경첩(hinge)과 같아서, 아래팔을 위아래로 움직이는 자유도만 가지게 된다. 이는 **1 자유도 관절**이라 한다. 반면, 인간의 어깨는 기구학적으로 볼 조인트(ball joint)와 같은데, 위팔이 특정한 방향을 가리키려면 좌우로 한 번 움직이고 위아래로 한 번 움직이면 된다(이를 영문으로는 각각 yaw와 pitch라고 한다). 이는 자유도가 2이다. 그런데, 위팔은 스크루 드라이버처럼 회전될 수 있다(이를 영문으로는 roll이라고 한다). 이는 자유도가 하나 증가한 3이 된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/ik1.png)   

IK를 위해 **해석적(Analytic) 기법**을 사용할 수 있는데, 아래의 그림 (a)와 같이 두 개의 관절을 가진 로봇 팔을 예로 들어보자. 아래팔의 끝점 T가 G로 표기된 목표 위치에 닿을 수 있도록 하려면 위팔과 아래팔이 회전되어야 한다. IK는 이렇게 위팔과 아래팔을 회전시키는데 필요한 **어깨와 팔꿈치의 관절각(Joint Angle)**을 계산한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/ik2.png)   

1자유도 관절인 팔꿈치의 관절각은 간단히 계산된다. 위 그림의 (b)를 보면 **코사인 법칙**을 적용하여 회전각 θ를 구할 수 있다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/ik3.png)   

위 식에서 θ는 다음과 같이 계산된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/ik4.png)   

이 계산이 끝나면 (c) 그림의 결과를 얻을 수 있다. 이제 어깨를 Φ만큼 움직여야하는데, 이는 벡터 v<sub>1</sub>과 v<sub>2</sub>의 **내적**을 통해 계산된다. 3자유도를 가진 어깨의 경우 회전각 하나로는 충분하지 않고 회전축이 필요하다(3차원 회전과 비슷하다고 볼 수 있다). 이는 v<sub>1</sub>과 v<sub>2</sub>에 모두 수직이어야 하므로 이들의 **벡터곱**을 통해 결정된다. 이렇게 구해진 회전축을 중심으로 위팔이 Φ만큼 회전하면 T는 G에 닿게 된다.

***

### 🌱 CCD 알고리즘
간단한 관절체의 경우 해석적 기법을 사용하면 빠르고 정확하게 IK를 해결할 수 있지만, 많은 관절을 가진 복잡한 관절체에서는 이것을 사용하기 어렵다. 대안으로 종종 **CCD(Cyclic Coordinate Descent) 알고리즘**이 채택된다. 이 기법은 **말단 장치로부터 시작해 계층 구조를 거슬러 올라가면서, 말단 장치와 목표 지점 간 거리가 최소화되도록 관절각을 조정**한다. 모든 관절각 조정을 마쳤는데도 말단 장치와 목표 지점 간 거리가 임계값 이상 벌어지면, 말단 장치로부터 시작해 관절각을 조정하는 작업을 반복한다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/ccd1.png)   

위의 그림은 위팔, 아래팔, 손으로 이루어진 골격을 이용해 CCD 작동 원리를 설명한다. 최종 목표는 T와 G를 같게 하는 것이다. CCD는 **말단 장치인 손부터 움직이는데**, T가 G를 가리키도록 회전시킨다. 이러한 회전이 완료되면 아래팔을 회전시키고, 점차적으로 올라가면서 하나의 사이클을 완료하게 된다. 여기서도 물론 벡터곱을 이용하여 회전축을, 내적을 이용하여 회전각을 구할 수 있다.

T와 G간 거리를 계산해서 그 거리가 **충분히 가까우면** CCD 알고리즘은 **종료**되지만, 아니라면 말단 장치인 손에서 시작해 동일한 작업을 반복한다. 이러한 반복은 T와 G간 거리가 **미리 정한 인계값 이하**가 되거나 혹은 **미리 설정한 최대 반복 횟수에 도달**했을 때 중단된다.

![Alt Text](/assets/images/posts_img/basics/computer-graphics/character-animation/ik5.png)   

게임에서는 자연스러운 캐릭터 애니메이션을 위해 IK를 자주 사용한다. 위의 이미지는 캐릭터의 머리 뼈와 팔에 IK가 적용되어 날아가는 공을 응시하고 공을 향해 손을 뻗는 애니메이션을 보여준다.

***

## 👻 글을 마치며
이번 시간에는 캐릭터 애니메이션에 대해 알아보았다. 매 프레임마다 수시로 변환이 된다는 점이 살짝 놀라웠다(더불어 더 좋은 컴퓨터를 사고 싶다는 생각이...). 언리얼 엔진을 한 번 다뤄보고 들어서 그런지 강의를 들으면서 익숙한 단어가 나올 때마다 반가웠던 것 같다. 그리고 더불어 엔진을 사용하면서 IK는 무슨 뜻인지 궁금했었는데 이번 시간에 알게되어서 좋았던 것 같다. 게임 개발에 도움이 많이 되는 것 같은 느낌적인 느낌!

***

_출처_   
_[한정현 컴퓨터 그래픽스 강의 (13장-캐릭터 애니메이션)](https://youtu.be/aZcrHIDO5zY)_   
_[도서 관련 예제 GitHub](https://github.com/medialab-ku/openGLESbook)_   
_[PPT 강의 자료 및 사진 출처](https://media.korea.ac.kr/books/)_

_관련 도서_   
_OpenGL ES를 이용한 3차원 컴퓨터 그래픽스 입문 - 한정현 지음_   