---
title: "[Programmers] Lv.1 - 가장 가까운 같은 글자"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 142086, lv1]

permalink: /algorithm/programmers-142086/

toc: true
toc_sticky: true

date: 2022-12-11 18:00:27+0900
last_modified_at: 2022-12-11 18:00:31+0900
---
 
## 👻 가장 가까운 같은 글자
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/142086?language=cpp)

***

### 🌱 문제
> 문자열 s가 주어졌을 때, s의 각 위치마다 자신보다 앞에 나왔으면서, 자신과 가장 가까운 곳에 있는 같은 글자가 어디 있는지 알고 싶습니다.
>
> 예를 들어, s = "banana"라고 할 때, 각 글자들을 왼쪽부터 오른쪽으로 읽어 나가면서 다음과 같이 진행할 수 있습니다.
>
> - b는 처음 나왔기 때문에 자신의 앞에 같은 글자가 없습니다. 이는 -1로 표현합니다.
- a는 처음 나왔기 때문에 자신의 앞에 같은 글자가 없습니다. 이는 -1로 표현합니다.
- n는 처음 나왔기 때문에 자신의 앞에 같은 글자가 없습니다. 이는 -1로 표현합니다.
- a는 자신보다 두 칸 앞에 a가 있습니다. 이는 2로 표현합니다.
- n도 자신보다 두 칸 앞에 n이 있습니다. 이는 2로 표현합니다.
- a는 자신보다 두 칸, 네 칸 앞에 a가 있습니다. 이 중 가까운 것은 두 칸 앞이고, 이는 2로 표현합니다.
>
> 따라서 최종 결과물은 [-1, -1, -1, 2, 2, 2]가 됩니다.
>
> 문자열 s가 주어질 때, 위와 같이 정의된 연산을 수행하는 함수 solution을 완성해주세요.

***

### 🌱 제한사항
- 1 ≤ s의 길이 ≤ 10,000
    - s는 영어 소문자로만 이루어져 있습니다.

***

### 🌱 입출력 예

|**s**|**result**|   
|"banana"|[-1, -1, -1, 2, 2, 2]|   
|"foobar"|[-1, -1, 1, -1, -1, -1]|   

***

#### 🪐 입출력 예 설명
- **입출력 예 #1**   
지문과 같습니다.

- **입출력 예 #2**   
설명 생략

***

## 👻 풀이

```c++
#include <string>
#include <vector>

using namespace std;

vector<int> solution(string s) {
    vector<int> answer;
    
    for (int i = 0; i < s.size(); i++)
    {
        if (i > 0)
        {
            int cnt = 0;
            bool isExist = false;
            for (int j = i - 1; j >= 0; j--)
            {
                cnt++;
                if (s[i] == s[j])
                {
                    isExist = true;
                    break;
                }
                else
                    continue;
            }
            
            if (isExist) answer.push_back(cnt);
            else answer.push_back(-1);
        }
        else
            answer.push_back(-1);
    }
    return answer;
}
```

***

## 👻 글을 마치며
이번 문제도 푸는 데에는 그리 오랜 시간이 걸리진 않았다. 하지만 나는 값을 주로 새 변수에 저장하는 습관이 있는 것 같다. 다른 풀이를 보니 이미 나온 변수들을 충분히 재활용하여 나타냈는데 이러한 스킬이 좀 부족한 것 같다. 알고리즘 설계하는 것도 중요하지만 이미 가지고 있는 변수를 활용하는 방법에 대해서도 모색해봐야할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv1/142086.cpp)_