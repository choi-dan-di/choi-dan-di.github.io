---
title: "[C++] 함수 기초"
excerpt: "함수의 개념에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, function, basic]

permalink: /cpp/function-basic/

toc: true
toc_sticky: true

date: 2022-11-14 15:35:13+0900
last_modified_at: 2022-11-14 15:35:15+0900
---

## 👻 함수란?
**함수**란 **비슷한 동작을 하는 코드의 묶음**을 나타낸다. 코드를 **재사용**하기 쉽게 하기위해 기능별로 묶어 사용하는 방식이다.

***

### 🌱 함수 생성 및 호출하기
> - input으로 무엇을 받고, output으로 무엇을 출력해줄지 정해줘야한다.
    - 둘 다 없는 경우도 존재한다.
- 반환타입이 없는 경우 ``` void ```를 입력한다.
    - 반환타입은 무조건 적어줘야하지만 인자에는 필수가 아니다.
- 함수 안에 ``` return ```을 입력하면 그 뒤에 있는 코드는 실행되지 않는다.
- 모든 함수에는 ``` return 0; ```이 생략되어있다. (main 함수도 포함)
- 매개변수는 함수 내에서만 사용된다.

```c++
/*
반환타입 함수이름([인자타입 매개변수])
{
    함수 내용

    return ~
}
*/

// Hello World를 콘솔에 출력하는 함수 만들어보기
// input : 없음 / output : 없음
void PrintHelloWorld()
{
    cout << "Hello World!" << endl;
}

int main()
{
    PrintHelloWorld();
}
```

입출력이 존재하는 경우도 문법에 맞춰서 써주면 된다.

***

## 👻 글을 마치며
이번 시간에는 함수 기초에 대해 알아보았다. 드디어 함수를 배움으로 인해 코딩이 편해질 것 같다. 😌 맨날 데이터만 갖고 놀다가 오랜만에 제대로 개발하는 느낌이라 그런지 재미있고 학구열이 타오르는 것 같다. 🔥🔥🔥

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/function/function-basic)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   