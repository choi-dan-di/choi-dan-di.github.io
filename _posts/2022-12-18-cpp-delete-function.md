---
title: "[C++] 함수의 삭제 선언(Delete Functions)"
excerpt: "함수를 사용하지 못하도록 막아주는 delete에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, modern cpp, modern, delete function, delete, function, friend]

permalink: /cpp/delete-function/

toc: true
toc_sticky: true

date: 2022-12-18 17:52:11+0900
last_modified_at: 2022-12-18 17:52:13+0900
---

## 👻 함수의 삭제 선언
C++의 컴파일러가 자동으로 만들어주는 함수 중 사용하지 않아야 할 함수를 삭제시키는 구문에 대해 알아보자. 클래스의 멤버 함수의 경우, 원래라면 ``` private ```으로 설정하여 접근을 막으면 됐지만 Modern C++로 넘어오면서 ``` delete ```라는 문법이 하나 생기게 되었다. 해당 문법을 사용하면 한 줄로 간편하게 함수 호출을 막을 수 있다.

```c++
class Knight {
public:
    // 내가 명시적으로 사용을 안 하겠다고 선언한 의미이다.
    void operator=(const Knight& k) = delete;
private:
    int _hp = 100;
};
```

여기서 눈여겨 볼 것은 ``` = delete ``` 부분이다. 함수명 뒤에 붙여줌으로써 간단하게 해당 함수의 호출을 막을 수 있다.

***

## 👻 friend
``` private ```으로 설정된 클래스 내부의 값들은 그 내부에서밖에 접근하지 못한다. 하지만 ``` friend ```를 사용하면 해당 클래스나 함수 **한정으로 접근이 가능**하게 만들어준다. 하지만 **정보 은닉**에 위배되므로 웬만하면 사용을 지양하는 게 좋다.

```c++
class Knight {
public:

// 예전 문법
private:
    // 정의되지 않은 비공개(private) 함수
    void operator=(const Knight& k) {

    }
    // 이 아이 한정 허용
    friend class Admin;
    friend void Print(const Knight& k);
private:
    int _hp = 100;
};

class Admin {
public:
    void CopyKnight(const Knight& k) {
        Knight k1;
        // 복사 연산
        k1 = k;
    }
};

void Print(const Knight& k) {
    cout << k._hp << endl;
}

int main() {
    Admin admin;
    admin.CopyKnight(k1);

    Print(k1);
}
```

``` friend ``` 문법은 클래스, 전역 함수, 멤버 함수에 한해 설정이 가능하다.

***

## 👻 글을 마치며
이번 시간에는 함수의 삭제 선언에 대해 알아보았다. 오늘은 클래스의 멤버 함수에 한해서만 다뤘지만 구글링하여 좀 더 찾아보니 전역 함수도 삭제가 가능한 것 같다. 자주 쓰이진 않겠지만 알아두면 유용하게 사용할 수 있을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/modern-cpp/delete-function)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   