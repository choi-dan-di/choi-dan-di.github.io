---
title: "[C++] Enum Class(Scoped Enum)"
excerpt: "Enum Class 문법에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, modern cpp, modern, enum class]

permalink: /cpp/enum-class/

toc: true
toc_sticky: true

date: 2022-12-18 17:01:17+0900
last_modified_at: 2022-12-18 17:01:21+0900
---

## 👻 Enum Class
일반 열거형인 ``` enum ```에 클래스가 붙은 열거형이다. 일반 열거형은 **Unscoped Enum, 범위가 없**는 열거형이며 지정한 이름을 다른 열거형에서 중복으로 사용할 수 없다. ``` enum class ```는 이름의 공간이 한정적이어서 중복 사용이 가능하다.

```c++
enum PlayerType {
    // None,    // 중복 사용 불가
    PT_Knight,
    PT_Archer, 
    PT_Mage
};

enum MonsterType {
    // None,    // 중복 사용 불가
};

// enum class
enum class ObjectType : int {
    Player,
    Monster,
    Projectile
};

// 이름 중복 사용 가능
enum class ObjectType2 {
    Player,
    Monster,
    Projectile
};
```

대신에 사용할 때 클래스 명을 반드시 붙여줘야한다.

```c++
ObjectType::Player;
```

또한 ``` enum class ```는 **암묵적인 변환이 금지**된다. 일반 열거형은 타입을 따로 지정해주지 않으면 자동으로 ``` int ``` 타입으로 세팅되지만 ``` enum class ```는 사용할 때 타입 변환을 반드시 해줘야한다. 경우에 따라 장점이 될 수도 있지만 단점이 될 수도 있다.

```c++
double value = PT_Knight; // 가능
double value2 = ObjectType::Player; // 불가능
double value2 = static_cast<double>(ObjectType::Player);
```

이러한 점은 코드가 길어진다는 단점을 가진다.

```c++
if (choice == static_cast<int>(ObjectType2::Monster)) {
    cout << "yyy" << endl;
}

unsigned int bitFlag;
bitFlag = (1 << static_cast<int>(ObjectType::Player));
```

***

## 👻 글을 마치며
이번 시간에는 enum class에 대해 알아보았다. 열거형은 하나밖에 없는 줄 알았는데 클래스가 합쳐진 버전도 있다는 걸 알고 놀랐다. ~~안 그래도 외울 거 많은데 뭐가 계속 늘어나..~~ 잘 사용하면 좋은 것 같은데 일반 열거형보다 크~게 좋다는 느낌은 받지 못했다. 코드 길어지는 게 제일 싫기 때문에..😅

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/modern-cpp/enum-class)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   