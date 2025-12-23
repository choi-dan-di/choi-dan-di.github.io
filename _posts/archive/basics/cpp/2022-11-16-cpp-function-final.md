---
title: "[C++] 오버로딩, 기본 인자값, 스택 오버플로우"
excerpt: "함수의 나머지 부분에 대해 정리하기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, function, overloading, default value, stack overflow]

permalink: /cpp/function-final/

toc: true
toc_sticky: true

date: 2022-11-16 20:37:48+0900
last_modified_at: 2022-11-16 20:37:52+0900
---

## 👻 오버로딩
**오버로딩(Overloading)**은 **함수의 중복 정의**를 의미한다. **함수 이름의 재사용**이라고 볼 수 있다.

덧셈과 관련된 함수를 만든다고 생각해보자. 각 데이터는 타입이 존재한다. 실수와 정수를 엄연히 구분하게 되고 결과값도 지정한 타입에 따라 달라지게된다. 정수의 덧셈과 실수의 덧셈을 해주는 함수를 각각 만들어보자.

```c++
int Add(int a, int b)
{
    int result = a + b;
    return result;
}

float AddFloat(float a, float b)
{
    float result = a + b;
    return result;
}
```

이렇게 함수를 정의하고 호출을 할 때 사용하려면 각각 ``` Add ```와 ``` AddFloat ``` 함수를 번갈아 사용해야한다. 덧셈이라는 같은 기능을 가지고 있는데도 다른 이름으로 말이다. 이러한 과정을 한 단계 줄여주기 위해 **C++**에서는 오버로딩 기능을 제공한다. 기능이 같으면 중복 정의를 허용한다는 뜻이다.

```c++
int Add(int a, int b)
{
    int result = a + b;
    return result;
}

float Add(float a, float b)
{
    float result = a + b;
    return result;
}
```

이렇게 덧셈이라는 함수를 중복 정의함으로써 호출 시에 조금 더 간편하게 사용할 수 있게 된다. 그대신 타입은 해당 결과에 맞게 설정해줘야하고 **매개 변수의 타입, 개수가 완전히 동일한 중복 정의**는 할 수 없다. 이름만 같다!

***

## 👻 기본 인자값
함수의 매개 변수 중, 필수값이 아닌 변수의 초기값을 설정해주면 입력을 받지 않아도 자동 세팅된다. 단, 필수값 뒤 마지막 부분에 위치해야하며 순서도 지켜줘야한다. **C#**과 다르게 특정 변수를 지정하여 값을 세팅할 수 없다는 단점이 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/function-final/default.PNG)   

```c++
SetPlayerInfo(100, 40, 10, 1);
```

위 코드를 실행하게되면 마지막 인자인 ``` castleId ```의 값은 자동으로 **2**가 들어가게된다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/function-final/result.PNG)   

***

## 👻 스택 오버플로우
**스택 오버플로우(Stack Overflow)**는 스택에 너무 많은 정보가 저장되었을 때 메모리 용량을 초과해버리는 상황을 의미한다. 재귀함수를 이용해 확인해보자.

```c++
int Factorial(int n)
{
    if (n <= 1)
        return 1;

    return n * Factorial(n - 1);
}
```

간단하게 재귀함수를 만들어주고, 매개 변수로 **1000000(백만)**을 넣은 후 디버깅해보자.

![Alt Text](/assets/images/posts_img/basics/cpp/function/function-final/error.PNG)   

한참 실행하다가 예외가 처리되지 않는다는 에러가 뜨게된다. 스택을 초과해서 스택 오버플로우가 발생한 것이다. 이러한 상황을 해결하려면 스택 오버플로우가 우려될 때는 반드시 **예외 처리**를 해줘야한다.

***

## 👻 글을 마치며
이번 시간에는 함수 마무리 겸 오버로딩, 기본 인자값, 스택 오버플로우에 대해 알아보았다. 항상 함수를 새로 생성할 때는 고민의 시간을 가지게 된다. 기존에 있는 함수를 조금 더 활용할 수 있지 않는지, 너무 비슷한 기능이진 않는지 하고 말이다. 약간은 비슷하지만 결과적으로 보면 다른 함수들, 그런 함수들을 오버로딩을 이용해서 조금이라도 더 정리시킬 수 있는 게 되게 좋은 부분같다. 기본 인자값도 좋은 기능이긴한데 순서를 반드시 지켜줘야해서 조금은 불편할 것 같다는 생각이 들었다. 그래도 쓰다보면 아주 잘 쓰일 것 같은 기능들인 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/function/function-final)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   