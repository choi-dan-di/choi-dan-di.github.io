---
title: "[C++] 연습 문제 - 별찍기와 구구단"
excerpt: "이제까지 배운 내용을 토대로 예제를 풀어보며 연습하기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, practice, star, multiplication table]

permalink: /cpp/star-and-multiplication-table/

toc: true
toc_sticky: true

date: 2022-11-11 17:08:51+0900
last_modified_at: 2022-11-11 17:08:53+0900
---

## 👻 입출력 알아보기
입출력에 대해 간단하게 알아보자.

```c++
cout << "Hello World" << endl;
```

- ``` cout ``` : **console out**의 줄임말로 출력을 의미한다.
- ``` endl ``` : **endline**의 줄임말로 한 줄 띄우기를 의미한다.  ``` \n ```와 같은 의미이다.

입력을 받고 싶을 땐 **cin**을 적고 화살표를 반대로 적으면 된다.

```c++
// 정수 타입의 값을 입력받아 input에 넣는다는 의미이다.
int input;
cin >> input;
```

- ``` cin ``` : **console in**의 줄임말로 입력을 의미한다.

***

## 👻 별 찍기
간단하게 별 찍기를 통해 반복문을 익혀보자.

***

### 🌱 문제 1 - 사각형 형태
> 유저들이 어떤 정수(n)를 입력하면 **n*n**개의 별을 찍어서 출력하기

```c++
int input;
cout << "정수를 입력해주세요. : ";
cin >> input;   // 입력받은 값을 input에 넣기

for (int i = 0; i < input; i++) // 줄 수
{
    for (int j = 0; j < input; j++) // 한 줄에 찍히는 별 개수
    {
        cout << "*";
    }
    cout << endl;   // 줄바꿈
}
```

- **결과**   
![Alt Text](/assets/images/posts_img/basics/cpp/flow-control/practice/star-and-multiplication-table/star.PNG)   

***

### 🌱 문제 2 - 삼각형 형태
> 1개부터 시작해서 순차적으로 줄마다 증가하는 별 출력하기

```c++
int input;
cout << "정수를 입력해주세요. : ";
cin >> input;   // 입력받은 값을 input에 넣기

for (int i = 1; i <= input; i++)
{
    for (int j = 0; j < i; j++)
    {
        cout << "*";
    }
    cout << endl;
}
```

- **결과**   
![Alt Text](/assets/images/posts_img/basics/cpp/flow-control/practice/star-and-multiplication-table/star2.PNG)   

***

### 🌱 문제 3 - 역삼각형 형태
> n개부터 시작해서 줄마다 1개씩 줄어드는 형태로 별 출력하기   

```c++
int input;
cout << "정수를 입력해주세요. : ";
cin >> input;   // 입력받은 값을 input에 넣기

for (int i = input; i > 0; i--)
{
    for (int j = i; j > 0; j--)
    {
        cout << "*";
    }
    cout << endl;
}

// 다른 버전
for (int i = 0; i < input; i++)
{
    for (int j = 0; j < (input - i); j++)
    {
        cout << "*";
    }
    cout << endl;
}
```

> 규칙을 찾으면 쉽게 알아낼 수 있다.

- **결과**   
![Alt Text](/assets/images/posts_img/basics/cpp/flow-control/practice/star-and-multiplication-table/star3.PNG)   

***

## 👻 구구단
> 2단부터 9단까지 출력하기

```c++
// 입력받는 값은 딱히 없음
for (int i = 2; i <= 9; i++)
{
    for (int j = 1; j <= 9; j++)
    {
        cout << i << " * " << j << " = " << (i * j) << endl;
    }
    cout << endl;   // 단이 바뀔 때 공백 한 줄 더 추가
}
```

- **결과**   
<span style="font-size: 0.7em; color: gray;">원래는 한 줄로 나오는데 너무 길어서 잘랐다.</span>   
<img src="/assets/images/posts_img/basics/cpp/flow-control/practice/star-and-multiplication-table/multiple.PNG" width="20%">
<img src="/assets/images/posts_img/basics/cpp/flow-control/practice/star-and-multiplication-table/multiple2.PNG" width="20%">

***

## 👻 글을 마치며
이번 시간엔 분기문과 반복문을 배우고나면 가장 먼저 연습하는 별 찍기와 구구단을 만들어보았다. 그래도 예전보다 실력이 많이 늘긴 늘었나보다. 별 찍기랑 구구단 만드는 데 얼마 안 걸리는 걸 보면..^^ 학부생 때는 손도 못 댔었던 것 같은데.. 하하 😅 그리고 다른 언어보다 C++이 뭔가 좀 더 입출력이 쉬운 것 같다. 좀 더 직관적이라고 해야하나.. 긴 코드가 없고 줄임말이 많아서 좋은 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/flow-control/practice/star-and-multiplication-table)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   