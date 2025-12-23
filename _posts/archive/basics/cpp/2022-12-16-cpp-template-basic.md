---
title: "[C++] 템플릿(Template) 기초"
excerpt: "템플릿의 기초에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, function, callback, template]

permalink: /cpp/template-basic/

toc: true
toc_sticky: true

date: 2022-12-16 19:52:59+0900
last_modified_at: 2022-12-16 19:53:03+0900
---

## 👻 템플릿
**템플릿(Template)**이란, **함수나 클래스를 찍어내는 틀**이라는 의미이다. 함수 템플릿과 클래스 템플릿이 있다.

***

### 🌱 함수 템플릿
선언 및 사용 방법은 아래와 같다.

```c++
template<typename T>
void Print(T a)
{
    cout << a << endl;
}

template<typename T>
T Add(T a, T b)
{
    return a + b;
}

int main()
{
    Print(50);
    Print(50.0f);
    Print(50.0);
    Print("Hello World!");

    int result1 = Add(1, 2);
}
```

``` T ```는 템플릿의 이름이고 아무렇게나 입력할 수 있다. 위의 코드는 함수 ``` Print ```를 오버로딩 하지 않고 사용할 수 있게 해준다. 지정하고자 하는 타입을 고정시켜 주고 싶은 경우 아래와 같이 사용한다.

```c++
Print<int>(50.0f);

float result2 = Add<float>(1.11f, 2.22f);
```

> ``` typename ```도 ``` class ```로 바꿔 사용할 수 있다.

여러개의 타입을 지정하고 싶으면 **콤마(,)**를 이용하여 지정해줄 수 있다.

```c++
template<typename T1, typename T2>
void Print(T1 a, T2 b)
{
    cout << a << " " << b << endl;
}

int main()
{
    Print("Hello ", 100);
}
```

***

#### 🪐 템플릿 특수화
템플릿화 된 함수 템플릿 중, 예외를 처리할 때 사용하는 방식이다. 사용 방법은 아래와 같다.

```c++
template<>
void Print(Knight a)
{
    cout << "Knight !!!" << endl;
    cout << a._hp << endl;
}
```

템플릿 꺽쇠 안에 아무것도 입력하지 않으면 해당 함수가 입력받는 타입에 따라 유동적으로 예외 처리를 할 수 있게 된다.

***

### 🌱 클래스 템플릿
함수 템플릿과 사용 방법이 동일하다. 사용할 클래스 템플릿 위에 문법을 작성해주면 된다.

```c++
template<typename T>
class RandomBox
{
public:
    T GetRandomData()
    {
        int idx = rand() % 10;
        return _data[idx];
    }
public:
    T _data[10];
};

int main()
{
    srand(static_cast<unsigned int>(time(nullptr)));

    RandomBox<int> rb1;
    for (int i = 0; i < 10; i++)
        rb1._data[i] = i;

    int value1 = rb1.GetRandomData();
    cout << value1 << endl;

    RandomBox<float> rb2;
    for (int i = 0; i < 10; i++)
        rb2._data[i] = i + 0.5f;

    float value2 = rb2.GetRandomData();
    cout << value2 << endl;
}
```

``` typename ```을 붙이면 어떤 타입도 다 넣을 수 있다. 그런데 무조건 ``` typename ```을 붙여야 하는 것은 아니다. 기본 타입을 설정할 수도 있다.

```c++
template<typename T, int SIZE>
class RandomBox
{
public:
    T GetRandomData()
    {
        int idx = rand() % SIZE;
        return _data[idx];
    }
public:
    T _data[SIZE];
};

int main()
{
    srand(static_cast<unsigned int>(time(nullptr)));

    RandomBox<int, 10> rb1;
    for (int i = 0; i < 10; i++)
        rb1._data[i] = i;

    int value1 = rb1.GetRandomData();
    cout << value1 << endl;

    RandomBox<float, 20> rb2;
    for (int i = 0; i < 20; i++)
        rb2._data[i] = i + 0.5f;

    float value2 = rb2.GetRandomData();
    cout << value2 << endl;
}
```

얼핏보면 함수와 비슷하게 생각하여 ``` rb1 ```과 ``` rb2 ```가 같아보일 수 있지만 두 클래스는 엄연히 다른 상태를 가지는 클래스가 된다.   

👉 ``` rb1 = rb2 ```가 성립하지 않는다.

***

#### 🪐 템플릿 특수화
클래스 템플릿도 함수 템플릿과 마찬가지로 특수적인 상황에 대한 예외 처리를 할 수 있다. 단, 클래스는 오버로딩이 안 되므로 특수화한 템플릿이라는 것을 알려주기 위해 클래스명 뒤에 꺽쇠를 이용해 정보를 추가해준다.

```c++
template<int SIZE>
class RandomBox<double, SIZE>
{
public:
    double GetRandomData()
    {
        cout << "RandomBox Double" << endl;
        int idx = rand() % SIZE;
        return _data[idx];
    }
public:
    double _data[SIZE];
};

int main()
{
    srand(static_cast<unsigned int>(time(nullptr)));

    RandomBox<double, 20> rb3;
    for (int i = 0; i < 20; i++)
    {
        rb3._data[i] = i + 0.5;
    }
    double value3 = rb3.GetRandomData();
    cout << value3 << endl;
}
```

``` RandomBox ```의 초기 템플릿 자체가 ``` <typename T, int SIZE> ```이므로 해당 규칙을 지켜줘야 한다. 그래서 ``` SIZE ``` 인자 하나 밖에 변하지 않지만 규칙을 지키기 위해 ``` typename T ```에 해당하는 ``` double ```을 입력해주었다.

***

## 👻 글을 마치며
이번 시간에는 템플릿에 대해 간단하게 알아보았다. 템플릿이라는 걸 처음 본 것 같은데 그래도 의미 자체는 크게 어렵지 않아서 이해를 잘 할 수 있었던 것 같다. 근데 이제 오버로딩을 포함해 많은 기능을 알게 되고 동시에 사용하게 되니까 각 문법의 경계가 모호해져서 헷갈리는 부분이 새로 생기는 것 같다. 앞으로는 각 문법의 효율성까지 알아야 머릿속에서 정리가 될 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/callback-function/template-basic)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   