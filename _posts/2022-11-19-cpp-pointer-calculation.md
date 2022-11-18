---
title: "[C++] 포인터 연산"
excerpt: "포인터의 연산 방법에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, pointer, calculation]

permalink: /cpp/pointer-calculation/

toc: true
toc_sticky: true

date: 2022-11-18 18:42:15+0900
last_modified_at: 2022-11-18 18:42:18+0900
---

## 👻 포인터 연산
포인터 연산에는 **주소 연산자**, **산술 연산자**, **간접 연산자**, **간접 멤버 연산자**가 있다.

***

### 🌱 주소 연산자
**앰퍼센트(&)**를 사용한다. **해당 변수의 주소를 알려달라**는 의미이다. 더 정확히 말하자면 해당 변수 타입에 따라서 ``` type* ```을 반환한다.

```c++
int* pointer = &number;
```

포인터 기초를 공부할 때 알아봤었던 문법인 걸 알 수 있다.

***

### 🌱 산술 연산자
**플러스(+), 마이너스(-)**를 사용한다.

```c++
pointer = pointer + 1;
pointer++;
++pointer;
pointer += 1;
```

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-calculation/memory.PNG)   

``` number ```의 값이 증가할 것 같지만 ``` pointer ```가 저장하고 있는 주소값이 증가한 것을 알 수 있다. 또한 1을 더했지만 **4바이트**만큼 증가했다는 것을 알 수 있다.

포인터에 1을 증가시키면 말 그대로 1이 증가하는 게 아닌, **다음 주소로 증가**하라는 의미가 된다. 결국 포인터 타입의 크기만큼을 이동하라는 의미가 된다. (``` * 타입크기 ```가 생략되어있다고 볼 수 있다.)

> 여기선 ``` number += 1; ``` 코드가 적혀져 있으므로 ``` number ```의 값이 **2**가 된거지 포인터 연산 때문에 나온 결과는 아니다. 또한 ``` int ```타입이라 **4바이트**만큼 증가한거지 타입이 달라지면 해당 타입의 크기만큼 증가 혹은 감소하게 된다.

***

### 🌱 간접 연산자
**별(*)**을 사용한다. 포탈을 타듯 해당 주소로 이동하라는 의미이다.

```c++
*pointer = 3;
```

위처럼 사용하게되면 ``` pointer ```가 가리키는 주소값인 ``` number ```의 값으로 이동해 **3**으로 변경하라는 의미가 된다. 구조체와도 사용이 가능하다.

```c++
Player player
{
    int hp;
    int damage;
}

Player player;
player.hp = 100;
player.damage = 10;

Player* playerPtr = &player;

(*playerPtr).hp = 200;
(*playerPtr).damage = 200;
```

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-calculation/memory2.PNG)   

``` &playerPtr ```의 값은 ``` 0x005DF768 ```이고 여기에 ``` player ```의 주소값이 들어가게된다.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-calculation/memory3.PNG)   

``` 0x005DF774 ```로 가면 ``` hp ``` 값인 **100**과 다음 주소에 ``` damage ``` 값인 **10**이 들어가있는 것을 확인할 수 있고, F10을 눌러 디버깅을 진행해보면 각각의 원본값이 직접 바뀐 것을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/pointer-calculation/memory4.PNG)   

***

### 🌱 간접 멤버 연산자
**화살표(->)**를 사용한다. 간접 연산자에서 **별(*)**과 **점(.)**이 합쳐진 개념이다.

```c++
// (*playerPtr).hp = 200;
// (*playerPtr).damage = 200;

playerPtr->hp = 200;
playerPtr->damage = 200;
```

의미는 간접 연산자에서 보았던 예시와 완전히 동일하다.

***

## 👻 글을 마치며
이번 시간에는 포인트 연산에 대해 알아보았다. 주소 연산자와 간접 연산자는 이전 기초 시간때 알아본 것이라 어렵진 않았다. 산술 연산자도 1을 더한다는 게 주소값이라 생각하니 쉽게 이해할 수 있었다. 간접 멤버 연산자는 간접 연산자가 좀 더 간소화해진 것 같아서 흥미로웠다. 역시 공부를 하다보면 점점 간소화되는 게 가장 재미있는 부분인 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/pointer/pointer-calculation)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   