---
title: "[C++] 다형성(Polymorphism)"
excerpt: "객체 지향 프로그래밍의 특징 중 하나인 다형성에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, polymorphism]

permalink: /cpp/polymorphism/

toc: true
toc_sticky: true

date: 2022-11-27 16:51:21+0900
last_modified_at: 2022-11-27 16:51:24+0900
---
 
## 👻 다형성
**다형성(Polymorphism)**이란 같은 이름을 사용하여 다른 기능을 구현하는 것을 의미한다. 동명이인, 동음이의어에 비유하면 이해가 쉽다. 대표적으로 **오버로딩**과 **오버라이딩**이 있다.

***

### 🌱 오버로딩(Overloading)
**오버로딩(Overloading)**은 함수를 **중복 정의**하는 것을 의미한다. 이전에 함수를 구현할 때 이름은 같지만 인자의 개수나 타입을 다르게 해서 만든 적이 있었다. 이러한 부분이 바로 함수 오버로딩에 해당된다.

```c++
class Player
{
public:
    void Move() { cout << "Move Player !" << endl; }

    void Move(int a) { cout << "Move Player (int) !" << endl; }
public:
    int _hp;
};
```

클래스 내부에 ``` Move ``` 함수를 두 번 중복 정의 하였다. 함수가 오버로딩 된 것이다.

***

### 🌱 오버라이딩
**오버라이딩(Overriding)**은 함수를 **재정의**하는 것을 의미한다. 상속성에 대해 알아보았을 때 부모 자식 클래스 간의 상속이 일어나게되고, 부모의 멤버 함수를 자식의 멤버 함수로 다시 만든 적이 있었다. 이 부분이 바로 함수 오버라이딩에 해당된다.

```c++
class Player
{
public:
    void Move() { cout << "Move Player !" << endl; }
public:
    int _hp;
};

class Knight : public Player
{
public:
    void Move() { cout << "Move Knight !" << endl; }
public:
    int _stamina;
};
```

부모 클래스인 ``` Player ```의 ``` Move ``` 함수를 자식 클래스인 ``` Knight ``` 에서 다시 한 번 더 만듦으로써 함수가 오버라이딩 되었다고 볼 수 있다.

***

### 🌱 바인딩
**바인딩(Binding)**은 묶는다라는 뜻을 가진 단어로 프로그래밍에선 **프로그램에 사용된 구성 요소(각종 값들)가 확정되어 더 이상 변경할 수 없는 상태**가 되는 것을 의미한다. **정적 바인딩**과 **동적 바인딩**이 있다.

***

#### 🪐 정적 바인딩
**정적 바인딩(Static Binding)**은 **컴파일 시점에 결정**되어 값이 묶여지는 것을 의미한다. 보통 일반적인 전역변수나 값이 바뀌지 않을 각종 값들이 정적 바인딩된다.

```c++
void MovePlayer(Player* player)
{
    player->Move();
}

int main()
{
    Knight k;
    MovePlayer(&k);
}
```

다음과 같은 부모 클래스의 포인터를 받는 전역 함수가 있다고 가정해보자. 그런 다음 메인 함수에서 ``` Knight ``` 클래스를 생성 후 주소값을 넘겨주면 에러없이 잘 실행되는 것을 알 수 있다.

하지만 함수에서 받는 인자값이 플레이어의 포인터이므로 플레이어 클래스 내의 ``` Move ``` 함수가 실행된다.

해당 함수는 컴파일 시 이미 받는 값, 넘겨줄 값 등이 고정되어버려 내가 함수를 호출했을 때 받는 인자의 값이 무엇인지는 알지 못하고 그냥 전달만 할 뿐이기 때문이다.

***

#### 🪐 동적 바인딩
**동적 바인딩(Dynamic Binding)**은 **실행 시점에 결정**되어 값이 묶여지는 것을 의미한다. 위처럼 일반 함수를 부모 클래스 타입으로 넓게 잡아 사용하면 자식 클래스까지 사용할 수 있다는 장점이 있지만 정적 바인딩으로 인해 어떤 클래스인지까지는 알지 못한다. 그러한 점을 보완하기 위해 동적 바인딩으로 변경해주어야한다.

동적 바인딩을 원한다면 **가상 함수(Virtual Function)**을 사용해야한다. 함수 앞에 ``` virtual ``` 을 붙여주어 만들 수 있다.

```c++
class Player
{
public:
    void Move() { cout << "Move Player !" << endl; }
    virtual void VMove() { cout << "VMove Player !" << endl; }
public:
    int _hp;
};

class Knight : public Player
{
public:
    void Move() { cout << "Move Knight !" << endl; }
    // 가상 함수는 재정의를 하더라도 가상 함수이다.
    virtual void VMove() { cout << "VMove Knight !" << endl; }
public:
    int _stamina;
};

void MovePlayer(Player* player)
{
    player->VMove();
}
```

부모 자식 클래스 각각에 가상 함수를 정의, 재정의 해준 다음 일반 전역 함수 내에서 실행할 함수를 가상 함수로 설정하고 다시 실행시키면 이번엔 자식 클래스인 ``` Knight ```의 가상 함수 ``` VMove ```가 실행된다. 이렇게 되면 값을 유연하게 넘기고 처리할 수 있다는 장점이 있다.

***

### 🌱 가상 함수
**가상 함수(Virtual Function)**는 정의, 재정의 되어지는 멤버 함수 중 동적으로 바인딩을 해주기 위해 사용하는 함수이다. 위에서 보았던 것처럼 ``` virtual ``` 문법을 사용해 가상 함수를 정의할 수 있다.

> 💡 **가상 함수를 어떻게 호출하는 걸까?**   
일반 함수와 다를 바가 없는데 어떻게 가상 함수인지 알고 유연하게 호출해주는 걸까?   
메모리를 살펴보면 **가상 함수 테이블(vftable)**을 사용한 것을 알 수 있다. 가상 함수 테이블은 가상 함수의 주소값을 담아놓은 테이블이다. 보통 클래스의 기본 생성자가 실행되기 전 생성되며 해당 값을 통해 가상 함수 유무를 알게 되고 알아서 호출해주는 것이다.   
![Alt Text](/assets/images/posts_img/basics/cpp/oop/polymorphism/vftable.PNG)   
![Alt Text](/assets/images/posts_img/basics/cpp/oop/polymorphism/vftable2.PNG)   
해당 주소값으로 가면 가상 함수 테이블이 하나 있고 그 다음 메모리부터 설정한 멤버 변수의 값이 차례로 들어가는 것을 확인할 수 있다.

***

#### 🪐 순수 가상 함수
**순수 가상 함수**는 구현은 없고 **인터페이스**만 전달하는 용도로 사용하고 싶을 경우에 사용한다.

```c++
class Player
{
public:
    virtual void VAttack() = 0;
};
```

위 처럼 함수 선언부에 ``` = 0; ```을 붙여 만들 수 있다.

순수 가상 함수가 1개 이상 포함되면 해당 클래스는 바로 **추상 클래스**로 간주하게 되고 **직접적으로 객체를 만들 수 없게 된다.** 위처럼 플레이어 클래스를 만들었다면 외부에서 ``` Player p; ```로 선언할 수 없다는 의미가 된다.

또한, 추상 클래스를 상속받은 자식 클래스는 반드시 **순수 가상 함수의 구현부를 설정**해야 사용할 수 있게된다.

***

## 👻 글을 마치며
이번 시간에는 객체 지향 프로그래밍의 특징 중 다형성에 대해 알아보았다. 오버로딩과 오버라이딩의 차이까지는 알았는데 바인딩, 가상 함수는 처음 알게되었다. 생각보다 프로그래밍 과정에서 많은 처리 단계가 있다는 걸 알게되었고 경우에 따라 적절한 방법을 사용해야겠다는 것을 배우게 되었다. 아직까지는 가상 함수의 응용법이 생각이 잘 안 나지만 여러 실습을 통해 금방 익힐 수 있을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/oop/polymorphism)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   