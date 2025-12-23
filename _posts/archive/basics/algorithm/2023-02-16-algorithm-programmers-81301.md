---
title: "[Programmers] Lv.1 - 숫자 문자열과 영단어"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 81301, lv1]

permalink: /algorithm/programmers-81301/

toc: true
toc_sticky: true

date: 2023-02-16 23:32:47+0900
last_modified_at: 2023-02-16 23:32:51+0900
---
 
## 👻 숫자 문자열과 영단어
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/81301?language=cpp)

***

### 🌱 문제
> ![Alt Text](/assets/images/posts_img/basics/algorithm/programmers-81301/img1.png)   
네오와 프로도가 숫자놀이를 하고 있습니다. 네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.
>
> 다음은 숫자의 일부 자릿수를 영단어로 바꾸는 예시입니다.   
- 1478 👉 "one4seveneight"
- 234567 👉 "23four5six7"
- 10203 👉 "1zerotwozero3"   
> 
> 이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 ``` s ```가 매개변수로 주어집니다.   
``` s ```가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.   
참고로 각 숫자에 대응되는 영단어는 다음 표와 같습니다.   
>
> |**숫자**|**영단어**|   
> |0|zero|   
> |1|one|   
> |2|two|   
> |3|three|   
> |4|four|   
> |5|five|   
> |6|six|   
> |7|seven|   
> |8|eight|   
> |9|nine|

***

### 🌱 제한사항
- 1 ≤ ``` s ```의 길이 ≤ 50
- ``` s ```가 "zero" 또는 "0"으로 시작하는 경우는 주어지지 않습니다.
- return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 ``` s ```로 주어집니다.

***

### 🌱 입출력 예

|**s**|**result**|   
|"one4seveneight"|1478|   
|"23four5six7"|234567|   
|"2three45sixseven"|234567|   
|"123"|123|

***

### 🌱 입출력 예 설명
**입출력 예 #1**   
- 문제 예시와 같습니다.   

**입출력 예 #2**   
- 문제 예시와 같습니다.

**입출력 예 #3**   
- "three"는 3, "six"는 6, "seven"은 7에 대응되기 때문에 정답은 입출력 예 #2와 같은 234567이 됩니다.
- 입출력 예 #2와 #3과 같이 같은 정답을 가리키는 문자열이 여러 가지가 나올 수 있습니다.

**입출력 예 #4**   
- ``` s ```에는 영단어로 바뀐 부분이 없습니다.

***

### 🌱 제한시간 안내
- 정확성 테스트 : 10초

***

## 👻 풀이

```c++
#include <string>
#include <unordered_map>

using namespace std;

unordered_map<int, string> m = 
{
    { 0, "zero" },
    { 1, "one" },
    { 2, "two" },
    { 3, "three" },
    { 4, "four" },
    { 5, "five" },
    { 6, "six" },
    { 7, "seven" },
    { 8, "eight" },
    { 9, "nine" }
};

int solution(string s) {
    for (int i = 0; i < 10; i++)
    {
        auto n = s.find(m[i]);
        while (n != string::npos)
        {
            s.replace(n, m[i].size(), to_string(i));
            n = s.find(m[i]);
        }
            
    }
    return stoi(s);
}
```

***

## 👻 글을 마치며
오랜만에 2021카카오 채용연계형 인턴십 코테로 출제된 문제를 풀어보았다. 워낙에 알고리즘 실력이 뒤쳐져 있는지라, 코테에 나왔던 문제들은 쳐다도 안 봤었는데 그래도 요즘 공부하면서 자신감이 좀 붙어서 도전해보았다. 레벨 1인데 못 풀면 어쩌나 하고 걱정했지만 그래도 어찌저찌 잘 풀어나간 것 같다. 다른 사람들의 풀이를 봐도 별반 차이가 없었고, 제일 간단하고 인기 많았던 방법을 원래 사용하려 했었는데 함수 활용도가 떨어져서 사용하지 못한 게 아쉬웠다. 여러 문제를 더 풀어보면서 활용법들을 확실히 익혀야 할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv1/81301.cpp)_