---
title: "[Programmers] Lv.1 - 정수 제곱근 판별"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 12934, lv1]

permalink: /algorithm/programmers-12934/

toc: true
toc_sticky: true

date: 2023-01-26 20:31:48+0900
last_modified_at: 2023-01-26 20:31:52+0900
---
 
## 👻 정수 제곱근 판별
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/12934?language=cpp)

***

### 🌱 문제
> 임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.
>
> n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, n이 양의 정수 x의 제곱이 아니라면 -1을 리턴하는 함수를 완성하세요.

***

### 🌱 제한사항
- n은 1이상, 50000000000000 이하인 양의 정수입니다.

***

### 🌱 입출력 예

|**n**|**return**|   
|121|144|   
|3|-1|

***

## 👻 풀이

```c++
#include <string>
#include <vector>
#include <cmath>

using namespace std;

long long solution(long long n) {
    long long answer = 0;
    double d = sqrt(n);
    if (d - int(d) == 0) 
        answer = (d + 1) * (d + 1);
    else
        answer = -1;
    return answer;
}
```

***

## 👻 글을 마치며
내장 함수를 모르면 코드가 조금 복잡해지는 문제같다. 항상 알고리즘을 공부하면서 드는 생각은 내장 함수를 언제 어디에 어떻게 쓸지 알려면 다 알아야한다는 것이다. 그래서 알고리즘을 공부하는 것이 기본 이론을 공부하는 것보다 훨씬 어렵지 않을까 하고 생각이 든다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv1/12934.cpp)_