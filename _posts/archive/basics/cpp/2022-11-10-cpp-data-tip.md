---
title: "[C++] 데이터 마무리"
excerpt: "데이터를 만질 때 유의해야 할 사항들 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, data]

permalink: /cpp/data-tip/

toc: true
toc_sticky: true

date: 2022-11-10 22:56:57+0900
last_modified_at: 2022-11-10 22:56:59+0900
---

## 👻 변수의 유효범위
- 함수 외부에서 선언하는 변수를 **전역 변수**라고 한다. 어디에서나 사용가능하다.
- 중괄호 ``` {} ``` 내에 선언 되는 변수는 **스택**에 올라간다. 중괄호 내에서 선언되는 변수는 중괄호 내에서만 유효하다. 같은 이름을 두번 사용할 때 문제가된다.
```c++
int main()
{
    int hp = 10;
    cout << hp << endl;

    int hp = 100;   // 재정의 에러 뜲
}
```

***

## 👻 연산 우선순위
![Alt Text](/assets/images/posts_img/basics/cpp/data-tip/priority.png)   
다 외우긴 힘드니 애매하면 괄호 ``` () ```로 묶어주는 게 가독성 높이기에 좋다.

***

## 👻 타입 변환
변수 타입을 변경하는 것. 작은 범위에서 큰 범위로 옮길 땐 괜찮지만 큰 범위에서 작은 범위로 옮길 땐 주의해야한다.

```c++
int hp = 77777;

// 타입 변환
short hp2 = hp;     // 상위 비트 데이터가 잘린 상태로 저장
float hp3 = hp;     // 실수로 변환할 때 정밀도 차이가 있기 때문에 데이터 손실이 있을 수 있다.
unsigned int hp4 = hp;  // 비트 단위로 보면 똑같은데 분석하는 방법이 달라서 이상한 값이 들어간다.

// 위의 코드는 사실 아래처럼 되어있다
short hp2 = (short)hp;
float hp3 = (float)hp;
unsigned int hp4 = (unsigned int)hp;
```

타입 변환은 위험한 작업이므로 신중하게 사용하자.

***

## 👻 사칙 연산 관련
- 곱셈의 경우 **오버플로우(overflow)**를 조심해야한다.
- 나눗셈의 경우 **0으로 나누지는 않는지** 조심해야한다.
```c++
int hp = 123;
int maxHp = 0;
float ratio = hp / maxHp;   // error
```
- 또, 정수끼리 나눈 후 실수 타입에 담게되면 소수점 뒷자리가 잘릴 수 있다. 👉 둘 중에 하나는 실수 타입으로 변환 후 나눠야한다.
```c++
int hp = 123;
int maxHp = 1000;
float ratio = hp / maxHp;   // 0
float ratio2 = hp / (float)maxHp;   // 0.123
```

***

## 👻 글을 마치며
사소하지만 중요하고 소소한 팁들을 정리하는 시간이었다. 이미 뭐 다 알고있는 것들이라서 복습하는 느낌으로 포스팅한 것 같다. 그치만 이렇게 해놓고도 같은 실수를 반복하는 날이 오겠지.. 해결하는 시간이 오래 걸리질 않길 바랄뿐이다. 😎

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   