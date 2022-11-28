---
title: "[C++] 타입 변환(Type Casting)"
excerpt: "타입 변환에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, type casting, type, type conversion]

permalink: /cpp/type-casting/

toc: true
toc_sticky: true

date: 2022-11-28 19:48:52+0900
last_modified_at: 2022-11-28 19:48:54+0900
---
 
## 👻 타입 변환
**타입 변환(Type Casting)**은 **서로 다른 타입을 변환하 것**을 의미한다. ``` malloc ```의 반환값은 **타입 변환** 후 사용되어진다.

***

### 🌱 타입 변환 유형 (비트열 재구성 여부)

***

#### 🪐 값 타입 변환
의미를 유지하기 위해서 **원본 객체와 다른 비트열**로 재구성한다.

```c++
int a = 123456789;
float b = (float)a;
```

``` a ```는 **2의 보수**로 표현되지만 ``` b ```는 **부동소수점(지수 + 유효숫자)** 방식으로 표현된다. 원본값의 근사치로 변환하여 담는다.

***

#### 🪐 참조 타입 변환
비트열을 재구성하지 않고 **관점**만 바꾸는 것이다. 거의 쓸 일은 없지만, 포인터 타입 변환도 '참조 타입 변환'과 동일한 룰을 따른다.

```c++
int a = 123456789;
float b = (float&)a;
```

``` b ```엔 ``` a ```의 값이 그대로 들어가게 된다.

***

### 🌱 안전도 분류

***

#### 🪐 안전한 변환
의미가 항상 100% 완전히 일치하는 경우를 뜻한다. **같은 타입이면서 크기만 더 큰 바구니로 이동**하거나 **작은 바구니에서 큰 바구니로 이동(업캐스팅)**하는 것을 말한다.   

> ex) ``` char 👉 short ```, ``` short 👉 int ```, ``` int 👉 __int64 ``` 등

```c++
int a = 123456789;
__int64 b = a;
```

***

#### 🪐 불안전한 변환
의미가 항상 100% 일치한다고 보장하지 못하는 경우를 뜻한다. **타입이 다르거나 같은 타입이지만 큰 바구니에서 작은 바구니로 이동(다운캐스팅)**하는 것을 말한다.

```c++
int a = 123456789;
float b = a;
short c = a;
```

이러한 과정에서 데이터가 유실될 가능성이 크다.

***

### 🌱 프로그래머 의도에 따라 분류

***

#### 🪐 암시적 변환
이미 알려진 타입 변환 규칙에 따라서 컴파일러가 **자동**으로 변환해주는 것을 의미한다.

```c++
int a = 123456789;
float b = a;  // 암시적으로
```

***

#### 🪐 명시적 변환
직접적으로 변환할 타입을 지정해주는 것을 의미한다.

```c++
int a = 123456789;
int* b = (int*)a; // 명시적으로
```

***

### 🌱 아무런 연관 관계가 없는 클래스 사이의 변환

***

#### 🪐 값 타입 변환
일반적으로는 안 된다. 예외로 **타입 변환 생성자**나 **타입 변환 연산자**를 이용해 변환할 수 있다.

```c++
class Knight
{
public:
  int _hp = 10;
}

class Dog
{
public:
  Dog() {}

  // 타입 변환 생성자
  Dog(const Knight& knight)
  {
    _age = knight._hp;
  }

  // 타입 변환 연산자
  operator Knight()
  {
    Knight knight;
    knight._hp = _age + _cuteness;
    return knight;
  }
public:
  int _age = 1;
  int _cuteness = 2;
}

class BullDog : public Dog
{
public:
  bool _french; // 프렌치 불독
}

// 타입 변환 생성자가 있어서 가능. 없으면 변환이 불가능하다.
Knight knight;
Dog dog = (Dog)knight;

Knight knight2 = dog;
```

***

#### 🪐 참조 타입 변환
암시적으로는 불가능하나 명시적으로는 가능하다.

```c++
Knight knight;
Dog& dog = (Dog&)knight;
dog._cuteness = 12;
```

***

### 🌱 상속 관계에 있는 클래스 사이의 변환

***

#### 🪐 값 타입 변환
``` 자식 👉 부모 ```는 가능하지만 ``` 부모 👉 자식 ```은 불가능하다.

```c++
// 부모 👉 자식은 불가능
Dog dog;
BullDog bulldog = (BullDog)dog;

// 자식 👉 부모는 가능
BullDog bulldog;
Dog dog = bulldog;
```

***

#### 🪐 참조 타입 변환
``` 자식 👉 부모 ```는 가능하지만 ``` 부모 👉 자식 ```은 **암시적으로는 불가능**하나 **명시적으로는 가능**하다.

```c++
// 부모 👉 자식
Dog dog;
BullDog& bulldog = (BullDog&)dog; // 가능
// BullDog& bulldog = dog;  // 불가능

// 자식 👉 부모
BullDog bulldog;
Dog& dog = bulldog;
```

***

## 👻 결론
> - **값 타입 변환** : 비트열도 변경하고, 논리적으로 말이 되게 바꾸는 변환
  - 논리적으로 말이 된다? (ex. BullDog -> Dog) 👉 **OK**
  - 논리적으로 말이 안 된다? (ex. Dog -> BullDog, Dog -> Knight) 👉 **NO**
- **참조 타입 변환** : 비트열은 놔두고 우리의 관점만 바꾸는 변환
  - 명시적 요구를 하면 변환이 가능하다.
  - 암시적으로 그냥 해주는지는 **안전성 여부**와 관련 있다.
    - 안전하면 암시적으로도 **OK**
    - 메모리 침범 위험이 있는 경우는 암시적으로 해주진 않는다. 대신에 명시적으로 정말 변환하겠다고 최종 서명을 하면 가능하다.

***

## 👻 글을 마치며
이번 시간에는 타입 변환에 대해 알아보았다. 명시적, 암시적인 변환만 알고 있었는데 이렇게 각 상황마다 나눠서 고려해보니 변환을 언제 어디서 어떻게 사용해야할지 감이 잡히는 것 같다. 그래도 대부분 프로그래밍을 하면서 본능적으로 알고 있었던 부분이라 크게 어렵진 않았던 것 같다. 앞으로 더 조심하게 잘 사용할 수 있을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/heap/type-casting)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   