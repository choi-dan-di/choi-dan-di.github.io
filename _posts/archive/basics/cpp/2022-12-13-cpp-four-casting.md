---
title: "[C++] 캐스팅(static, dynamic, const, reinterpret)"
excerpt: "C++ 스타일의 타입 변환(캐스팅) 문법 4가지를 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, type casting, casting, four casting, static_cast, dynamic_cast, const_cast, reinterpret_cast]

permalink: /cpp/four-casting/

toc: true
toc_sticky: true

date: 2022-12-13 18:25:21+0900
last_modified_at: 2022-12-13 18:25:24+0900
---
 
## 👻 캐스팅 4총사
괄호를 이용하여 타입을 변환하는 방법은 C에서부터 존재해 온 문법이다. C++ 스타일의 ``` static_cast ```, ``` dynamic_cast ```, ``` const_cast ```, ``` reinterpret_cast ```에 대해 알아보자.

***

### 🌱 static_cast
타입 원칙에 비춰볼 때 상식적인 캐스팅만 허용해준다.

- ``` int ``` ↔ ``` float ```

```c++
int hp = 100;
int maxHp = 200;
float ratio = static_cast<float>(hp) / maxHp;
```

- 부모 👉 자식 (다운캐스팅)
    - 단, 안전성은 보장 못한다.

```c++
Player* p = new Knight();
Knight* k1 = static_cast<Knight*>(p);
```

***

### 🌱 dynamic_cast
상속 관계에서의 안전한 타입 변환을 지원해준다. **RTTI(RunTime Type Information)**, **다형성**을 활용하는 방식이다. ``` virtual ``` 함수를 하나라도 만들면, 객체의 메모리에 **가상 함수 테이블(vftable) 주소**가 기입된다. 만약 잘못된 타입으로 캐스팅을 했으면 **nullptr**을 반환한다. 이를 이용하면 맞는 타입으로 캐스팅을 했는지 확인할 때 유용하다.

```c++
Knight* k2 = dynamic_cast<Knight*>(p);
```

***

### 🌱 const_cast
``` const ```를 붙이거나 뗄 때 사용한다.

```c++
void PrintName(char* str)
{
    cout << str << endl;
}

int main()
{
    PrintName(const_cast<char*>("DanDi"));
}
```

***

### 🌱 reinterpret_cast
**re-interpret**은 **다시-간주하다, 다시-생각하다**라는 의미이며, 가장 위험하고 강력한 형태의 캐스팅이다. 상관 없는 클래스 간의 타입 변환을 허용한다.

- 포인터와 전혀 관계없는 다른 타입으로 변환할 때

```c++
__int64 address = reinterpret_cast<__int64>(k2);

Dog* dog1 = reinterpret_cast<Dog*>(k2);

void* p = malloc(1000);
Dog* dog2 = reinterpret_cast<Dog*>(p);
```

***

## 👻 글을 마치며
이번 시간에는 C++ 스타일의 4가지의 캐스팅 문법에 대해 알아보았다. C 스타일의 문법(괄호 사용)은 방대하게 사용할 수 있지만 그만큼 실수 발생률도 높아진다. 이러한 실수를 조금이라도 방지하기 위해 C++ 스타일의 캐스팅 문법을 사용하면 좋을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/heap/four-casting)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   