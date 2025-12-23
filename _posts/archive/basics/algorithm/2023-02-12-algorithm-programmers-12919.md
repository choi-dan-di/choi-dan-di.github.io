---
title: "[Programmers] Lv.1 - 서울에서 김서방 찾기"
excerpt: "프로그래머스 코딩 테스트 연습 문제 풀이"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, programmers, 12919, lv1]

permalink: /algorithm/programmers-12919/

toc: true
toc_sticky: true

date: 2023-02-12 22:15:06+0900
last_modified_at: 2023-02-12 22:15:11+0900
---
 
## 👻 서울에서 김서방 찾기
[👉🏻 문제 보러가기 👈🏻](https://school.programmers.co.kr/learn/courses/30/lessons/12919?language=cpp)

***

### 🌱 문제
> String형 배열 seoul의 element중 "Kim"의 위치 x를 찾아, "김서방은 x에 있다"는 String을 반환하는 함수, solution을 완성하세요. seoul에 "Kim"은 오직 한 번만 나타나며 잘못된 값이 입력되는 경우는 없습니다.

***

### 🌱 제한사항
- seoul은 길이 1 이상, 1000 이하인 배열입니다.
- seoul의 원소는 길이 1 이상, 20 이하인 문자열입니다.
- "Kim"은 반드시 seoul 안에 포함되어 있습니다.

***

### 🌱 입출력 예

|**seoul**|**return**|   
|["Jane", "Kim"]|"김서방은 1에 있다"|

***

## 👻 풀이

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

string solution(vector<string> seoul) {
    string answer = "김서방은 " + to_string(find(seoul.begin(), seoul.end(), "Kim") - seoul.begin()) + "에 있다";
    return answer;
}
```

***

## 👻 글을 마치며
코드의 줄 수를 줄이려고 하다보니 한 줄로 더럽게 작성되었는데, 이럴 바엔 코드 수를 포기하고 가독성을 늘리는 방식이 좋아보인다. 기존의 함수를 최대한 이용하려 했고, 문자열을 합치는 과정에서 + 오퍼레이터를 사용했는데 타입이 안 맞아서 별도의 변환이 필요하다는 것을 알게 되었다. ~~자바스크립트를 사용 할 때는 그냥 알아서 변환해주니 신경쓰지 않아도 됐는데 C++에서는 신경을 꼭 써줘야하는 것 같다.~~

- **넘 웃겨서 가져와 본 코드🤣**   
![Alt Text](/assets/images/posts_img/basics/algorithm/programmers-12919/code1.PNG)   


***

_[소스코드 보러가기](https://github.com/choi-dan-di/algorithms/blob/main/Programmers/Lv1/12919.cpp)_