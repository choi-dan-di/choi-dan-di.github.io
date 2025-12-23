---
title: "[C++] 초기화 리스트(Initializer List)"
excerpt: "멤버 변수를 초기화하는 문법 중 초기화 리스트에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, initializer list]

permalink: /cpp/initializer-list/

toc: true
toc_sticky: true

date: 2022-11-27 18:26:43+0900
last_modified_at: 2022-11-27 18:26:47+0900
---
 
## 👻 초기화
모든 값들을 선언하기 전에는 초기화가 필요하다. **초기화 리스트(Initializer List)**는 **클래스 내의 멤버 변수를 초기화 시키는 문법** 중 하나이다. 이 외에는 **생성자 내에서** 하는 방법과 **C++11 문법**을 사용해서 하는 방법이 있다.

간단하게 알아보자면 다음과 같다.

- **생성자 내에서**   

```c++
class Knight
{
public:
    Knight() {
        _hp = 100;  // _hp를 100으로 초기화
    }
public:
    int _hp;
};
```

- **C++11 문법**   

```c++
int _hp = 200;  // 선언과 동시에 바로 초기화
```

***

### 🌱 초기화는 왜 해야할까?

```c++
class Knight
{
public:
    Knight() { }
public:
    int _hp;
};

int main()
{
    Knight k;
    cout << k._hp << endl;
}
```

여기서 ``` k._hp ```의 값은 0이 될 것 같지만 이상한 숫자가 들어가있다. 메모리에는 공간의 할당 유무로 데이터를 관리하기 때문에 이상한 값이 들어있을 가능성이 높다. 그래서 사용하기 전엔 꼭 초기화를 해주어야 올바른 값을 사용하여 **버그를 예방**할 수 있다. 포인터 등 주소값이 연루되어 있다면 더더욱 중요하다. 컴파일러 레벨을 높여 더욱 까다롭게 확인하도록 만드는 것도 버그를 예방하는 데 도움이 된다.

***

## 👻 초기화 리스트
초기화 리스트는 생성자 뒤에서 초기화를 해주는 방법으로 다음과 같이 사용할 수 있다.

```c++
class Knight : public Player
{
public:
    Knight() : Player(1), _hp(100), _inventory(20)
        /*
        선처리 영역
        Inventory()
        */
    {
        // _hp = 100;
        // _inventory = Inventory(20);
    }
public:
    int _hp;
    Inventory _inventory;
};
```

초기화 리스트 방식을 사용하면 기본 생성자가 실행되기 전, **선처리 영역** 부분에서 실행되며 생성자 내에서 초기화 시키는 것보다 훨씬 더 효율성이 좋다.

또한 상속 관계에서 원하는 부모 생성자를 호출할 수 있고, 정의함과 동시에 초기화가 필요한 경우에도 유용하다. 멤버 변수 선언 시 참조 타입과 상수 타입을 선언하게 되면 **생성자 내부에서 초기화할 수 없기** 때문이다.

```c++
class Knight : public Player
{
public:
    Knight() : Player(1), _hp(100), _inventory(20), _hpRef(_hp), _hpConst(100)
        /*
        선처리 영역
        Inventory()
        */
    {
        // _hp = 100;
        // _inventory = Inventory(20);
    }
public:
    int _hp;
    Inventory _inventory;

    int& _hpRef;
    const int _hpConst;
};
```

> C++11 이전 문법이니 참고만 해두도록 하자.

***

## 👻 Is-A vs Has-A
프로젝트의 크기가 커지다보면 클래스 내부에 클래스 타입으로 선언을 하게 되는 경우가 생기게 된다. 클래스와 클래스 간의 연결엔 **상속 관계(Is-A)**와 **포함 관계(Has-A)**가 있다.

상속 관계면 말 그대로 상속 받아 사용하면 되고, 포함 관계면 클래스 내부에 선언하여 사용할 수 있게 된다.

> **Example**   
- **상속 관계(Is-A)**
    - **Knight Is-A Player?(기사는 플레이어인가?) 👉 YES**
    - Knight Is-A Inventory?(기사는 인벤토리인가?) 👉 NO
- **포함 관계(Has-A)**
    - Knight Has-A Player?(기사는 플레이어를 갖고 있는가?) 👉 NO
    - **Knight Has-A Inventory?(기사는 인벤토리를 갖고 있는가?) 👉 YES**

***

## 👻 글을 마치며
이번 시간에는 멤버 변수를 초기화 하는 방법 중 하나인 초기화 리스트에 대해 알아보았다. 생성자 내에서 초기화 하는 것보다 훨씬 더 효율적이고 원하는 부모 클래스의 생성자를 지정해 호출할 수 있어서 좋은 것 같다. 하지만 C++11 문법이 너무 간편해서 생각보다는 자주 쓰이진 않을 것 같긴하다. 😅

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/oop/initializer-list)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   