---
title: "[C++] 연습 문제 : TextRPG"
excerpt: "오로지 스택 메모리만을 사용하여 TextRPG 구현해보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, pointer, practice, text rpg, stack memory]

permalink: /cpp/text-rpg-stack/

toc: true
toc_sticky: true

date: 2022-11-24 18:28:33+0900
last_modified_at: 2022-11-24 18:28:36+0900
---

## 👻 TextRPG
이번 시간에는 꾸준히 만들어왔던 TextRPG를 전역 메모리가 아닌, 스택 메모리만 사용해서 만들어보자. 스택 메모리만 사용하려면 전역 변수를 사용하지 않고 오로지 함수 내에서만 변수들을 만들어 전달하는 방식을 사용하게 되는 것이다. 이제껏 많은 정리를 해왔으니 이번에는 간단하게 헷갈리는 부분만 정리할 예정이다.

***

### 🌱 함수 선언부
```c++
void EnterLobby();
void PrintMessage(const char* msg);
void CreatePlayer(StatInfo* playerInfo);
// StatInfo CreatePlayer2();
void PrintStatInfo(const char* name, const StatInfo& info);
void EnterGame(StatInfo* playerInfo);
void CreateMonsters(StatInfo monsterInfo[], int count);
bool EnterBattle(StatInfo* playerInfo, StatInfo* monsterInfo);
```

포인터나 참조, 배열을 사용하여 매개 변수를 설정하였다. 각 함수 내에서 변수를 어떻게 사용하는지 알아보자.

***

### 🌱 포인터 매개 변수
- ``` CreatePlayer ```   

```c++
void CreatePlayer(StatInfo* playerInfo) {
    bool ready = false;
    while (!ready) {
        PrintMessage("캐릭터 생성창");
        PrintMessage("[1] 기사 [2] 궁수 [3] 법사");
        cout << "> ";

        int input;
        cin >> input;

        switch (input) {
        case PT_Knight:
            playerInfo->hp = 100;
            playerInfo->attack = 10;
            playerInfo->defence = 5;
            ready = true;
            break;
        case PT_Archer:
            playerInfo->hp = 80;
            playerInfo->attack = 15;
            playerInfo->defence = 3;
            ready = true;
            break;
        case PT_Mage:
            playerInfo->hp = 50;
            playerInfo->attack = 25;
            playerInfo->defence = 1;
            ready = true;
            break;
        }
    }
}
```

크게 다를 것은 없고, 매개 변수인 ``` playerInfo ```가 주소값을 가리키고 있으니 ``` -> ``` 기호를 이용하여 값에 접근해주었다. 해당 값을 수정하면 ``` playerInfo ```의 값이 바로 바뀌게 된다.

참고로 ``` playerInfo ```는 ``` EnterLobby ```에서 선언해 준 다음 넘겨주었다. 넘겨줄 때는 주소값을 나타내는 **앰퍼센트(&)**를 붙여서 넘겨주는 걸 잊지말자.

```c++
// 플레이어 생성
StatInfo playerInfo;
CreatePlayer(&playerInfo);
```

- ``` PrintMessage ```   

```c++
void PrintMessage(const char* msg) {
    cout << "*****************************\n";
    cout << msg << '\n';
    cout << "*****************************\n";
}
```

> ``` PrintMessage ``` 함수도 포인터 형으로 받지만 ``` cout ```과 함께 사용하는 ``` << ``` 오퍼레이터는 온갖 타입 별로 동작하게 재정의 되어 있어서 포인터 형으로 값을 받아도 자동으로 변환되어 문자열로 출력해주기 때문에 그냥 사용하면 된다.

***

### 🌱 참조 매개 변수
- ``` PrintStatInfo ```

```c++
void PrintStatInfo(const char* name, const StatInfo& info) {
    cout << "*****************************\n";
    cout << name << " : HP = " << info.hp << ", ATT = " << info.attack << ", DEF = " << info.defence << '\n';
    cout << "*****************************\n";
}
```

참조도 사용해보았다. 이 함수 내에서는 **읽어오기(readonly)**만 할 예정이기 때문에 앞 부분에 ``` const ```를 붙여주었다.

***

### 🌱 배열 매개 변수
- ``` CreateMonsters ```

```c++
void CreateMonsters(StatInfo monsterInfo[], int count) {
    for (int i = 0; i < count; i++) {
        int randValue = (rand() % 3) + 1;

        switch (randValue) {
        case MT_Slime:
            monsterInfo[i].hp = 30;
            monsterInfo[i].attack = 5;
            monsterInfo[i].defence = 1;
            break;
        case MT_Orc:
            monsterInfo[i].hp = 40;
            monsterInfo[i].attack = 8;
            monsterInfo[i].defence = 2;
            break;
        case MT_Skeleton:
            monsterInfo[i].hp = 50;
            monsterInfo[i].attack = 15;
            monsterInfo[i].defence = 3;
            break;
        }
    }
}
```

이번에는 몬스터 수를 2마리로 늘려 배열로 저장해보았다. 매개 변수 타입도 배열이지만 사실상 포인터와 같은 역할을 하기 때문에 포인터처럼 사용해주면 된다.

***

## 👻 글을 마치며
이번 시간에는 스택 메모리만을 사용하여 연습 문제를 구현해보았다. 확실히 많은 걸 배울수록 코드의 깊이가 깊어지고 메모리 활용이 효율적이라는 게 느껴지는 것 같다. 비록 스크롤을 계속 움직여야하지만 이것도 파일 정리하면 훨씬 간편해질 것이다. 근데 우선 나는 코드를 내 알고리즘에 맞게 짜는 것부터 시작해야 할 것같다. 처음부터 완벽하게 만들려고 하다보니 쉽사리 손을 때지 못하고 어느 코드가 더 나을지 결정을 하지 못해 수시로 수정하게 되는 것 같다. 단기적으로 보면 좋은 습관이지만 장기적으로 보면 오히려 힘들어질 것 같아서 이 습관은 최대한 빨리 고쳐나가야 할 것 같다. 포인터 끝!

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/pointer/practice/text-rpg)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   