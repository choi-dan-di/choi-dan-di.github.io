---
title: "[C++] 전달 참조(Forwarding Reference)"
excerpt: "전달 참조에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, modern cpp, modern, forwarding reference, reference, forwarding]

permalink: /cpp/forwarding-reference/

toc: true
toc_sticky: true

date: 2022-12-18 20:31:31+0900
last_modified_at: 2022-12-18 20:31:32+0900
---

## 👻 전달 참조
**전달 참조(Forwarding Reference)**는 **C++17**이전엔 **보편 참조(Universal Reference)**로도 불렸으며 오른값 참조와 비슷하게 생겼지만 **형식 연역(Type Deduction)**이 일어날 때 일어날 수 있는 값 전달 방식이다. 형식 연역은 추론하여 타입을 정해주는 뜻이고, 이전에 앞서 배운 ``` auto ```나 ``` template ``` 문법이 이에 해당된다.

오른값 참조와 똑같이 앰퍼센트 두 개를 연달아 붙여 사용하지만 상황에 따라 왼값 참조, 오른값 참조 방식으로 나뉘게 된다.

```c++
template<typename T>
void Test_ForwardingRef(T&& param) {
    // TODO
}

int main() {
    Knight k1;

    // 왼값 참조
    Test_ForwardingRef(k1);
    // 오른값 참조
    Test_ForwardingRef(move(k1));

    // 왼값 참조
    auto&& k2 = k1;
    // 오른값 참조
    auto&& k3 = move(k1);
}
```

전달 받을 때 받는 타입에 따라 왼값 참조인지 오른값 참조인지 추론해서 타입이 정해지게 되며 이러한 특징이 오른값 참조와 전달 참조의 차이를 나타낸다. 참고로 **오른값**과 **오른값 참조**는 다른 의미이다.

> - **오른값** : 왼값이 아니다 = 단일식에서 벗어나면 사용 불가능한 것들
- **오른값 참조** : 오른값만 참조할 수 있는 참조 타입
>
> ```c++
> Knight& k4 = k1;    // 왼값 참조
> Knight&& k5 = move(k1); // 오른값 참조
>
> Test_ForwardingRef(k5);
> ```
>
> 위의 코드에서 매개변수로 건넨 ``` k5 ```가 오른값 참조 타입을 가진다고 해서 오른값 참조가 되는 것은 아니다. **타입이 오른값 참조**일 뿐이고 오른값이 아닌 **왼값**이다. 그래서 결과는 왼값 참조로 들어가게된다. 오른값 참조로 값을 전달하고 싶다면 ``` move ``` 를 사용하여 캐스팅 해주어야한다.

***

### 🌱 std::forward
위에서 만들었던 함수같은 경우 왼값 참조 타입의 값을 받을 때도 있고 오른값 참조 타입의 값을 받을 때도 있다. ``` forward ```를 사용하게 되면 해당 값의 타입을 구분해줄 수 있다.

```c++
template<typename T>
void Test_ForwardingRef(T&& param) {
    // TODO
    // 만약 넘겨받은 param이 왼값 참조라면 복사 형태로, 오른값 참조라면 이동 형태로 작동되어야한다.
    // std::forward를 이용하면 알아서 구분해준다.
    Test_Copy(forward<T>(param));
}
```

***

## 👻 글을 마치며
이번 시간에는 전달 참조에 대해 알아보았다. 점점 개념이 어려워지고 복잡해지는 것 같다. 😂 오른값 참조 까지는 어찌저찌 이해했는데.. 전달 참조를 많이 사용하게 될지 궁금해졌다. 얼른 프로젝트를 진행해야겠다고 생각이 드는 요즘이다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/modern-cpp/forwarding-reference)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   