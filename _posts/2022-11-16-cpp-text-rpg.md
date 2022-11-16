---
title: "[C++] 연습 문제 : TextRPG"
excerpt: "간단한 RPG를 만들면서 복습해보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, function, textrpg, rpg, practice, padding]

permalink: /cpp/text-rpg/

toc: true
toc_sticky: true

date: 2022-11-16 23:29:55+0900
last_modified_at: 2022-11-16 23:29:59+0900
---

## 👻 TextRPG
간단하게 콘솔에서 할 수 있는 게임을 만들어보자. 게임의 흐름은 이렇다.

> 1. 로비 입장
2. 플레이어 직업 선택
3. 필드 입장
4. 랜덤 몬스터 생성
5. 전투

※ 모든 함수는 **선언**을 먼저 해주었다.

***

### 🌱 로비 입장
- ``` EnterLobby() ``` **생성**

```c++
void EnterLobby()
{
    while (true)
    {
        cout << "--------------------" << endl;
        cout << "로비에 입장했습니다!" << endl;
        cout << "--------------------" << endl;

        // 플레이어 직업 선택
        SelectPlayer();

        cout << "---------------------------" << endl;
        cout << "(1) 필드 입장 (2) 게임 종료" << endl;
        cout << "---------------------------" << endl;

        int input;
        cin >> input;

        if (input == 1)
        {
            EnterField();
        }
        else
        {
            return;
        }
    }
}
```

로비에 입장한 후 플레이어의 직업을 선택하는 부분이다. 플레이어의 직업 선택이 끝나면 필드 입장과 게임 종료 중에서 선택을 한다.

***

### 🌱 플레이어 직업 선택
아직 포인터를 배우지 않았기 때문에 전역 변수로 플레이어의 정보를 저장하는 방식을 사용했다.

- **전역 변수 설정**

```c++
enum PlayerType
{
    PT_Knight = 1,
    PT_Archer = 2,
    PT_Mage = 3
};

int playerType;
int hp;
int attack;
int defence;
```

그리고 플레이어 직업을 선택하는 함수를 만들어주었다.

- ``` SelectPlayer() ``` **생성**

```c++
void SelectPlayer()
{
    while (true)
    {
        cout << "직업을 골라주세요!" << endl;
        cout << "(1) 기사 (2) 궁수 (3) 법사" << endl;
        cout << "> ";
        cin >> playerType;

        if (playerType == PT_Knight)
        {
            cout << "기사 생성중... !" << endl;
            hp = 150;
            attack = 10;
            defence = 5;
            break;
        }
        else if (playerType == PT_Archer)
        {
            cout << "궁수 생성중... !" << endl;
            hp = 100;
            attack = 15;
            defence = 3;
            break;
        }
        else if (playerType == PT_Mage)
        {
            cout << "법사 생성중... !" << endl;
            hp = 80;
            attack = 25;
            defence = 0;
            break;
        }
    }
}
```

직업을 선택하면 전역 변수에 각각의 정보가 담기게된다. 설정이 끝나면 ``` EnterLobby() ```로 다시 넘어오게된다.

***

### 🌱 필드 입장
- ``` EnterField() ``` **생성**

```c++
void EnterField()
{
    while (true)
    {
        cout << "--------------------" << endl;
        cout << "필드에 입장했습니다!" << endl;
        cout << "--------------------" << endl;

        cout << "[PLAYER] HP : " << hp << " / ATT : " << attack << " / DEF : " << defence << endl;

        // 몬스터 생성
        CreateRandomMonster();

        cout << "-----------------" << endl;
        cout << "(1) 전투 (2) 도주" << endl;
        cout << "-----------------" << endl;
        cout << "> ";

        int input;
        cin >> input;

        if (input == 1)
        {
            EnterBattle();

            if (hp == 0)
                return;
        }
        else
        {
            EnterLobby();
        }
    }
}
```

필드에 생성하면 몬스터를 랜덤으로 생성한다. 생성이 완료되면 전투와 도주중에 선택을 하게된다. 전투 선택 시, 전투가 끝난 후 플레이어 체력이 0이면 로비로 돌아간다.

***

### 🌱 랜덤 몬스터 생성
플레이어와 마찬가지로 몬스터의 정보를 전역 변수에 저장하는 방식을 사용하였다.

- **전역 변수 생성**

```c++
enum MonsterType
{
    MT_Slime = 1,
    MT_Orc = 2,
    MT_Skeleton = 3
};

int monsterType;
int monsterHp;
int monsterAttack;
int monsterDefence;
```

- ``` CreateRandomMonster() ``` **생성**

```c++
void CreateRandomMonster()
{
    // 1 ~ 3 사이 난수 생성
    monsterType = (rand() % 3) + 1;

    switch (monsterType)
    {
    case MT_Slime:
        cout << "슬라임 생성중...! (HP:15 / ATT:5 / DEF:0)" << endl;
        monsterHp = 15;
        monsterAttack = 5;
        monsterDefence = 0;
        break;
    case MT_Orc:
        cout << "오크 생성중...! (HP:40 / ATT:10 / DEF:3)" << endl;
        monsterHp = 40;
        monsterAttack = 10;
        monsterDefence = 3;
        break;
    case MT_Skeleton:
        cout << "스켈레톤 생성중...! (HP:80 / ATT:15 / DEF:5)" << endl;
        monsterHp = 80;
        monsterAttack = 15;
        monsterDefence = 5;
        break;
    }
}
```

플레이어 직업 선택시와 동일하게 몬스터 정보를 세팅해주었다. 설정이 완료되면 ``` EnterField() ```로 돌아간다.

***

### 🌱 전투
- ``` EnterBattle() ``` **생성**

```c++
void EnterBattle()
{
    while (true)
    {
        int damage = attack - monsterDefence;
        if (damage < 0)
            damage = 0;

        // 플레이어 선
        monsterHp -= damage;
        if (monsterHp < 0)
            monsterHp = 0;

        cout << "몬스터 남은 체력 : " << monsterHp << endl;

        if (monsterHp == 0)
        {
            cout << "몬스터를 처치했습니다!" << endl;
            return;
        }

        damage = monsterAttack - defence;
        if (damage < 0)
            damage = 0;

        // 몬스터가 반격
        hp -= damage;
        if (hp < 0)
            hp = 0;

        cout << "플레이어 남은 체력 : " << hp << endl;

        if (hp == 0)
        {
            cout << "당신은 사망했습니다... GAME OVER" << endl;
            return;
        }
    }
}
```

플레이어 선으로 한 턴씩 번갈아가면서 전투를 진행하게 된다. 디펜스가 있으면 먼저 데미지에서 제한 후 나머지 값을 각 체력에 적용시킨다. 둘 중 하나가 0이 될때까지 진행한 후 해당 함수를 빠져나오게 된다.

***

### 🌱 결과

![Alt Text](/assets/images/posts_img/basics/cpp/function/practice/text-rpg/result.PNG)   

이렇게 해서 간단한 RPG를 만들어보았다.

> 💡   
- **Ctrl + M, O** : 모든 코드 접기
- **Ctrl + M, P** : 모든 코드 열기

***

## 👻 구조체
플레이어의 정보와 몬스터의 정보를 전역 변수로 설정할 때, 동일한 대상의 정보를 표현하고 있는데 개별로 선언해서 사용 중인 걸 알 수 있다. 이러한 변수들을 ``` enum ```처럼 묶어서 사용할 수 있는 것이 바로 **구조체(Structure)**이다.

```c++
struct [이름]
{
    타입 변수명;
    타입 변수명;
    타입 변수명;
};
```

사용 방식은 ``` enum ```과 비슷하지만 초기값이 없고 콤마(,)가 아닌 세미콜론(;)을 붙임으로써 한 줄을 마무리하는 것을 알 수 있다. ~~java의 object와 약간 비슷한 느낌..?~~

구조체를 사용하여 위의 변수를 한데 묶어보자.

```c++
struct ObjectInfo
{
    int type;
    int hp;
    int attack;
    int defence;
};

ObjectInfo playerInfo;
ObjectInfo monsterInfo;
```

플레이어와 관련된 정보는 ``` playerInfo ```에, 몬스터와 관련된 정보는 ``` monsterInfo ```에 담기게 된다. 사용할 때는 구조체 변수 뒤에 점(.)을 찍고 원하는 변수명을 적어서 사용하면 된다.

- **사용**
```c++
cin >> playerInfo.type;
monsterInfo.attack = 10;
```

***

### 🌱 패딩
패딩에 대해서 잠깐 다뤄보도록 하자.

**패딩(Padding)**은 간단히 말해서 컴퓨터가 계산하기 편하게끔 변수의 범위를 조절해 정렬을 시켜주는 것을 의미한다. 방금 위에서 만들었던 ``` ObjectInfo ```는 모두 ``` int ```타입으로 4바이트씩 16바이트가 할당되어있지만 타입이 다를 경우, 해당 타입들의 크기 합만큼 메모리에 할당되지 않고 빈 공간을 추가해 모두 같은 크기로 맞춰줘서 연산을 편리하게 진행하는 개념이다.

***

## 👻 글을 마치며
이번 시간엔 실습을 진행하며 함수의 선언과 정의, 코드 흐름에 대해 알게 되었고 마지막에 구조체를 배움으로써 조금 더 코드를 정리시킬 수 있었다. 역시 직접 해보는 게 가장 많이 이해되고 습득할 수 있는 것 같다. ~~얼른 파일 나누고 싶어😖~~

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/function/practice/text-rpg)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   