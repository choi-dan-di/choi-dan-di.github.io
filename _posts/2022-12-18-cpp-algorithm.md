---
title: "[C++] 알고리즘(Algorithm)"
excerpt: "알고리즘 라이브러리에 있는 대표적인 함수들 살펴보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, stl, algorithm, find, find_if, count, count_if, all_of, any_of, none_of, for_each, remove, remove_if]

permalink: /cpp/algorithm/

toc: true
toc_sticky: true

date: 2022-12-18 02:19:00+0900
last_modified_at: 2022-12-18 02:19:02+0900
---

## 👻 알고리즘
앞서 배웠던 STL 문법들을 조금 더 효율적으로 사용할 수 있게 만들어주는 게 바로 **알고리즘(Algorithm)**이다. 알고리즘은 자료구조를 활용해 저장한 데이터에 접근하여 어떻게 사용할 것인지에 대한 방법들을 정리해 놓은 것이다. C++에서 ``` algorithm ``` 라이브러리를 통해 사용할 수 있다. 자주 사용되는 문법들을 몇 가지 알아보자.

[👉 자세한 차이는 여기에 👈](/algorithm/algorithm-vs-data-structure)

***

### 🌱 find, find_if
조회할 때 사용하는 함수이다. ``` find ```는 **일치하는 값**을, ``` find_if ```는 **조건에 일치하는 값**을 찾을 때 사용한다. 일치하는 첫 데이터의 이터레이터를 반환한다.

```c++
// find
vector<int>::iterator itFind = find(v.begin(), v.end(), number);

// 함수 객체 생성 -> 추후엔 람다식으로 간편하게 만들 수 있음.
// 람다식 : [](int n) { return n % 11 == 0; }
struct CanDivideBy11 {
    bool operator()(int n) {
        return n % 11 == 0;
    }
};

// find_if
vector<int>::iterator itFind = find_if(v.begin(), v.end(), CanDivideBy11());
```

***

### 🌱 count, count_if
카운트 해주는 함수이다. ``` count ```는 **일치하는 값**, ``` count_if ```는 **조건에 일치하는 값**이면 1씩 증가시키고 총 누적 카운트를 반환한다.

```c++
// count
int count = count(v.begin(), v.end(), 4);

// count_if
struct IsOdd {
    bool operator()(int n) {
        return n % 2 != 0;
    }
};

int count = count_if(v.begin(), v.end(), IsOdd());
```

***

### 🌱 all_of, any_of, none_of
모두 불리언 값을 반환하고 각각 **모든 데이터가 조건을 만족하는가?, 조건을 만족하는 데이터가 하나라도 존재하는가?, 조건을 만족하는 데이터가 하나도 없는가?**를 의미한다.

```c++
// all_of : 모든 데이터가 조건을 만족하는가?
// any_of : 조건을 만족하는 데이터가 하나라도 존재하는가?
// none_of : 조건을 만족하는 데이터가 하나도 없는가?

bool b1 = all_of(v.begin(), v.end(), IsOdd());
bool b2 = any_of(v.begin(), v.end(), IsOdd());
bool b3 = none_of(v.begin(), v.end(), IsOdd());
```

***

### 🌱 for_each
모든 데이터를 스캔할 때 사용한다. 모든 데이터를 순회하며 건드리거나 출력할 수 있다.

```c++
struct MultiplyBy3 {
    void operator()(int& n) {
        n *= 3;
        // 출력까지 하고 싶으면
        cout << n << endl;
    }
};

for_each(v.begin(), v.end(), MultiplyBy3());
```

***

### 🌱 remove, remove_if
``` remove ```와 ``` remove_if ```는 사용 시에 반드시 ``` erase ```와 같이 사용해야한다. 각각 **일치하는 값**, **조건에 일치하는 값**을 삭제시켜주는데 그 즉시 삭제하게 되므로 뒷부분에 중복이 발생할 수 있다. 그 부분을 ``` erase ```로 지워줘야한다.

```c++
// remove
v.erase(remove(v.begin(), v.end(), 4), v.end());

struct IsOdd {
    bool operator()(int n) {
        return n % 2 != 0;
    }
};

// remove_if
// vector<int>::iterator it = remove_if(v.begin(), v.end(), IsOdd());
// v.erase(it, v.end());
v.erase(remove_if(v.begin(), v.end(), IsOdd()), v.end());
```

***

> 이 외에도 많은 알고리즘 문법이 있으니 필요한 기능이 있을 때마다 구글링하여 참고하자.

***

## 👻 글을 마치며
이번 시간에는 자주 쓰이는 알고리즘 문법에 대해 알아보았다. 코딩 테스트를 급하게 준비했을 때 제일 많이 봤던 함수가 find 였는데, 라이브러리마다 있을 정도로 흔한 함수여서 사용하는 데 애를 좀 먹었던 기억이 있다. 오늘 알고리즘 라이브러리의 find 함수에 대해 알게 되었으니 이 부분 만큼은 확실하게 안 찾아보고 잘 사용할 수 있을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/STL/algorithm)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   