---
title: "[C++] 함수 객체(Functor)"
excerpt: "함수 객체에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, function, callback, functor]

permalink: /cpp/functor/

toc: true
toc_sticky: true

date: 2022-12-16 18:28:51+0900
last_modified_at: 2022-12-16 18:28:55+0900
---

## 👻 함수 객체
**함수 객체(Functor)**란 **함수처럼 동작하는 객체**이다. 이전 시간에 함수 포인터에 대해 알아보았다. 하지만 이 함수 포인터는 동작을 넘겨줄 땐 유용하지만 큰 단점이 있다.

> **함수 포인터의 단점**   
- 시그니처가 안 맞으면 사용할 수 없다.
- 상태를 가질 수 없다.

이러한 단점을 보완하기 위해 **함수 객체**를 사용한다. 선언 및 사용 방법은 아래와 같다.

```c++
class Functor
{
public:
    void operator()()
    {
        cout << "Functor Test" << endl;
        cout << _value << endl;
    }

    bool operator()(int num)
    {
        cout << "Functor(int) Test" << endl;
        _value += num;
        cout << _value << endl;

        return true;
    }
private:
    int _value = 0;
};

int main()
{
    Functor functor;
    functor();
    bool ret = functor(3);
}
```

우선 **() 연산자 오버로딩**이 필요하다. 객체 지향 클래스 문법을 이용하되 함수처럼 이용할 수 있도록 () 연산자 오버로딩을 하는 것이다.

함수 객체를 사용하게 되면 함수 포인터와 다르게 **생성 시점**과 **실행 시점**을 분리할 수 있다.

***

### 🌱 활용 예
MMORPG를 만들 때 서버 쪽에서 유용하게 활용할 수 있다. 예를 들어 클라가 보내준 네트워크 패킷을 받아서 서버가 처리할 때를 생각해보자. 만약 현재 처리하는 일이 많아서 요청은 받았지만 처리를 기다려야 할 때, ``` Functor ```를 이용해 해당 기능을 처리해야 한다고 생성만 해둔 다음 여유가 될 때 일감을 실행시키게 되는 것이다.

***

## 👻 글을 마치며
이번 시간에는 함수 객체에 대해 알아보았다. 간단하게 연산자 오버로딩을 통해 쉽게 만들 수 있었다. 함수 포인터가 가진 단점을 보완해줄 수 있는 정도로만 알아두면 좋을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/callback-function/functor)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   