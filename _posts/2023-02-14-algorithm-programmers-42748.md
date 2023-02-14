---
title: "[Programmers] Lv.1 - K번째수"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 42748, lv1]

permalink: /algorithm/programmers-42748/

toc: true
toc_sticky: true

date: 2023-02-14 18:25:00+0900
last_modified_at: 2023-02-14 18:25:03+0900
---
 
## 👻 K번째수
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/42748?language=cpp)

***

### 🌱 문제
> 배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.
>
> 예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면   
1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
3. 2에서 나온 배열의 3번째 숫자는 5입니다.
> 배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

***

### 🌱 제한사항
- array의 길이는 1 이상 100 이하입니다.
- array의 각 원소는 1 이상 100 이하입니다.
- commands의 길이는 1 이상 50 이하입니다.
- commands의 각 원소는 길이가 3입니다.

***

### 🌱 입출력 예

|**array**|**commands**|**return**|   
|[1, 5, 2, 6, 3, 7, 4]|[[2, 5, 3], [4, 4, 1], [1, 7, 3]]|[5, 6, 3]|

***

### 🌱 입출력 예 설명
[1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.   
[1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.
[1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.

[출처](https://neerc.ifmo.ru/subregions/northern.html)

***

## 👻 풀이

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> solution(vector<int> array, vector<vector<int>> commands) {
    vector<int> answer;
    
    for (int i = 0; i < commands.size(); i++)
    {
        vector<int> v;
        int start = commands[i][0] - 1;
        int end = commands[i][1] - 1;
        for (int j = 0; j < array.size(); j++)
        {
            if (start <= end)
            {
                v.push_back(array[start]);
                start++;
            }
        }
        sort(v.begin(), v.end());
        answer.push_back(v[commands[i][2] - 1]);
    }
    
    return answer;
}
```

***

## 👻 글을 마치며
가장 기초적으로 생각하여 이중 for문을 작성하였는데 다른 사람의 풀이에서 구간을 정하는 것과 정렬하는 것을 한 번에 처리한 코드가 인상깊었다. 코딩 머리는 존재하는가...🤔

***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv1/42748.cpp)_