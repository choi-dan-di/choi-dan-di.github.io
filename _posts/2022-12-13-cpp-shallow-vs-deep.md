---
title: "[C++] 얕은 복사 vs 깊은 복사"
excerpt: "복사 방법에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, shallow copy, deep copy, copy]

permalink: /cpp/shallow-vs-deep/

toc: true
toc_sticky: true

date: 2022-12-13 17:48:06+0900
last_modified_at: 2022-12-13 17:48:08+0900
---
 
## 👻 얕은 복사 vs 깊은 복사
C++에서 메모리의 값을 복사하는 방법에 대해 알아보자.

우선, 기사 클래스와 펫 클래스가 있다고 가정해보자. 기사는 펫을 가질 수 있다.

```c++
class Pet
{
public:
    Pet()
    {
        cout << "Pet()" << endl;
    }
    
    ~Pet()
    {
        cout << "~Pet()" << endl;
    }

    Pet(const Pet& pet)
    {
        cout << "Pet(const Pet& pet)" << endl;
    }
};

class Knight
{
public:

public:
    int _hp = 100;
    // Pet _pet;
    Pet* _pet;
};
```

***

### 🌱 얕은 복사
**얕은 복사(Shallow Copy)**는 **멤버 데이터를 비트열 단위로 똑같이 복사**하는 방식을 의미한다. 메모리 영역 값을 그대로 복사한다.

메인 함수에서 기사를 복사해보자.

```c++
int main()
{
    Pet* pet = new Pet();

    Knight knight;  // 기본 생성자
    knight._hp = 200;
    knight._pet = pet;

    Knight knight2 = knight;    // 복사 생성자
    // Knight knight3(knight);  // 같은 의미

    Knight knight3; // 기본 생성자
    knight3 = knight;   // 복사 대입 연산자 (오버로딩)

    return 0;
}
```

위처럼 다양한 방법으로 복사를 할 수 있다. 복사 생성자와 복사 대입 연산자는 우리가 클래스 내에 생성자를 따로 지정해 주지 않아도 컴파일러가 **암시적으로** 만들어주기 때문에 별다른 이상없이 복사를 할 수 있게된다. 하지만 이렇게 한다면 총 3명의 기사가 힙 영역에 있는 하나의 펫을 같이 가리키게 되는 문제가 발생한다.

***

### 🌱 깊은 복사
**깊은 복사(Deep Copy)**는 **멤버 데이터가 참조(주소) 값이라면, 데이터를 새로 만들어 복사**하는 방식을 의미한다. 원본 객체가 참조하는 대상까지 새로 만들어서 복사한다. 얕은 복사에서 발생하는 문제점을 해결하기 위한 복사 방식이다.

복사 생성자와 복사 대입 연산자를 **명시적으로** 지정해주어 깊은 복사를 시행할 수 있다.

```c++
class Knight
{
public:
    Knight()
    {
        _pet = new Pet();
    }

    // 복사 생성자
    Knight(const Knight& knight)
    {
        _hp = knight._hp;
        _pet = new Pet(*(knight._pet)); // 깊은 복사
    }

    // 복사 대입 연산자
    Knight& operator=(const Knight& knight)
    {
        _hp = knight._hp;
        _pet = new Pet(*(knight._pet)); // 깊은 복사
        return *this;
    }

    ~Knight()
    {
        delete _pet;
    }
public:
    int _hp = 100;
    // Pet _pet;
    Pet* _pet;
};
```

이렇게 사용하게되면 기사 클래스의 복사가 이루어질 때마다 펫을 가리키는 주소도 새로 생성되어 기사 객체마다 따로따로 할당받게 되는 것이다. 클래스 내에 다른 클래스의 참조(주소)값을 담고 있을 땐 반드시 깊은 복사를 우선으로 생각해야한다.

***

### 🌱 암시적 vs 명시적
암시적, 명시적으로 복사가 될 때의 과정을 살펴보자.

- **암시적으로 복사 생성자가 호출될 때**
1. 부모 클래스의 복사 생성자 호출
2. 멤버 클래스의 복사 생성자 호출
3. 멤버가 기본 타입일 경우 메모리 복사 (얕은 복사 Shallow Copy)

- **명시적으로 복사 생성자가 호출될 때**
1. 부모 클래스의 기본 생성자 호출
2. 멤버 클래스의 기본 생성자 호출

- **암시적으로 복사 대입 연산자가 호출될 때**
1. 부모 클래스의 복사 대입 연산자 호출
2. 멤버 클래스의 복사 대입 연산자 호출
3. 멤버가 기본 타입일 경우 메모리 복사 (얕은 복사 Shallow Copy)

- **명시적으로 복사 대입 연산자가 호출될 때**   
알아서 해주는 게 없다.

> **왜 이렇게 호출될까?**   
👉 객체를 **복사** 한다는 것은 두 객체의 값들을 일치시키려는 것이다. 따라서 기본적으로 얕은 복사(Shallow Copy) 방식으로 동작하게 된다. 명시적 복사는 **모든 책임을 프로그래머에게 위임하겠다**는 의미를 가진다.
>
> 👉 명시적으로 복사 생성자 혹은 복사 대입 연산자를 지정해주었을 때 연관된 다른 클래스들의 복사와 관련된 함수를 똑같이 명시적으로 설정해주지 않으면 일부만 복사되는 문제가 발생할 수 있다. 명시적으로 복사를 하고 싶을 땐 모두 직접 관리해줘야 원하는 방식으로 복사를 진행할 수 있게 된다.

***

## 👻 글을 마치며
이번 시간에는 얕은 복사와 깊은 복사에 대해 알아보았다. 우리가 일반적으로 사용했던 방식은 얕은 복사였고, 클래스의 크기가 커지고 관계성이 깊어질 때 깊은 복사를 사용해야 하는 것을 알 수 있었다. 개념 이해는 잘 된 것 같지만 좀 더 확실하게 하려면 실습을 통해 꾸준히 복습해야 할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/heap/shallow-vs-deep)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   