---
title: "[C++] 상속성(Inheritance)"
excerpt: "객체 지향 프로그래밍의 특징 중 하나인 상속성에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, inheritance]

permalink: /cpp/inheritance/

toc: true
toc_sticky: true

date: 2022-11-26 17:33:32+0900
last_modified_at: 2022-11-26 17:33:36+0900
---
 
## 👻 상속성
이번 시간에는 객체 지향 프로그래밍의 특징 중 하나인 **상속성(Inheritance)**에 대해 알아보자. 상속은 말 그대로 부모로부터 유산을 상속받는 것처럼 무언가를 물려받는다는 의미이다. 공통된 기능을 가진 부모 객체를 만든 후, 자식 객체가 부모 객체의 특징을 상속받아 사용할 수 있게끔 하는 것이다.

여기 기사, 법사 클래스가 있다고 가정해보자.

```c++
class Knight
{
public:
    void Move() {}
    void Attack() {}
    void Die() {}

public:
    int _hp;
    int _attack;
    int _defence;
};

class Mage
{
public:
    void Move() {}
    void Attack() {}
    void Die() {}

public:
    int _hp;
    int _attack;
    int _defence;
};
```

동일한 멤버 함수와 멤버 변수를 가진다. 이처럼 코드가 중복이 되면 비효율적이기 때문에 여기서 공통된 기능만 뽑아 부모 객체로 새로 만들어야한다.

```c++
// 공통된 기능을 묶어둔 부모 객체 생성
class Player
{
public:
    void Move() {}
    void Attack() {}
    void Die() {}

public:
    int _hp;
    int _attack;
    int _defence;
}
```

부모 객체를 생성한 후 자식 객체에서 상속받아 사용하면 된다.

```c++
class Knight : public Player
{
public:
    
};

class Mage : public Player
{
public:
    
};
```

이렇게 내용이 비어있어도 호출하면 아무런 문제없이 ``` Player ```의 모든 멤버 변수와 멤버 함수를 사용할 수 있게된다.

> 메모리는 ``` 부모 객체 👉 자식 객체 ``` 순으로 할당되며 자식 객체에서 확장된 내용은 부모 객체의 뒤에 붙게 된다.

***

### 🌱 재정의(Overriding)
부모 객체의 멤버 함수와 같은 이름으로 자식 객체에 구현하게되면 해당 함수는 **재정의(Overriding)**되게 된다. 즉, 상속을 받는 관계에서 재정의를 할 수 있게 된다.

```c++
class Player
{
public:
    void Move() { cout << "Player Move 호출" << endl; }
}

class Knight : public Player
{
public:
    // 재정의
    void Move() { cout << "Knight Move 호출" << endl; }
}

int main()
{
    Knight k;
    k.Move();
    // 부모 객체의 원본 함수 호출
    k.Player::Move();
}
```

여기서 ``` k.Move() ```의 실행값은 재정의 된 ``` Knight Move 호출 ```이 실행된다. 부모 객체의 함수를 실행하고 싶으면 ``` k.Player::Move() ```를 입력하면 되는데, 보통은 사용하지 않는다.

***

### 🌱 생성자와 소멸자
그렇다면 상속 받은 객체 간의 생성자와 소멸자의 호출 순서는 어떻게 될까?

각 객체에 기본 생성자와 소멸자를 만들어준 후 실행시켜보면 결과는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/cpp/oop/inheritance/const-dest.PNG)   

``` 부모 생성자 👉 자식 생성자 👉 자식 소멸자 👉 부모 소멸자``` 순으로 진행되는 것을 확인할 수 있다.

하지만 로우레벨 디테일로 살펴보면 약간 호출 시점이 다르다는 것을 확인할 수 있다. 어셈블리 코드를 살펴보자.

![Alt Text](/assets/images/posts_img/basics/cpp/oop/inheritance/asm1.PNG)   

우선 ``` Knight k; ```가 실행되고, 클래스가 생성이 된다. 처음엔 자식 클래스인 **Knight** 클래스가 생성이 되는 것이다. F11을 눌러 안으로 들어가보면 본격적으로 기본 생성자 내부를 실행시키기 전에 부모 생성자를 호출하는 것을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/oop/inheritance/asm2.PNG)   

여기서 부모 생성자 호출 부분에 브레이크 포인트를 잡고 내부로 들어가면 부모 생성자가 먼저 실행되는 것을 알 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/oop/inheritance/asm3.PNG)   

결론은 부모 생성자가 먼저 호출되는 게 아닌, **자식 생성자가 먼저 호출되었지만, 생성자 내부를 실행하기 전에 부모 생성자를 호출하게 되는 구조**이다. 소멸자도 비슷한 순서이다. 그대신 생성자와는 순서가 반대로 진행된다.

- **부모의 기본 생성자 외의 생성자를 호출하고 싶을 땐?**   

자식 생성자에서 부모 생성자가 무조건 호출이 된다는 것을 알게되었다. 그렇다면 부모의 기본 생성자 대신 기타 생성자를 호출하고 싶을 땐 어떻게 해야할까?

상속해준 것처럼 생성자에도 똑같이 적용하면 된다.

```c++
class Player
{
public:
    Player() { ... }
    Player(int hp) { ... }
    ~Player() { ... }
}

class Knight : public Player
{
public:
    Knight() { ... }
    Knight(int stamina) : Player(100) { ... }
}
```

위 처럼 똑같이 기타 생성자 뒤에 ``` : 부모 기타 생성자 ```를 추가하면 해당 생성자를 호출하라는 의미가 된다.

***

## 👻 글을 마치며
이번 시간에는 객체 지향 프로그래밍의 특징 중 상속성에 대해 알아보았다. 상속성이라는 단어, 그리고 부모-자식 간의 관계를 아주 오랜만에 접한 것이라 옛 기억이 새록새록 떠올랐다. 내가 관심있는 분야를 예제로 정하니 이해가 더욱 쉽고 빠르게 된 것 같고 재정의와 생성자, 소멸자의 호출 순서까지 한 번에 공부하게 되면서 굉장히 흥미로웠던 시간이었던 것 같다. 계층 구조를 잘 설계한다면 유지보수가 편한 코드를 만들 수 있지 않을까 하는 생각이 든다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/oop/inheritance)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   