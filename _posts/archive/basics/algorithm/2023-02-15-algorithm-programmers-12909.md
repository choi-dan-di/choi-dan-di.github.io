---
title: "[Programmers] Lv.2 - 올바른 괄호"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 12909, lv2]

permalink: /algorithm/programmers-12909/

toc: true
toc_sticky: true

date: 2023-02-15 01:44:32+0900
last_modified_at: 2023-02-15 01:44:36+0900
---
 
## 👻 올바른 괄호
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/12909?language=cpp)

***

### 🌱 문제
> 괄호가 바르게 짝지어졌다는 것은 '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫혀야 한다는 뜻입니다. 예를 들어   
- "()()" 또는 "(())()" 는 올바른 괄호입니다.
- ")()(" 또는 "(()(" 는 올바르지 않은 괄호입니다.   
> 
> '(' 또는 ')' 로만 이루어진 문자열 s가 주어졌을 때, 문자열 s가 올바른 괄호이면 true를 return 하고, 올바르지 않은 괄호이면 false를 return 하는 solution 함수를 완성해 주세요.

***

### 🌱 제한사항
- 문자열 s의 길이 : 100,000 이하의 자연수
- 문자열 s는 '(' 또는 ')' 로만 이루어져 있습니다.

***

### 🌱 입출력 예

|**s**|**answer**|   
|"()()"|true|   
|"(())()"|true|   
|")()("|false|   
|"(()("|false|

***

### 🌱 입출력 예 설명
입출력 예 #1, 2, 3, 4   
문제의 예시와 같습니다.

***

## 👻 풀이

```c++
#include <string>
#include <stack>

using namespace std;

bool solution(string s)
{
    stack<char> st;
    
    for (int i = 0; i < s.size(); i++)
    {
        st.push(s[i]);
        
        if (st.size() == 1)
        {
            if (st.top() == ')')
                break;
        }
        else if (st.size() > 1)
        {
            if (st.top() == ')')
            {
                st.pop();
                st.pop();
            }
        }
    }
    return st.empty();
}
```

***

## 👻 글을 마치며
시간 꽤나 걸렸던 것 같다. 처음엔 스택 string을 이용했었는데 효율성 테스트에서 계속 탈락을 했었고 string을 char로 바꿔주었더니 바로 통과했다. 문제를 꼼꼼히 읽으면 코드의 반 정도의 힌트를 얻을 수 있는 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv2/12909.cpp)_