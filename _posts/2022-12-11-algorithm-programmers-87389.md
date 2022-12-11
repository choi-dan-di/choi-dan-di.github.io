---
title: "[Programmers] Lv.1 - 나머지가 1이 되는 수 찾기"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 87389, lv1]

permalink: /algorithm/programmers-87389/

toc: true
toc_sticky: true

date: 2022-12-11 17:49:07+0900
last_modified_at: 2022-12-11 17:49:11+0900
---
 
## 👻 나머지가 1이 되는 수 찾기
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/87389?language=cpp)

***

### 🌱 문제
> 자연수 n이 매개변수로 주어집니다. n을 x로 나눈 나머지가 1이 되도록 하는 가장 작은 자연수 x를 return 하도록 solution 함수를 완성해주세요. 답이 항상 존재함은 증명될 수 있습니다.

***

### 🌱 제한사항
- 3 ≤ n ≤ 1,000,000

***

### 🌱 입출력 예

|**n**|**result**|   
|10|3|   
|12|11|   

***

#### 🪐 입출력 예 설명
- **입출력 예 #1**   
10을 3으로 나눈 나머지가 1이고, 3보다 작은 자연수 중에서 문제의 조건을 만족하는 수가 없으므로, 3을 return 해야 합니다.

- **입출력 예 #2**   
12를 11로 나눈 나머지가 1이고, 11보다 작은 자연수 중에서 문제의 조건을 만족하는 수가 없으므로, 11을 return 해야 합니다.

***

## 👻 풀이

```c++
#include <string>
#include <vector>

using namespace std;

int solution(int n) {
    int answer = 0;
    for (int i = 1; i < n; i++)
    {
        if (n % i == 1)
        {
            answer = i;
            break;
        }
    }
    return answer;
}
```

***

## 👻 글을 마치며
수와 관련된 레벨 1 문제는 굉장히 쉽게 푼 것 같다. 레벨 1 내에서도 난이도가 좀 나뉘는 것 같다. 근데 프로그래머스는 난이도로만 분류되어있어서 분야 별 약한 부분을 보완하려면 그 전에 문제 분류부터 우선이 되어야 할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv1/87389.cpp)_