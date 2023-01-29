---
title: "[Paper2D Training] #5. 몬스터 생성 및 전투"
excerpt: "몬스터를 인게임에 생성한 후 전투 기능 구현해보기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, create monster, monster, fight]

permalink: /paper2d-training/monster-fight/

toc: true
toc_sticky: true

date: 2023-01-08 20:32:31+0900
last_modified_at: 2023-01-08 20:32:35+0900
---

## 👻 들어가기에 앞서
몬스터 아트 리소스를 다운 받아 파일을 추가해주고 캐릭터와 동일하게 작업해주며 플립북까지 완성하였다. 이제 캐릭터를 생성할 때처럼 몬스터도 생성해줘야하는데, 몬스터와 캐릭터는 체력이나 데미지 등 겹치는 부분이 많기 때문에 동일한 부모 클래스의 상속을 받아 만들어 관리하는 것이 더욱 편리하다. 그래서 ``` Creature 👉 Monster 👉 Skeleton ```, ``` Creature 👉 Player 👉 Knight ``` 이렇게 두 가지의 상속 관계를 지정해주기 위해 파일을 좀 정리해주었다.

공통적으로 포함되는 ``` Update Animation ``` 이나 ``` Update Logic ``` 부분은 부모 클래스에 남겨두고 나머지는 각 입맛에 맞게 개발할 수 있도록 삭제해주거나 변경해주었다. ~~(자세히는 코드 참고)~~

***

### 🌱 DataTable
인게임에 출현하는 캐릭터가 많아질수록 관리하기 힘들다. 이 때 **DataTable**을 사용하면 데이터로 변환해 아주 많은 정보를 쉽게 관리할 수 있다.

우선 구조체를 하나 만들어서 테이블의 **열(Column)**에 해당하는 목록을 지정해준다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/data.PNG)   

``` Update Animation ``` 함수에서 사용되어지는 플립북을 기사면 기사, 몬스터면 몬스터가 나올 수 있도록 설정해주기 위해 변경되는 부분을 열로 추가해주었다.

그 다음, ``` Miscellaneous 👉 Data Table ```을 클릭하여 데이터 테이블을 하나 생성해주고 데이터 설정까지 완료하였다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/datatable.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/datatable2.PNG)   

블루프린트에서 값을 가져오기 위해선 ``` Get DataTable Row ``` 노드를 사용한다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/get-datatable-row.PNG)   

이 노드를 이용해 부모 클래스인 ``` Creature ```의 ``` Update Animation ``` 함수를 해당 데이터로 변경해주면 된다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/connect-data.PNG)   

> 💡 그 전에 ``` Knight ```와 ``` Skeleton ``` 클래스 내에서 데이터 세팅을 먼저 해줘야 한다.

***

## 👻 피격 판정 추가하기
몬스터와 전투하기 위해 피격 판정을 추가해보자. **수학적 공식**을 사용하거나 **충돌** 등을 이용해서 적용할 수 있다.

***

### 🌱 Box Collision
우선 Box Collision을 이용해서 피격 범위를 설정해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/box-collision.PNG)   

이렇게 해주면 해당 범위 내에 어떤 액터가 들어오는지 판정하고 이벤트를 추가할 수 있다.

***

### 🌱 Process Attack
공격을 시도하고 해당 범위내에 피격될 만한 액터가 있는지 판단 후 데미지를 계산해주는 함수이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/process-attack.PNG)   

각 피격 범위에 들어온 액터를 확인하기 위해 ``` Get Overlapping Actors ``` 노드를 사용해주었다. 겹치는 액터가 있으면 찾아주는 함수이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/process-attack2.PNG)   

그런 다음 **자기 자신을 제외한** 액터가 피격 범위에 들어오면 데미지만큼 체력을 깎아준다. 참고로 ``` On Damaged ``` 함수는 다음과 같다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/on-damaged.PNG)   

> ``` Hp ```와 ``` Damage ```를 **Data Table**에 추가해주고 시작 전 ``` Hp ```와 ``` MaxHp ```를 **Data의 Hp**로 세팅해주었다.

- **결과**

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/attack.gif)   

***

## 👻 체력바 추가하기
피격할 때마다 깎이는 체력을 표시하기 위해 체력바를 추가해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/widget-blueprint.PNG)   

> ``` 우클릭 👉 User Interface 👉 Widget Blueprint ```

그 다음 간단하게 ``` Progress Bar ```를 이용하여 체력바를 만들어 주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/progress-bar.PNG)   

다시 ``` Creature ``` 상세 페이지로 돌아와서 방금 만든 체력바를 추가해주었다.

> ``` Add 👉 UserInterface 👉 Widget ```

해당 위젯은 오브젝트이므로 연결할 위젯 클래스를 설정해줘야한다. 방금 만든 위젯 블루프린트를 연결해주자.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/widget-class.PNG)   

- **결과**   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/hpbar-widget.PNG)   

***

### 🌱 Set Percent
위젯 블루프린트에서 보면 **Percent**를 조절함으로써 체력바를 표시할 수 있다. 이제 Hp가 변경될 때마다 ``` Set Percent ```를 통해 체력바를 관리해주면된다. 관련 기능을 ``` On Damaged ``` 함수에 추가해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/set-percent.PNG)   

> 💡 **Get User Widget Object**   
앞서 설정한 위젯 오브젝트를 가져오는 노드이다. 설정한 클래스로 캐스트 한 후 사용해야한다.

***

### 🌱 Binding
Hp가 변경될 때마다 매번 퍼센트를 세팅해주는 건 코드 남용이 우려되고, 어쩌다 빠트리는 실수를 하게되면 버그가 발생할 가능성이 커진다. 이러한 가능성을 조금이라도 줄이기 위해 **Binding** 기능이 존재한다. 위젯 상세 페이지에서 **퍼센트를 바인딩**해보자.

우선 우측 상단에서 Designer가 아닌 **Graphics**로 넘어가 퍼센트를 나타내는 변수를 하나 추가해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/hp-ratio.PNG)   

> 참고로 바인딩은 **함수**와 **변수** 모두 가능하다.

다시 ``` Designer ```의 **퍼센트에 해당 변수를 바인딩** 해주고 ``` On Damaged ```로 넘어와 코드를 수정해주자.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/binding.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/set-hpratio.PNG)   

이렇게 하면 계산된 체력의 비율이 ``` HpRatio ```에 세팅되고 자동으로 바인딩 된 퍼센트가 자동으로 세팅 되는 것이다.

- **결과**

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-fight/hpbar.gif)   

***

## 👻 글을 마치며
이번 시간에는 몬스터를 생성하고 전투하는 것까지 구현하였다. 피격 판정을 단순 충돌로 구현한다는 점에서 생각보다 간단했던 것 같다. 또 처음으로 UI를 만들어 적용해보니 약간의 UI 욕심도 생기는 것 같다. 나중에 이쁘게 만들어 봐야겠다. ☺☺☺

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/ji8q)_