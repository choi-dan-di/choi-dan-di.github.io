---
title: "[Programmers] Lv.1 - 음양 더하기"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 76501, lv1]

permalink: /algorithm/programmers-76501/

toc: true
toc_sticky: true

date: 2023-01-04 18:08:14+0900
last_modified_at: 2023-01-04 18:08:18+0900
---
 
## 👻 음양 더하기
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/76501?language=cpp)

***

### 🌱 문제
> 어떤 정수들이 있습니다. 이 정수들의 절댓값을 차례대로 담은 정수 배열 absolutes와 이 정수들의 부호를 차례대로 담은 불리언 배열 signs가 매개변수로 주어집니다. 실제 정수들의 합을 구하여 return 하도록 solution 함수를 완성해주세요.

***

### 🌱 제한사항
- absolutes의 길이는 1 이상 1,000 이하입니다.
    - absolutes의 모든 수는 각각 1 이상 1,000 이하입니다.
    
- signs의 길이는 absolutes의 길이와 같습니다.
    - ``` signs[i] ```가 참이면 ``` absolutes[i] ```의 실제 정수가 양수임을, 그렇지 않으면 음수임을 의미합니다.

***

### 🌱 입출력 예

|**absolutes**|**signs**|**result**|   
|[4, 7, 12]|[true, false, true]|9|   
|[1, 2, 3]|[false, false, true]|0|   


***

#### 🪐 입출력 예 설명
- **입출력 예 #1**   
signs가 ``` [true, false, true] ```이므로, 실제 수들의 값은 각각 4, -7, 12입니다.   
따라서 세 수의 합인 9를 return 해야 합니다.

- **입출력 예 #2**   
signs가 ``` [false, false, true] ```이므로, 실제 수들의 값은 각각 -1, -2, 3입니다.   
따라서 세 수의 합인 0을 return 해야 합니다.

***

## 👻 풀이

```c++
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> absolutes, vector<bool> signs) {
    int answer = 0;
    for (size_t i = 0; i < absolutes.size(); i++)
    {
        if (signs[i] == false) absolutes[i] *= -1;
        answer += absolutes[i];
    }
    return answer;
}
```

***

## 👻 글을 마치며
어렵지 않게 풀 수 있었다. 예전에는 레벨 1 짜리도 푸는 데 많은 시간이 걸렸는데 요즘 매일 쉬운 문제라도 한 문제씩 풀다보니 알고리즘 실력이 조금 는 것 같은 느낌이 많이 든다. 앞으로도 꾸준히 연습해서 점진적으로 스킬의 레벨을 올려야겠다. ☺

***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv1/76501.cpp)_