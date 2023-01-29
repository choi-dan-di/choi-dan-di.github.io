---
title: "[3D Game Training] #11. 몬스터 AI 구현하기"
excerpt: "Behavior Tree를 이용해 몬스터에 인공지능 부여해보기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, animation, blueprint, unreal, ai, artificial intelligent, behavior tree, blackboard]

permalink: /3d-game-training/behavior-tree/

toc: true
toc_sticky: true

date: 2023-01-24 18:40:22+0900
last_modified_at: 2023-01-24 18:40:24+0900
---

## 👻 몬스터 AI 구현하기
플레이어는 어느 정도 마무리가 되었으니 이제 몬스터를 만들어보자. ``` Behavior Tree ```를 이용해 **인공지능(AI)**을 부여해 자동으로 움직이다 일정 거리 안에 적(플레이어)이 있으면 쫓아가는 기능을 구현해 볼 것이다. 우선 ``` BP_Monster ```라는 이름으로 블루프린트 클래스를 하나 만들어주고, ``` AIController ```를 상속받은 컨트롤러를 연결해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/event-on-possess.PNG)   

몬스터 클래스에 컨트롤러가 연결될 때 ``` Behavior Tree ```를 실행하도록 구현하였다.

***

### 🌱 Behavior Tree
![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/create-behavior-tree.PNG)   

우클릭하여 ``` Behavior Tree ```와 ``` Blackboard ```를 만들어주었다. **비헤이비어 트리(Behavior Tree)**는 말 그대로 **행동 트리**라는 것이고 인공지능의 행동 알고리즘의 구현을 도와준다. **블랙보드(Blackboard)**는 인공지능의 결정에 필요한 정보들을 담아둔 저장소이다.

비헤이비어 트리는 좌측부터 우측으로 실행 순서가 정해져있으며 노드 좌측 상단의 번호로 실행 순서를 알 수 있다. 이 트리를 이용하여 몬스터가 랜덤으로 움직이도록 만들 것이다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/composites-tasks.PNG)   

비헤이비어 트리에는 ``` Composite ```와 ``` Task ```를 추가할 수 있다. **Composite**는 **합성물, 복합물** 등의 뜻을 가진다. C++에서 **If, For** 등과 비슷한 의미를 가지며 해당 노드에 여러개의 조건(서비스, 데코레이터 등)을 추가하여 사용할 수 있다.

> 💡 **Composites**   
- ``` Selector ``` : 자식 노드의 좌측에서 우측으로 실행하며 하나의 자식 노드라도 성공하면 실행을 멈춘다. 하나의 자식 노드가 성공하면 셀렉터도 성공한 것이고, 모든 자식 노드가 실행을 실패하면 셀렉터도 실패한다. (OR)
- ``` Sequence ``` : 자식 노드의 좌측에서 우측으로 실행하며 하나의 자식 노드라도 실패하면 실행을 멈춘다. 하나의 자식 노드가 실패하면 시퀀스도 실패한 것이고, 모든 자식 노드가 성공하면 시퀀스도 성공한다. (AND)
- ``` Simple Parallel ``` : 단순 병렬 노드. 하나의 단일 작업 노드(선택적 데코레이터 포함)여야 하고 다른 하나는 완전한 하위 트리일 수 있다. "A를 수행하는 동안 B도 수행"

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/patrol-tree.PNG)   

위 비헤이비어 트리는 순찰을 돌 위치를 랜덤으로 찾아 몬스터를 이동시키는 행동을 한다. 3개의 태스크를 시퀀스가 실패할 때까지 반복 실행하며 순찰을 도는 것처럼 보이게 된다. ``` Find Patrol Location ``` 태스크는 커스텀 태스크이다.

> **비헤이비어 트리 상단에서** ``` New Task ```를 눌러 태스크를 새로 추가할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/find-patrol-location.PNG)   

``` Event Receive Execute AI ```를 이용해 인공지능의 행동을 설정할 수 있다. 현재 몬스터가 있는 위치의 반경 ``` Radius ``` 값 만큼을 이동 범위로 잡고 랜덤 위치를 반환한다. 블랙보드에 값을 넘겨줘서 비헤이비어 트리에서 사용할 수 있도록 해준다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/blackboard-key-selector.PNG)   

``` Blackboard Key Selector ```라는 변수 타입으로 블랙보드의 키값을 사용할 수 있다. 외부(BT, BB)에서 사용해야 하기 때문에 변수는 눈 모양을 뜨게 만들어 ``` Public ```으로 만들어줘야한다.

- **Blackboard**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/bb-in-bb.PNG)   

- **Behavior Tree**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/bb-in-bt.PNG)   

> 💡 **Navigation Mesh**   
해당 물체가 갈 수 있는 곳을 지정해주는 메쉬이다. 복잡한 길이면 길찾기 알고리즘을 이용하여 이동 행동을 자연스레 설정할 수 있다.   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/nav-mesh.PNG)   
<span style="font-size: 0.7rem; color: gray;">Volumes의 NavMeshBoundsVolume을 이용하여 이동 가능한 곳을 설정한 모습. 단축키 P로 Show 유무를 설정할 수 있다.</span>

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/move-to-result.gif)   
<span style="font-size: 0.7rem; color: gray;">Wait 1s 👉 Find Patrol Location 👉 Move To Patrol Location</span>

***

#### 🪐 Service
해당 분기문이 실행되는 동안 콜백을 등록하여 **주기적으로 발생**해야 하는 다양한 종류의 업데이트를 수행하는 기능이다. 서비스가 추가된 **Composite** 노드의 하위 트리에 실행이 남아있는 한 서비스는 활성 상태이다. 커스텀 서비스는 상단 ``` New Service ```를 이용해 만들어 적용 시킬 노드를 우클릭 해 추가할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/add-service.PNG)   

서비스를 이용해 몬스터 입장에서 타겟을 찾는 기능을 1초마다 콜백해 볼 것이다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/find-target.PNG)   

``` Event Receive Tick AI ``` 노드를 이용해 반복적으로 실행되는 코드를 구현하였다. 몬스터의 위치와 타겟(플레이어)의 위치를 구해 차이를 이용하여 일정 거리(Range)보다 가까이 있으면 블랙보드의 변수에 타겟을 세팅해주었다.

> 💡 **서비스의 실행 주기는 비헤이비어 트리 내에서 가능하다.**   
- ``` Interval ``` : 콜백 주기(Tick)
- ``` Random Deviation ``` : 불규칙 비율. 규칙적으로 콜백하고 싶다면 0으로 설정

***

#### 🪐 Decorator
조건부에 해당하는 기능이다. **Composite** 노드에 붙여 사용할 수 있고 여러 개의 데코레이터를 같은 노드에서 사용할 수 있다. 앞에서 만들었던 서비스의 결과값을 기준으로 분기문을 조절하기 위해 데코레이터를 사용해보자. 일정 거리 내에 적이 있으면 블랙보드의 키 ``` TargetEnemy ```에 값이 존재할 것이고 적이 없으면 존재하지 않을 것이기 때문에 **블랙보드 데코레이터**를 이용해보자.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/add-decorator.PNG)   

``` Add Decorator ```에 블랙보드가 이미 있으므로 여기선 바로 추가할 수 있지만 커스텀의 경우 상단에서 ``` New Decorator ```를 이용하여 먼저 만들어줘야한다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/service-decorator.PNG)   

블랙보드 데코레이터를 추가해주고 나면 **블랙보드 키와 성립 조건**을 설정해줘야한다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/service-decorator-result.gif)   

이제 커스텀 데코레이터를 만들어보자.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/custom-decorator.PNG)   

커스텀 데코레이터로 타겟이 사거리에 들어왔을 때를 체크하는 기능을 만들었다. 이 조건이 성립하면 몬스터가 공격을 하거나 기타 행동을 할 수 있을 것이다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/check-ai-function.PNG)   

커스텀 데코레이터는 ``` Perform Condition Check AI ``` 함수를 재정의하여 사용한다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/condition-check-ai.PNG)   

역시나 몬스터의 위치와 타겟의 위치의 차이를 이용해 Boolean 값을 반환한다. 이렇게 되면 타겟이 사거리 내에 들어온다는 조건이 성립하지 않을 때까지 무한 반복하며 5초를 기다릴 것이다. 

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/behavior-tree/result-result.gif)   
<span style="font-size: 0.7rem; color: gray;">아까완 다르게 움직여도 그 즉시 따라오지 않는다.</span>

***

## 👻 글을 마치며
이번 시간에는 몬스터 AI 기능을 비헤이비어 트리를 이용하여 구현해보았다. 생각보다 어려워서 복습을 오래 한 것 같다. 그래도 수업을 들으며 따라할 때와 혼자 복기하며 코드 리뷰를 하는 것의 차이가 큰 것 같다. 코드 리뷰를 하거나 개인적으로 직접 만들어보지 않으면 금세 까먹을 것 같다. 😅 복습은 꼭 하는 걸로!

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/AXLS)_