---
title: "[Programmers] Lv.1 - 자연수 뒤집어 배열로 만들기"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 12932, lv1]

permalink: /algorithm/programmers-12932/

toc: true
toc_sticky: true

date: 2023-01-21 19:09:45+0900
last_modified_at: 2023-01-21 19:09:49+0900
---
 
## 👻 자연수 뒤집어 배열로 만들기
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/12932?language=cpp)

***

### 🌱 문제
> 자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열 형태로 리턴해주세요. 예를들어 n이 12345이면 [5,4,3,2,1]을 리턴합니다.

***

### 🌱 제한사항
- n은 10,000,000,000이하인 자연수입니다.

***

### 🌱 입출력 예

|**n**|**return**|   
|12345|[5, 4, 3, 2, 1]|

***

## 👻 풀이

```c++
#include <string>
#include <vector>

using namespace std;

vector<int> solution(long long n) {
    vector<int> answer;
    while (n > 0)
    {
        answer.push_back(n % 10);
        n = n / 10;
    }
    return answer;
}
```

***

## 👻 글을 마치며
대다수의 사람과 동일한 풀이를 한 게 처음인 것 같다. 처음엔 문자열로 바꿔서 reverse 함수를 이용해 뒤집어볼까 고민했지만 그렇게 되면 또 여러 단계를 거쳐야 할 것 같았다. n의 수가 너무 커서 나눗셈을 하는데 무리가 갈까 처음엔 생각하지 않았지만 그래도 답은 이거라 했는데 제대로 잘 푼 것 같아서 기분이 좋고 능력이 조금 향상된 느낌이 든다! ~~비록 레벨1짜리 문제지만..ㅎㅎ~~

***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv1/12932.cpp)_