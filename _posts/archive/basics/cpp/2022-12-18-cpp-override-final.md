---
title: "[C++] override와 final"
excerpt: "Modern C++ 문법에서 사용되는 override와 final에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, modern cpp, modern, override, final]

permalink: /cpp/override-final/

toc: true
toc_sticky: true

date: 2022-12-18 18:18:46+0900
last_modified_at: 2022-12-18 18:18:47+0900
---

## 👻 override
``` override ```는 **상속받은 클래스 사이에서 부모의 멤버 함수를 자식이 그대로 물려받아 재사용 한다는 의미**를 나타내는 문법이다. 그냥 재정의 해도 상관없지만 그렇게 되면 어디서부터 시작인지 알기가 힘들기 때문에 가독성은 높이고 에러 발생은 줄이기 위해 함수의 이름 뒤에 ``` override ```를 붙여 관리한다.

```c++
class Creature {
public:
    virtual void Attack() {
        cout << "Creature!" << endl;
    }
};

class Player : public Creature {
public:
    virtual void Attack() override {
        cout << "Player!" << endl;
    }
};

class Knight : public Player {
public:
    // override를 붙여줌으로써 Player로부터 해당 함수를 상속 받아
    // 재정의 하고 있다는 사실을 알 수 있다.
    virtual void Attack() override {
        cout << "Knight!" << endl;
    }

private:
    int _stamina = 100;
};
```

> 💡 **함수명 뒤에** ``` const ``` **를 붙이면?**   
다음과 같이 클래스가 정의되어 있다고 가정하자.
>
> ```c++
> class Player {
> public:
>     virtual void Attack() {
>         cout << "Player!" << endl;
>     }
> };
> 
> class Knight : public Player {
> public:
>     // 재정의 성립 안 됨
>     // readonly
>     virtual void Attack() const {
>         cout << "Knight!" << endl;
>         // _stamina = 10;   // 불가능
>     }
> 
> private:
>     int _stamina = 100;
> };
> ```
> 
> 해당 함수처럼 ``` const ```가 붙게 되면 **해당 함수 내에서 아무것도 수정할 수 없다**는 의미이다. 그리고 ``` const ```를 붙임으로써 부모 클래스의 ``` Attack ``` 함수와 **시그니처**가 달라지므로 둘은 재정의 된 관계가 아닌 각자 다른 별개의 멤버 함수라고 볼 수 있다.

***

## 👻 final
``` final ```은 ``` override ```와 마찬가지로 재정의에 대한 상태를 나타내는데, 단어 뜻 그대로 **이번 재정의를 끝으로 더이상 재정의를 허락하지 않겠다**는 의미이다. 혹여나 나를 상속받는 자식 클래스가 생긴다면, 자식 클래스에선 ``` final ```이 붙은 함수를 더이상 재정의하지 못한다.

```c++
class Creature {
public:
    virtual void Attack() {
        cout << "Creature!" << endl;
    }
};

class Player : public Creature {
public:
    // 나를 끝으로 더이상 재정의하지 않겠다.
    virtual void Attack() final {
        cout << "Player!" << endl;
    }
};

class Knight : public Player {
public:
    // 재정의 불가능!!!
    virtual void Attack() {
        cout << "Knight!" << endl;
    }

private:
    int _stamina = 100;
};
```

***

## 👻 글을 마치며
이번 시간에는 override와 final에 대해 알아보았다. 이제 코드에 살이 붙는다는 느낌이 점점 가독성이 좋은 쪽으로 붙고 있다는 느낌이 많이 든다. 로직은 대부분 비슷비슷한데 어떤 단어를 사용하느냐에 따라 가독성의 편차가 더욱 크게 나타나는 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/modern-cpp/override-final)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   