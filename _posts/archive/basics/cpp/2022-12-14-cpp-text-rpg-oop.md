---
title: "[C++] TextRPG - OOP"
excerpt: "객체 지향 방식을 활용하여 TextRPG 만들어보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, practice, text rpg]

permalink: /cpp/text-rpg-oop/

toc: true
toc_sticky: true

date: 2022-12-14 17:31:26+0900
last_modified_at: 2022-12-14 17:31:30+0900
---
 
## 👻 TextRPG - OOP
이전 시간에 계속 실습해왔던 TextRPG를 객체 지향 방식을 결합해 만들어보자.

***

### 🌱 파일 분할
이제껏 한 파일에 모든 코드를 작성했었다. 이번에는 파일을 분할하여 코드를 작성해 볼 것이다.

비주얼 스튜디오 화면에서 좌측 솔루션 탐색기의 **소스 파일**을 우클릭 하여 폴더를 만들 수 있다. 게임 관리자의 정보를 담을 폴더와 플레이어, 몬스터 정보를 담을 폴더를 추가해주었다.

![Alt Text](/assets/images/posts_img/basics/cpp/text-rpg-oop/new-filter.PNG)   

![Alt Text](/assets/images/posts_img/basics/cpp/text-rpg-oop/source-file.PNG)   

그런 다음, ``` Game ``` 폴더에서 우클릭 후 클래스를 새로 추가해주었다. 헤더 파일과 cpp 파일을 따로 만들어도 되지만 클래스를 추가해주면 두 파일을 동시에 만들 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/text-rpg-oop/source-file2.PNG)   

> **Ctrl + Shift + S** : 모든 파일 저장

이렇게 모든 정보를 담은 헤더 파일과 cpp 파일을 만들었다. ``` Game ``` 폴더에는 게임과 필드를, ``` Creature ``` 폴더에는 플레이어, 몬스터 그리고 두 클래스의 부모 클래스인 Creature를 만들어주었다.

![Alt Text](/assets/images/posts_img/basics/cpp/text-rpg-oop/source-file3.PNG)   

***

### 🌱 코드 작성
> 1. 게임 시작 시 직업 선택
2. 몬스터 랜덤 생성 후 자동 전투
    - 플레이어의 Hp가 0이 될 때까지 무한 반복 전투
    - 몬스터의 Hp가 0이 되면 새로운 몬스터를 소환
3. 전투 종료 시 직업 선택으로 다시 돌아감

메인 함수는 아래와 같다.

```c++
#include <iostream>
#include "Game.h"
using namespace std;

// Text RPG - OOP

int main()
{
    srand((unsigned int)time(nullptr));
    Game game;
    game.Init();

    while (true)
        game.Update();

    return 0;
}
```

몬스터 랜덤 생성을 위해 ``` srand ```를 추가해주었고, 게임 시작 부분인 ``` Init() ```과 한 턴을 의미하는 ``` Update() ``` 함수를 만들어주었다.

각 파일들의 대략적인 정보는 다음과 같다.

- ``` Game ``` : 게임 시작, 필드 생성, 캐릭터 생성
- ``` Field ``` : 몬스터 생성, 전투 시작
- ``` Creature ``` : 플레이어와 몬스터의 정보 출력, 데미지 계산, 생사 여부 확인
- ``` Player ``` : 직업 클래스 생성 및 초기 세팅(수가 적어서 한 파일에 작성)
- ``` Monster ``` : 몬스터 클래스 생성 및 초기 세팅(수가 적어서 한 파일에 작성)

> 💡 **namespace**   
함수나 매크로 앞 부분에 namespace가 존재한다. 해당 라이브러리에 있는 함수를 사용한다는 의미이다.   
ex) ``` cout ``` : 출력해주는 함수이지만 원래는 ``` std::cout ```으로 사용해야한다. 코드가 길어지면 중복되는 부분이 늘어나므로 ``` using namespace std ```로 빼주어서 사용하게되면 중복되는 부분을 줄일 수 있다.

***

### 🌱 결과
![Alt Text](/assets/images/posts_img/basics/cpp/text-rpg-oop/result1.PNG)   

![Alt Text](/assets/images/posts_img/basics/cpp/text-rpg-oop/result2.PNG)   

***

## 👻 전방선언
아래는 TextRPG의 코드 중 ``` Game.h ```의 내용이다.

```c++
#pragma once

// 전방선언
class Player;
class Field;

class Game
{
public:
	Game();
	~Game();

	void Init();
	void Update();

	void CreatePlayer();
private:
	Player* _player;
	Field* _field;
};
```

**포함 관계에 있는 클래스를 작성할 때 사용하는 방식**이다. 각 헤더 파일과 cpp 파일은 독립적으로 빌드된다. 그래서 다른 파일에 있는 클래스를 가져다 쓸 때 그냥 쓰게되면 에러가 발생하게 되는데, 이러한 에러를 방지하기 위해선 해당 클래스가 있다는 것을 알려주어야한다. 이것을 **전방선언**이라 한다.

포인터 형이 아닌 일반 클래스를 포함하게 되면 위의 ``` Game ``` 클래스는 크기가 몇 바이트인지 정확하게 알지 못한다. 그래서 ``` include ```를 통해 해당 클래스가 있는 설계도 전부를 가져와야하고, 포함된 클래스가 완성이 되었다는 전제하에 ``` Game ``` 클래스가 만들어지게된다. **의존성이 높아진 것이다.**

반면 포인터 형이 포함되면 크기를 확실히 알 수 있기 때문에 해당 클래스가 있다는 **존재만 알려주면 된다.** 그래서 위의 코드처럼 선언만 해주게 되는 것이다. 포인터형 앞에 ``` class ```를 붙여 바로 선언할 수도 있다.

```c++
private:
    class Player* _player;
    class Field* _field;
```

단, 전방선언을 한다는 것은 설계도 전체를 가져오는 게 아닌 해당 클래스가 다음에 나올 것이라는 예고만 해주는 의미이므로 클래스 안의 변수 혹은 함수에 **직접 접근은 하지 못한다.** 그러나 **자기 자신을 타입으로 가지는 포인터 형 클래스 변수**는 전방선언을 하지 않아도 빌드가 가능하며 cpp에서 접근이 가능하다.

> 포인터 형의 크기는 **4바이트(x86)** 혹은 **8바이트(x64)**이다.

***

## 👻 글을 마치며
이번 시간에는 실습을 통해 객체 지향 방식에 대해 익혀보았다. 파일을 만드는데 처음엔 정신없었지만 오히려 기능을 분리하니 코드를 정리하고 관리하는 게 훨씬 수월해졌다. 가끔 include를 너무 많이 하지는 않는건지 걱정이 됐었는데 헤더 파일과 cpp 파일 각각의 규칙이 어느정도 정해져 있어서 금방 적응할 수 있을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/text-rpg-oop)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   