---
title: "[C++] 함수 포인터(Function Pointer)"
excerpt: "함수 포인터에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, function, callback, pointer, function pointer]

permalink: /cpp/function-pointer/

toc: true
toc_sticky: true

date: 2022-12-16 18:17:47+0900
last_modified_at: 2022-12-16 18:17:51+0900
---

## 👻 함수 포인터
**함수 포인터(Function Pointer)**는 **함수를 데이터 타입으로 가지는 포인터**를 의미한다. 이전 시간에 배웠던 포인터는 일반적인 변수의 주소값을 담고 있었다. 함수 포인터는 함수의 시작 주소를 담으며, 함수의 이름은 함수의 시작 주소를 의미한다.(배열과 유사하다.)

```c++
int Add(int a, int b)
{
    return a + b;
}

int Sub(int a, int b)
{
    return a - b;
}
```

다음과 같이 덧셈과 뺄셈의 기능을 가지는 두 함수가 있다고 가정해보자. 두 함수 모두 두 개의 int 타입 인자를 받고 int를 반환한다. 함수 포인터 선언은 다음과 같다.

```c++
[반환 타입](함수 포인터 이름)(인자)
```

보통은 길기 때문에 ``` typedef ```를 이용하여 이름을 따로 지정해줄 수 있다.

```c++
typedef [반환 타입](함수 포인터 이름)(인자);
```

위의 두 함수를 가리키는 포인터는 다음과 같이 선언 및 사용할 수 있다.

```c++
// typedef 지정
typedef int(FUNC_TYPE)(int, int);

FUNC_TYPE* fn;
fn = Add;

int result = fn(1, 2);
// 함수 포인터는 *(접근 연산자)가 붙어도 함수 주소를 가리키는 것은 똑같다.
int result2 = (*fn)(1, 2);

// typedef 미지정
int(*fn2)(int, int) = Add;
int result3 = fn2(3, 5);
```

***

### 🌱 사용하는 이유
> **여러 줄의 코드를 한 번에 관리할 수 있다.**

위의 코드와 같이 함수를 중복 호출하는 코드가 있을 때, ``` Add ``` 함수를 사용하다 ``` Sub ``` 함수로 변경하려면 여러 줄의 코드를 일일이 수정해야한다는 문제점이 있다. 함수 포인터를 사용하면 포인터를 지정한 한 줄만 변경함으로써 여러 줄의 코드를 한 번에 변경할 수 있다.

또한, 여러 개의 조건을 통해 검색한다는 가정하에 검색하는 함수를 만들 때, 원래라면 조건대로 함수를 중복 생성해야 하지만 함수 포인터를 사용하면 해당 인자를 받는 함수 하나로 여러 개의 조건문을 포함한 함수를 만들 수 있게 된다.

```c++
typedef bool(ITEM_SELECTOR)(Item*, int);
// ITEM_SELECTOR* selector == bool(*selector)(Item*)
Item* FindItem(Item items[], int itemCount, ITEM_SELECTOR* selector, int value)
{
    for (int i = 0; i < itemCount; i++)
    {
        Item* item = &items[i];
        // TODO 동작
        if (selector(item, value))
            return item;
    }

    return nullptr;
}

bool IsRareItem(Item* item, int value)
{
    return item->_rarity >= value;
}

bool IsOwnerItem(Item* item, int ownerId)
{
    return item->_ownerId == ownerId;
}
```

***

### 🌱 멤버 함수 포인터
위의 함수 포인터 선언 방식은 일반 함수(전역 함수, 정적 함수)에만 해당된다. **멤버 함수 포인터** 선언 방법에 대해 알아보자.

```c++
class Knight
{
public:
    // 정적 함수
    static void HelloKnight()
    {
        cout << "HelloKnight()" << endl;
    }

    // 멤버 함수
    int GetHp()
    {
        cout << "GetHp()" << endl;
        return _hp;
    }
public:
    int _hp = 100;
};
```

위와 같이 ``` Knight ``` 클래스가 있다고 가정하자. 정적 함수인 ``` HelloKnight ```는 일반 함수 포인터 선언 방법으로 사용할 수 있지만 멤버 함수인 ``` GetHp ```는 사용할 수 없다. 이러한 멤버 함수 포인터의 선언 및 사용 방법은 다음과 같다.

```c++
// 선언
typedef int(Knight::*PMEMFUNC)();
PMEMFUNC mfn;
mfn = &Knight::GetHp;

int(Kngiht::*mfn2)() = &Knight::GetHp;

// 사용
Knight k1;
(k1.*mfn)();

Knight* k2 = new Knight();
// 같은 의미
((*k2).*mfn)();
(k2->*mfn)();
delete k2;
```

> - 일반 함수 포인터는 ``` & ```를 생략할 수 있지만 멤버 함수 포인터의 경우, ``` & ```를 반드시 붙여줘야한다. 고로 웬만하면 모두 ``` & ```를 붙이는 게 좋다.
- 혹여나 함수 포인터와 같은 이름의 멤버 변수나 멤버 함수가 존재할 수 있기 때문에 그러한 중복 문제를 방지하기 위해 멤버 함수 포인터의 앞엔 접근 연산자 ``` * ```을 붙인다.

***

## 👻 글을 마치며
이번 시간에는 함수 포인터에 대해 알아보았다. 일반 포인터만 알고 있었는데 이번 시간에 처음 알아보는 파트였다. 하지만 타입만 함수 타입일 뿐 나머지는 모두 동일하게 동작해서 크게 어렵지 않았다. ~~약간의 C++ 특유의 괴랄한 문법이 눈에 익지 않을뿐..~~ 함수 포인터가 얼마나 자주 쓰일지는 아직까진 가늠이 되진 않지만 익혀두면 반드시 유용하게 쓸 곳이 있을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/callback-function/function-pointer)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   