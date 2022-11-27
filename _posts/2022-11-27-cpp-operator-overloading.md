---
title: "[C++] 연산자 오버로딩(Operator Overloading)"
excerpt: "연산자 오버로딩에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, operator overloading, overloading]

permalink: /cpp/operator-overloading/

toc: true
toc_sticky: true

date: 2022-11-27 20:02:32+0900
last_modified_at: 2022-11-27 20:02:35+0900
---
 
## 👻 연산자 오버로딩
**연산자 오버로딩(Operator Overloading)**은 말 그대로 **연산자를 오버로딩, 즉 중복 정의 한다**는 의미이다. 일반적인 변수들을 더하고 싶을 땐 ``` + ``` 기호를 사용하면 문제없이 잘 더해지지만 클래스 타입인 경우 문제가 생기게 된다.

```c++
class Position
{
public:
    int _x;
    int _y;
}

int main()
{
    Position pos;
    pos._x = 0;
    pos._y = 0;

    Position pos2;
    pos2._x = 1;
    pos2._y = 1;

    Position pos3 = pos + pos2;     // error
}
```

![Alt Text](/assets/images/posts_img/basics/cpp/oop/operator-overloading/error.PNG)   

위처럼 에러가 뜨게된다. 이러한 연산을 허용해주기 위해 **연산자 함수**를 사용하여 연산자 오버로딩을 해줘야 한다.

> - 모든 연산자를 오버로딩 할 수 있는 것은 아니다. (``` :: ```, ``` . ```, ``` .* ``` 이런 건 오버로딩이 불가능하다.)   
- 모든 연산자가 2개 항이 있는 것은 아니다. (증감 연산자 ``` ++ ```, ``` -- ```가 대표적인 단항 연산자이다.)

***

### 🌱 연산자 함수
```c++
[반환 타입] operator[기호]([매개 변수])
```

일단 연산자 오버로딩을 하기 위해선 **연산자 함수**를 정의해야 한다. 함수도 멤버 함수와 전역 함수가 존재하는 것처럼, 연산자 함수도 **멤버 연산자 함수**와 **전역 연산자 함수**로 만들 수 있다.

연산자 함수의 이름은 ``` operator ``` 뒤에 중복 정의하고 싶은 연산자의 기호를 입력한다.

***

#### 🪐 멤버 연산자 함수
``` a op b ```에서 왼쪽을 기준으로 실행된다. 단, ``` a ```가 클래스여야 가능하며 ``` a ```를 **기준 피연산자**라고 한다. 하지만 멤버 연산자 함수는 ``` a ```가 **클래스가 아니면** 사용하지 못한다.

```c++
class Position
{
public:
    // 멤버 연산자 함수
    Position operator+(const Position& arg)
    {
        Position pos;
        pos._x = _x + arg._x;
        pos._y = _y + arg._y;
        return pos;
    }

    Position operator+(int arg)
    {
        Position pos;
        pos._x = _x + arg;
        pos._y = _y + arg;
        return pos;
    }

    bool operator==(const Position& arg)
    {
        return _x == arg._x && _y == arg._y;
    }

    void operator=(int arg)
    {
        _x = arg;
        _y = arg;
    }

public:
    int _x;
    int _y;
};
```

> 대입 연산자의 경우 연속으로 사용할 수 있다. 하지만 연산자 함수 중복 정의 시 반환 타입을 ``` void ```로 지정하게 되면 연속으로 사용할 수 없게된다. 연속으로 사용하고 싶으면 자기 자신을 반환값으로 지정해주면 된다.
> 
> ```c++
> Position& operator=(int arg)
> {
>     _x = arg;
>     _y = arg;
> 
>     return *this;
> }
> 
> int main()
> {
>     pos5 = (pos4 = 3);  // 가능
> }
> ```

이런 식으로 멤버 연산자 함수를 정의할 수 있고 외부에서 접근 시 아래처럼 사용 가능하다.

```c++
int main()
{
    Position pos3 = pos + pos2;
    // pos3 = pos.operator+(pos2)와 같은 의미

    Position pos4 = pos3 + 1;

    bool isSame = (pos3 == pos4);

    Position pos5;
    pos5 = 5;
    // Position pos5 = 5와는 다른 의미
}
```

여기서 ``` pos4 ``` 구문의 순서를 반대로 하여 연산을 하면 에러가 나게된다. 고로 멤버 연산자 함수는 왼쪽 피연산자가 기본이 되므로 반드시 클래스 타입을 지녀야한다. 이러한 에러를 올바르게 해결하려면 전역 연산자 함수를 사용하면 된다.

***

#### 🪐 전역 연산자 함수
전역 연산자 함수는 ``` a op b ```에서 a, b 모두를 연산자 함수의 **피연산자**로 만들어준다. 그래서 멤버 연산자 함수와는 다르게 순서가 상관없다.

```c++
Position operator+(int a, const Position& b)
{
    Position ret;
    ret._x = b._x + a;
    ret._y = b._y + a;
    return ret;
}
```

전역 연산자 함수를 위처럼 선언해주면 ``` Position pos4 = 1 + pos3; ```도 사용 가능하다.

> 하지만 **대입 연산자(**``` = ```**)**는 사용할 수 없다. 전역으로 풀어주게되면 왼쪽 값이 오른쪽으로 대입될 수도 있는 위험한 상황이 발생하기 때문에 막아둔 것이다.

***

### 🌱 복사 대입 연산자
**복사 대입 연산자**는 대입 연산자 중 **자기 자신의 참조 타입을 인자로 받는 것**이다. _복사 생성자, 복사 대입 연산자 등_ 복사 개념이 특별 대우를 받는 이유는 말 그대로 객체가 **복사**되길 원하는 특징 때문이다. 그래서 복사 생성자 같은 경우도 따로 정의해주지 않으면 컴파일러가 자동으로 생성해주기 때문에 일반적인 복사는 가능하다.

복사 대입 연산자는 말 그대로 복사해서 대입해준다는 의미이다.

```c++
class Position
{
public:
    // 복사 대입 연산자
    Position& operator=(const Position& arg)
    {
        _x = arg._x;
        _y = arg._y;

        return *this;
    }

public:
    int _x;
    int _y;
};
```

> 참조 타입을 인자로 받는 경우엔 대부분 ``` const ```를 붙여주는 게 좋다.

***

### 🌱 증감 연산자
증감 연산자의 경우 **전위형**과 **후위형**이 있다. 우선 테스트를 해보자.

```c++
int d = ++(++c);
int e = (d++)++;
```

``` d ```의 경우 가능하지만 ``` e ```의 경우 불가능하다. 이러한 원래의 기능을 최대한 살려서 정의해보면 다음과 같이 구현할 수 있다.

> - **전위형** : ``` operator++() ```   
- **후위형** : ``` operator++(int) ```   
>
> 👉 쉽게 구분하기 위해 인자에 ``` int ```를 사용한다.

```c++
class Position
{
public:
    // 전위형
    Position& operator++()
    {
        _x++;
        _y++;
        return *this;
    }

    // 후위형
    void operator++(int)
    {
        _x++;
        _y++;
    }

public:
    int _x;
    int _y;
};
```

전위형은 계산이 끝난 후 자기 자신을 반환해야 연속으로 계산이 가능하다. 반대로 후위형은 현재 자기 자신의 값을 반환하고 ``` 세미콜론(;) ```을 지난 다음 바뀐 값이 적용되기 때문에 연속으로 계산이 불가능하다. 이러한 점을 살려 전위형은 참조값을 반환하고, 후위형은 값만 변화시켰다.

**증감 연산자**와 **대입 연산자**를 동시에 사용해보자.

```c++
int main()
{
    pos5 = pos3++;
}
```

증감 연산자를 한 번 실행하고 대입 연산자를 실행하는 구문이다. 하지만 후위형은 반환값이 없으므로 해당 구문에서 대입 연산 시 에러가 나게된다.

``` pos5 ```는 복사 대입 연산자의 **기준 피연산자**로 자기 자신의 참조값을 반환 타입으로 받고 있지만 ``` pos3++ ```은 반환값이 없기 때문이다.

```c++
Position operator++(int)
{
    Position ret = *this;
    _x++;
    _y++;
    return ret;
}
```

후위형의 반환값을 복사값으로 바꿔서 넘기게 되면 이제 값은 존재한다. 하지만 여전히 **타입은 일치하지 않는다.**

``` pos5 ```는 ``` Position& ```을, ``` pos3++ ```은 ``` Position ```을 넘겨주기 때문이다. 여기서 ``` pos5 ```가 값을 전달 받기 위해서는 참조값으로 받는 매개 변수(인자) 앞에 ``` const ```를 붙여주어 받게 되면 에러를 해결할 수 있다.

***

## 👻 글을 마치며
이번 시간에는 연산자 오버로딩에 대해 알아보았다. 이 문법은 처음 보는 문법이었지만 오버로딩의 의미를 알고 있었기 때문에 어렵지 않게 이해할 수 있었다. 연산자 오버로딩을 할 일이 그리 많지는 않을 것 같지만 외워두지 않으면 해석하기 어려울 것 같다. 역시 객체와 관련된 연산은 항상 헷갈리는 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/oop/operator-overloading)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   