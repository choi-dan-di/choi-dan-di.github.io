---
title: "[Programmers] Lv.1 - 없는 숫자 더하기"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 86051, lv1]

permalink: /algorithm/programmers-86051/

toc: true
toc_sticky: true

date: 2023-01-21 19:09:45+0900
last_modified_at: 2023-01-21 19:09:49+0900
---
 
## 👻 없는 숫자 더하기
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/86051?language=cpp)

***

### 🌱 문제
> 0부터 9까지의 숫자 중 일부가 들어있는 정수 배열 ``` numbers ```가 매개변수로 주어집니다. ``` numbers ```에서 찾을 수 없는 0부터 9까지의 숫자를 모두 찾아 더한 수를 return 하도록 solution 함수를 완성해주세요.

***

### 🌱 제한사항
- 1 ≤ ``` numbers ```의 길이 ≤ 9
    - 0 ≤ ``` numbers ```의 모든 원소 ≤ 9
    - ``` numbers ```의 모든 원소는 서로 다릅니다.

***

### 🌱 입출력 예

|**numbers**|**result**|   
|[1, 2, 3, 4, 6, 7, 8, 0]|14|   
|[5, 8, 4, 0, 6, 7, 9]|6|

***

#### 🪐 입출력 예 설명
- **입출력 예 #1**   
5, 9가 ``` numbers ```에 없으므로, 5 + 9 = 14를 return 해야 합니다.

- **입출력 예 #2**   
1, 2, 3이 ``` numbers ```에 없으므로, 1 + 2 + 3 = 6을 return 해야 합니다.

***

## 👻 풀이

```c++
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> numbers) {
    int answer = 0;
    int arr[10] = {};
    for (int i = 0; i < numbers.size(); i++)
        arr[numbers[i]]++;
    for (int i = 0; i < 10; i++)
        if (arr[i] == 0) answer += i;
    
    return answer;
}
```

***

## 👻 글을 마치며
나름 코드를 줄인다고 줄인 건데 다른 사람들의 답변을 보니 전체 총합에서 배열을 돌며 값들을 빼는 방식을 사용한 코드를 많이 볼 수 있었다. 나 같은 경우는 없는 수를 찾는 단계와 더하는 단계, 총 두 단계로 나뉘는데 빼는 방식을 사용하면 한 단계만 거쳐도 답을 구할 수 있다. 알고리즘을 학습하며 내 머릿속의 알고리즘을 진짜 바꿀 수 있을지 걱정이 되기 시작했다. 😭 그래도 일단 꾸준히 해야겠지..

***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv1/86051.cpp)_