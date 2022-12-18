---
title: "[C++] 오른값 참조(rvalue reference)와 std::move"
excerpt: "오른값 참조와 std::move에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, modern cpp, modern, rvalue reference, lvalue, move]

permalink: /cpp/rvalue-reference/

toc: true
toc_sticky: true

date: 2022-12-18 19:39:16+0900
last_modified_at: 2022-12-18 19:39:18+0900
---

## 👻 오른값 참조
**오른값 참조(RValue Reference)**는 말 그대로 오른쪽의 값을 참조한다는 의미이다. **대입 연산자**를 기준으로 왼쪽은 **왼값(lvalue)**, 오른쪽은 **오른값(rvalue)**을 가지게 된다.

> - **lvalue** : 단일식을 넘어서 계속 지속되는 개체
- **rvalue** : lvalue가 아닌 나머지 (임시 값, 열거형, 람다, i++ 등)

참조를 의미하는 앰퍼센트 두 개를 연달아 붙여서 사용한다.

```c++
void TestKnight_RValueRef(Knight&& knight) { }

int main() {
    Knight k1;
    // 오른값에 해당하는 임시 객체 전달
    TestKnight_RValueRef(Knight());
    // 타입 변환하여 전달 가능
    TestKnight_RValueRef(static_cast<Knight&&>(k1));
}
```

오른값의 참조값을 전달하게 되면 **원본 자체를 건드릴 수 있게**된다. 그 의미는 곧 더 이상 해당 원본은 사용하지 않을 예정이며 날려도 된다는 의미를 내포하고 있다. 쉽게 말해 **이동 대상**이 된다는 의미이다.

***

### 🌱 이동 생성자와 이동 대입 연산자
깊은 복사를 하기 위해선 우리가 직접 복사 생성자와 복사 대입 연산자를 구현해줘야 한다고 했었다. 마찬가지로 오른값 참조 시에 원하는 동작을 만들고 싶으면 **이동 생성자**와 **이동 대입 연산자**를 직접 구현해주면 된다.

```c++
class Knight {
public:
    Knight() {
        cout << "Knight()" << endl;
    }

    ~Knight() {
        if (_pet)
            delete _pet;
    }

    // 이동 생성자
    Knight(Knight&& knight) {
        cout << "Knight&&" << endl;
    }

    // 이동 대입 연산자
    void operator=(Knight&& knight) noexcept {
        cout << "operator=(copnst Knight&&)" << endl;

        // 얕은 복사
        _hp = knight._hp;
        _pet = knight._pet;

        // 오른값의 정보를 수정할 수 있다.
        knight._pet = nullptr;
    }

public:
    int _hp = 100;
    Pet* _pet = nullptr;
};
```

얕은 복사가 되더라도 정보를 참고하는 오른값의 데이터를 건드릴 수 있다.

***

### 🌱 std::move
``` move ``` 함수는 **오른값 참조 캐스팅**을 조금 더 편리하게 실행시켜주는 함수이다. ``` rvalue_cast ``` 의 의미에 가깝다.

```c++
Knight k3;
Knight k4;
k4 = move(k3);  // 오른값 참조로 캐스팅
```

``` k3 ```를 오른값 참조로 캐스팅한 후에 ``` k4 ```로 이동시킨다음 ``` k3 ```의 펫 소유권을 박탈시키는 기능을 하게 될 것이다. (이동 대입 연산자 코드 참고)

> 💡 **참고**   
``` unique_ptr ```은 오로지 하나만 존재하는 포인터를 의미한다. 고유한 값이기 때문에 복사가 불가능하다. 하지만 ``` move ```를 이용하여 소유권을 계속 이전시키면 여러번 이동하며 해당 값을 유지할 수 있다.
>
> ```c++
> // 복사 불가
> unique_ptr<Knight> uptr = make_unique<Knight>();
> // 소유권 이전의 개념. 이동 후 uptr은 소멸된다.
> unique_ptr<Knight> uptr2 = move(uptr);
> ```

***

## 👻 글을 마치며
이번 시간에는 오른값 참조에 대해 알아보았다. 처음 보는 문법이어서 조금 어렵긴 했는데 직접 코드도 계속 수정해보고 개념 자체를 이해하려고 노력하다보니 의미는 알 수 있게 되었다. 그런데 아직까지는 예시가 좀 부족해서 그런지 익숙하지 않은데 많이 찾아봐야 할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/modern-cpp/rvalue-reference)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   