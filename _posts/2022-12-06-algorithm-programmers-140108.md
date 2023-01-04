---
title: "[Programmers] Lv.1 - 문자열 나누기"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 140108, lv1, string]

permalink: /algorithm/programmers-140108/

toc: true
toc_sticky: true

date: 2022-12-06 00:37:07+0900
last_modified_at: 2022-12-06 00:37:12+0900
---
 
## 👻 문자열 나누기
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/140108?language=cpp)

***

### 🌱 문제
> 문자열 s가 입력되었을 때 다음 규칙을 따라서 이 문자열을 여러 문자열로 분해하려고 합니다.
>
> - 먼저 첫 글자를 읽습니다. 이 글자를 x라고 합시다.
- 이제 이 문자열을 왼쪽에서 오른쪽으로 읽어나가면서, x와 x가 아닌 다른 글자들이 나온 횟수를 각각 셉니다. 처음으로 두 횟수가 같아지는 순간 멈추고, 지금까지 읽은 문자열을 분리합니다.
- s에서 분리한 문자열을 빼고 남은 부분에 대해서 이 과정을 반복합니다. 남은 부분이 없다면 종료합니다.
- 만약 두 횟수가 다른 상태에서 더 이상 읽을 글자가 없다면, 역시 지금까지 읽은 문자열을 분리하고, 종료합니다.
>
> 문자열 s가 매개변수로 주어질 때, 위 과정과 같이 문자열들로 분해하고, 분해한 문자열의 개수를 return 하는 함수 solution을 완성하세요.

***

### 🌱 제한사항
- 1 ≤ s의 길이 ≤ 10,000
- s는 영어 소문자로만 이루어져 있습니다.

***

### 🌱 입출력 예

|**s**|**result**|   
|"banana"|3|   
|"abracadabra"|6|   
|"aaabbaccccabba"|3|

***

#### 🪐 입출력 예 설명
- **입출력 예 #1**   
s = "banana"인 경우 ba - na - na와 같이 분해됩니다.

- **입출력 예 #2**   
s = "abracadabra"인 경우 ab - ra - ca - da - br - a와 같이 분해됩니다.

- **입출력 예 #3**   
s = "aaabbaccccabba"인 경우 aaabbacc - ccab - ba와 같이 분해됩니다.

***

## 👻 풀이

```c++
#include <string>
#include <vector>

using namespace std;

int solution(string s) {
    int answer = 0;
    int x = 1;
    int y = 0;
    int cnt = 1;
    while (s.size())
    {
        if (s[0] == s[cnt]) x++;
        else y++;
        
        if (x == y)
        {
            answer++;
            if (cnt + 1 < s.size()) 
            {
                s = s.substr(cnt + 1);
                x = 1; y = 0; cnt = 0;
            }
            else
                break;
        }
        
        if (cnt + 1 >= s.size())
        {
            answer++;
            break;
        }
        cnt++;
    }
    return answer;
}
```

***

## 👻 글을 마치며
백준 브론즈 단계만 풀다가 약간 인터페이스에 질려서 프로그래머스로 환기시키려 했는데, 레벨 1짜린데도 어려워서 혼났다.. 어찌저찌 풀긴 했는데 백준과는 다르게 내가 작성한 코드의 효율성을 알아보기가 힘든 것 같다. 그래도 그 많은 테스트 케이스를 통과한 거니까 괜찮은 코드겠지..?

***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv1/140108.cpp)_