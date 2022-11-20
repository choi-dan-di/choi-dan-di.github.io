---
title: "[C++] 포인터 실습"
excerpt: "textRPG를 포인터를 이용하여 구현해보고 데이터 이동에 대해 비교 및 이해하기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, pointer, training]

permalink: /cpp/pointer-training/

toc: true
toc_sticky: true

date: 2022-11-20 22:48:09+0900
last_modified_at: 2022-11-20 22:48:10+0900
---

## 👻 포인터 실습
이번 시간엔, 지난 시간에 실습 해보았던 [textRPG](/cpp/text-rpg/)를 포인터를 이용하여 간단하게 실습해보자. 포인터 사용법만 익히는 실습이므로 _플레이어 직업 선택과 몬스터 랜덤 생성 단계_ 는 제외한 간단한 알고리즘으로 구현해볼 것이다.

> 1. 로비 입장
2. 플레이어 생성
3. 몬스터 생성
4. 전투 및 결과

시작하기 전 기본 코드는 아래와 같다.

```c++
struct StatInfo
{
    int hp; // +0 (주소)
    int attack; // +4
    int defence;    // +8
};

void EnterLobby();

// 차이를 보기 위해 다르게 만듦
// 포인터 이전 버전
StatInfo CreatePlayer();
// 포인터 이후 버전
void CreateMonster(StatInfo* info);

// 플레이어 승리 유무 리턴 (승 : true, 패 : false)
bool StartBattle(StatInfo* player, StatInfo* monster);
```

플레이어와 몬스터가 공통으로 사용할 **구조체**를 하나 작성해주고 함수들을 선언해주었다.

***

### 🌱 코드 작성
```c++
void EnterLobby()
{
    cout << "로비에 입장했습니다." << endl;

    // 플레이어 생성
    StatInfo player;

    // 메모리를 편하게 살펴보기 위해 임의의 값 세팅
    player.hp = 0xbbbbbbbb;
    player.attack = 0xbbbbbbbb;
    player.defence = 0xbbbbbbbb;

    player = CreatePlayer();

    // 몬스터 생성
    StatInfo monster;

    // 메모리를 편하게 살펴보기 위해 임의의 값 세팅
    monster.hp = 0xbbbbbbbb;
    monster.attack = 0xbbbbbbbb;
    monster.defence = 0xbbbbbbbb;

    CreateMonster(&monster);
}

StatInfo CreatePlayer()
{
  StatInfo ret;

  cout << "플레이어 생성" << endl;
  // 플레이어 세팅
  ret.hp = 100;
  ret.attack = 10;
  ret.defence = 2;

  return ret;
}

void CreateMonster(StatInfo* info)
{
  cout << "몬스터 생성" << endl;
  // 몬스터 세팅
  info->hp = 40;
  info->attack = 8;
  info->defence = 1;
}

bool StartBattle(StatInfo* player, StatInfo* monster)
{
    while (true)
    {
        int damage = player->attack - monster->defence;
        if (damage < 0)
            damage = 0;

        monster->hp -= damage;
        if (monster->hp < 0)
            monster->hp = 0;

        cout << "몬스터 HP : " << monster->hp << endl;

        if (monster->hp == 0)
            return true;

        damage = monster->attack - player->defence;
        if (damage < 0)
            damage = 0;

        player->hp -= damage;
        if (player->hp < 0)
            player->hp = 0;

        cout << "플레이어 HP : " << player->hp << endl;

        if (player->hp == 0)
            return false;
    }
}
```

``` CreatePlayer() ```에는 이전과 다르게 **구조체**와 **리턴값** 조건이 추가되었고 ``` CreateMonster() ```에는 **구조체**와 **포인터** 조건이 추가되었다. 간접 멤버 연산자를 통해 몬스터의 값을 세팅해주었다.

***

### 🌱 코드 분석

위 코드의 디버깅을 통해 코드를 분석해보자. 브레이크 포인트는 각각 ``` player = CreatePlayer(); ```, ``` CreateMonster(&monster); ```에 잡았다.

***

#### 🪐 CreatePlayer
우선 ``` CreatePlayer() ``` 함수를 통해 플레이어 값이 어떤 방식으로 세팅되는 지 천천히 알아보자.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/memory.PNG)   

메모리를 쉽게 확인하기 위해 ``` 0xbbbbbbbb ```라는 임의의 값을 설정해두었는데 ``` &player ```를 입력해 플레이어의 주소로 가면 값이 잘 들어가 있는 것을 확인할 수 있다.

> ``` &player ``` : ``` 0x00EFFD3C ```

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/asm.PNG)   

다음으로 어셈블리코드를 확인해보자. ``` CreatePlayer() ``` 함수 실행 이전에 ``` [ebp-110h] ```를 스택에 **push**해주는 걸 확인할 수 있다. 아직까지는 무엇을 뜻하는지는 모르지만 메모리에 ``` ebp-110h ```를 입력해 해당 주소로 이동해보자.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/ebp.PNG)   

참고로 여기서 조금만 내리면 ``` &player ```를 확인할 수 있다. 지금은 쓰레기값이 들어가있지만 아무튼 이 주소값을 ``` temp ```라 가정하고 다음 ``` CreatePlayer() ``` 함수 내부로 들어가보자. 함수 내부 ``` StatInfo ret; ```에 브레이크 포인트를 추가한 다음 F5를 눌러 진행해보자.

> ``` ebp-110h(temp) ``` : ``` 0x00EFFC40 ```

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/asm2.PNG)   

함수 내부로 들어와 한 줄씩 실행해주었다. ``` &ret ```를 찾아가니 값이 잘 들어가 있는 것을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/ret.PNG)   

> 여기서 플레이어의 값을 세팅해주고 있는데 주소가 각각 ``` [ret] ```, ``` [ebp-0Ch] ```, ``` [ebp-8] ```로 되어있는 것을 볼 수 있다. 모두 직접 스택 프레임에 접근해 값을 세팅해 주고 있다는 뜻이며 ``` [ret] ```는 우리가 보기 편하도록 되어있는 것이지 ``` [ebp-16] ```과 동일하다.

이제 이 값을 어떻게 외부로 보내느냐에 대해 알아보자.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/asm3.PNG)   

``` ebp+8 ```을 ``` eax ```에, ``` ret(ebp-16) ```을 ``` ecx ```에 각각 넣어주고 있는 것을 확인할 수 있다. 그 밑의 어셈블리 코드는 구조체 자체를 복사해오는 코드이다. ``` ebp+8 ```을 찾아가보자.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/ebp8.PNG)   

해당 주소에 들어있는 값은 아까 보았던 ``` ebp-110h(temp) ``` 주소값과 동일한 걸 알 수 있다. 이제 다시 ``` temp ```로 이동해 값이 세팅되는 것을 실시간으로 확인해보자.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/ebp2.PNG)   

F10을 눌러 한 줄씩 실행시키면 각 값이 세팅되는 것을 확인할 수 있다. 이로써 ``` temp ```에 값 세팅은 끝났고 ``` ret ```를 만나면 함수 내부를 빠져나와 함수 호출 밑의 코드가 실행될 것이다.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/asm4.PNG)   

``` add esp, 4 ```부터 아래에 있는 코드가 바로 ``` temp ```의 값을 ``` player ```에 세팅해주는 기능을 수행한다. 플레이어의 주소로 이동한 후에 한 줄씩 실행해보면 각 값이 세팅되는 것을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/player.PNG)   

> 💡 **요약** (각 스택의 depth는 숫자로 구분)   
1. 지역변수1 ``` player ```와 임시 저장소 역할을 하는 ``` temp(ebp-110h) ``` 할당
2. ``` CreatePlayer() ``` 함수 호출
3. 매개변수2에 ``` temp ``` 주소값 저장
4. 지역변수2 ``` ret ``` 할당 및 값 세팅
5. ``` return ret; ```을 함으로써 ``` temp ```에 값 세팅
6. ``` temp ```의 값을 ``` player ```에 세팅

***

#### 🪐 CreateMonster
이제 몬스터 생성 함수를 살펴보자. 메모리 창에 ``` &monster ```를 검색하여 값을 확인해보자.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/monster.PNG)   

플레이어와 같이 임의로 지정한 값이 세팅되어있는 것을 알 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/asm5.PNG)   

어셈블리코드는 플레이어와 다르게 간단한 것을 볼 수 있다. 이번엔 임시 저장소가 아닌 ``` monster ```의 주소값을 스택에 **push**한 후 함수 호출을 진행한다. F11을 눌러 함수 내부로 이동해보자.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/asm6.PNG)   

함수 내부로 이동한 후 어셈블리코드에 브레이크 포인트를 걸어주었다. 다음 F5를 눌러 바로 해당 포인트로 이동해 주었다. 매개변수인 ``` info ```의 주소값을 바로 전달받아 ``` eax ```에 세팅하고, 해당 주소가 가리키는 값을 바로 변경해주는 걸 볼 수 있다. ``` info ```가 가리키는 주소는 곧 ``` monster ```의 주소와 동일하다는 걸 알 수 있고, 그렇다는 뜻은 처음 설정해 주었던 ``` monster ``` 원본 자체에 직접 접근하여 값을 변경해주고 있다는 것을 알 수 있다. 한 줄 씩 실행시키면서 ``` monster ```의 메모리를 확인해보자.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/asm7.PNG)   

어셈블리코드는 모두 진행이 되었고,

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/monster2.PNG)   

몬스터의 값이 바로 세팅된 것을 볼 수 있다.

> 💡 **요약** (각 스택의 depth는 숫자로 구분)   
1. 지역변수1 ``` monster ``` 할당
2. ``` CreateMonster() ``` 함수 호출
3. 매개변수2 ``` info(&monster) ``` 할당
4. 매개변수2가 가리키는 지역변수1 ``` monster ```에 값 바로 세팅

***

#### 🪐 StartBattle

**간접 멤버 연산자**를 이용해 플레이어와 몬스터 원본 자체에 **직접 접근**하여 데이터를 세팅하고 있다.

***

## 👻 실습 결과
![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/result.PNG)   

포인터를 사용하지 않은 플레이어의 경우 임시 저장소 생성부터 **수많은 값의 복사**가 일어나게 되는데, 포인터를 사용한 몬스터의 경우 원본 자체에 접근을 하여 간편하고 빠르게 값을 이동시키는 걸 확인할 수 있었다. 고로 포인터를 사용하게되면 사용하지 않는 경우보다 훨씬 **간편**하고 **효율적**으로 프로그램을 작성할 수 있게된다.

***

## 👻 번외
번외로, 구조체끼리 복사할 때 어떤 일이 벌어지는지 알아보자.

```c++
player = monster;
```

해당 코드를 작성한 후 어셈블리코드를 살펴보자.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-training/asm8.PNG)   

우리가 작성한 코드는 한 줄이지만 어셈블리코드로 살펴보면 여섯 번의 이동이 있다는 것을 알 수 있다. 고로 ``` player = monster; ```의 뜻은

```c++
player.hp = monster.hp;
player.attack = monster.attack;
player.defence = monster.defence;
```

와 같다.

지금은 3개의 ``` int ``` 타입을 예시로 들어 구조체를 만들어 사용했지만, 실제 게임을 만들다보면 데이터가 수천 바이트까지 늘어날 수 있다. 그러다보면 이렇게 구조체를 복사하는 것도 많은 부하가 걸리기 때문에 조심해서 사용해야 할 필요가 있다.

***

## 👻 글을 마치며
이번 시간에는 지난 시간에 만들어보았던 textRPG를 포인터를 이용하여 구현해보았다. 확실히 포인터를 이용하니 코드가 간편해지고 데이터 접근 자체가 쉬워졌다. 디버깅을 통해 데이터가 어떤 식으로 이동되는지까지 동시에 알아보니 포인터의 중요성을 더 많이 느끼게 되었다. 반대로 포인터를 사용했을 때의 데이터 이동 방식은 쉽게 이해할 수 있었는데, 포인터를 이용하지 않았을 때의 데이터 이동이 약간 어려운 것 같다. 아무래도 임시 저장소가 새로 생겨서 그런 것 같은데, 데이터 이동 방식은 알겠으나 주소를 정하는 기준이 약간 애매모호하다.. 포스팅 후 복습을 조금 더 해야할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/pointer/pointer-training)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   