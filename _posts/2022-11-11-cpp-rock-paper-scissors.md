---
title: "[C++] 연습 문제 - 간단한 가위바위보 게임 만들기"
excerpt: "이제까지 배운 내용을 토대로 간단한 가위바위보 게임 만들어보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, practice, rock paper scissors]

permalink: /cpp/rock-paper-scissors/

toc: true
toc_sticky: true

date: 2022-11-11 17:46:12+0900
last_modified_at: 2022-11-11 17:46:14+0900
---

## 👻 난수 생성 함수 - rand()
컴퓨터와 가위바위보를 하는 게임을 만들어보려면, 우선 **난수 생성 함수**를 알아야한다. 그래야 비교해서 누가 이기고 졌는지를 알 수 있기 때문이다. ``` rand() ```라는 함수를 사용하면 난수를 생성할 수 있다.

```c++
// 난수 생성 함수
int value = rand(); // 0 ~ 32767

// 범위 설정 (0 ~ (n-1))
int value2 = rand() % n;

// 범위 설정2 (1 ~ n)
int value3 = (rand() % n) + 1;
```

수가 어떻게 생성되는 지 출력해보자.

```c++
int main()
{
    cout << rand() << endl;
    cout << rand() << endl;
    cout << rand() << endl;
    cout << rand() << endl;
    cout << rand() << endl;
}
```

그냥 이렇게만 쓰고 실행시켜보면 프로그램을 백번 재실행 시켜도 항상 동일한 값이 나오게된다.

- **결과**   
![Alt Text](/assets/images/posts_img/basics/cpp/flow-control/practice/rock-paper-scissors/rand.PNG)   

> 사실상 컴퓨터는 규칙이 없는 랜덤 숫자를 생성해낼 수 없다. 첫 특정값이 정해지면 그 수에 다양한 연산을 하는, 그러한 일련의 과정들을 거쳐서 다음 난수를 생성하는 방식인데 언뜻보면 랜덤한 숫자처럼 보여서 그냥 컴퓨터상의 랜덤이라 생각하면 될 듯하다.

결국 첫 번째 값을 구하는 게 중요한건데 ``` srand() ``` 함수를 통해 **시드값**을 설정해야한다. 매번 같은 값을 시드값으로 넣을 순 없으니 괄호 안에 현재 시간을 뜻하는 ``` time(0) ```을 넣어 매번 다르게 지정해주면 프로그램을 실행할 때마다 각각 다른 시드값으로 난수를 생성할 수 있게 된다.

```c++
int main()
{
    // 시드 설정
    srand(time(0));

    cout << rand() << endl;
    cout << rand() << endl;
    cout << rand() << endl;
    cout << rand() << endl;
    cout << rand() << endl;
}
```

이렇게 시드를 설정해주면 매번 다른 값이 나오게 된다.

***

## 👻 가위바위보 만들기
> 유저가 입력하면 컴퓨터와 비교해 승패를 결정해주는 게임을 만들어보기.   
(유효한 값이 입력되면 게임은 무한반복되고, 유효하지 않은 값이 입력되면 게임은 종료된다.)

- **내가 푼 코드**   

```c++
#include <iostream>
using namespace std;

int main()
{
    while(true)
    {
        srand(time(0));

        // 1 2 3
        int comInput = (rand() % 3) + 1;

        int userInput;

        cout << "가위(1) 바위(2) 보(3) 골라주세요!" << endl;
        cout << "> ";
        cin >> userInput;

        if (userInput > 3)
            break;

        switch (userInput)
        {
            case 1:
                cout << "가위(유저)";
                break;
            case 2:
                cout << "바위(유저)";
                break;
            case 3:
                cout << "보(유저)";
                break;
        }

        cout << " vs ";

        switch (comInput)
        {
            case 1:
                cout << "가위(컴퓨터)";
                break;
            case 2:
                cout << "바위(컴퓨터)";
                break;
            case 3 :
                cout << "보(컴퓨터)";
                break;
        }

        cout << " 결과 : ";

        // 1, 2 -> 2    // -1 패
        // 1, 3 -> 1    // -2 승
        // 2, 1 -> 2    // 1 승
        // 2, 3 -> 3    // -1 패
        // 3, 1 -> 1    // 2 패
        // 3, 2 -> 3    // 1 승
        if (userInput == comInput)
        {
            cout << "무승부입니다!" << endl;
        }
        else
        {
            int diff = userInput - comInput;
            if (diff == -1 || diff == 2)
                cout << "졌습니다!" << endl;
            else
                cout << "이겼습니다!" << endl;
        }

    }
}
```

- **다른 답**
```c++
int main()
{
    srand(time(0));

    const int SCISSORS = 1;
    const int ROCK = 2;
    const int PAPER = 3;

    while (true)
    {
        cout << "가위(1) 바위(2) 보(3) 골라주세요!" << endl;
        cout << "> ";

        // 컴퓨터
        int comInput = (rand() % 3) + 1;

        // 유저
        int userInput;
        cin >> userInput;

        if (userInput == SCISSORS)
        {
            switch (comInput)
            {
            case SCISSORS:
                cout << "가위(유저) vs 가위(컴퓨터) 결과 : 무승부입니다!" << endl;
                break;
            case ROCK:
                cout << "가위(유저) vs 바위(컴퓨터) 결과 : 졌습니다!" << endl;
                break;
            case PAPER:
                cout << "가위(유저) vs 보(컴퓨터) 결과 : 이겼습니다!" << endl;
                break;
            }
        }
        else if (userInput == ROCK)
        {
            switch (comInput)
            {
            case SCISSORS:
                cout << "바위(유저) vs 가위(컴퓨터) 결과 : 이겼습니다!" << endl;
                break;
            case ROCK:
                cout << "바위(유저) vs 바위(컴퓨터) 결과 : 무승부입니다!" << endl;
                break;
            case PAPER:
                cout << "바위(유저) vs 보(컴퓨터) 결과 : 졌습니다!" << endl;
                break;
            }
        }
        else if (userInput == PAPER)
        {
            switch (comInput)
            {
            case SCISSORS:
                cout << "보(유저) vs 가위(컴퓨터) 결과 : 졌습니다!" << endl;
                break;
            case ROCK:
                cout << "보(유저) vs 바위(컴퓨터) 결과 : 이겼습니다!" << endl;
                break;
            case PAPER:
                cout << "보(유저) vs 보(컴퓨터) 결과 : 무승부입니다!" << endl;
                break;
            }
        }
        else
        {
            // 예외의 값이 나오면 반복문(while(true)) 탈출
            break;
        }
    }
}
```

뭔가 내 코드가 더 간결해 보이는 것 같기도 하고..🤔?

- **결과**   
![Alt Text](/assets/images/posts_img/basics/cpp/flow-control/practice/rock-paper-scissors/result1.PNG)   

***

### 🌱 승률 추가하기
> 만들어 둔 가위바위보 게임에 **현재 승률**을 추가해보기 (단, 무승부는 제외)

```c++
int main()
{
    // 조금만 수정해주기
    srand(time(0));

    const int SCISSORS = 1;
    const int ROCK = 2;
    const int PAPER = 3;

    int round = 0;
    int win = 0;

    while(true)
    {

        // 1 2 3
        int comInput = (rand() % 3) + 1;

        int userInput;

        cout << "Round : " << round << ", win : " << win << endl;

        cout << "가위(1) 바위(2) 보(3) 골라주세요!" << endl;
        
        float winrate = 0;
        if (round > 0)
            winrate = (win / (float)round) * 100;

        cout << "> 현재 승률 : " << winrate << "%" << endl;
        cout << "> ";
        cin >> userInput;

        if (userInput > 3)
            break;

        switch (userInput)
        {
            case SCISSORS:
                cout << "가위(유저)";
                break;
            case ROCK:
                cout << "바위(유저)";
                break;
            case PAPER:
                cout << "보(유저)";
                break;
        }

        cout << " vs ";

        switch (comInput)
        {
            case SCISSORS:
                cout << "가위(컴퓨터)";
                break;
            case ROCK:
                cout << "바위(컴퓨터)";
                break;
            case PAPER:
                cout << "보(컴퓨터)";
                break;
        }

        cout << " 결과 : ";

        // 1, 2 -> 2    // -1 패
        // 1, 3 -> 1    // -2 승
        // 2, 1 -> 2    // 1 승
        // 2, 3 -> 3    // -1 패
        // 3, 1 -> 1    // 2 패
        // 3, 2 -> 3    // 1 승
        if (userInput == comInput)
        {
            cout << "무승부입니다!" << endl;
        }
        else
        {
            int diff = userInput - comInput;
            if (diff == -1 || diff == 2)
                cout << "졌습니다!" << endl;
            else
            {
                cout << "이겼습니다!" << endl;
                win++;
            }
            cout << endl;

            round++;
        }
    }
}
```

> **정수 / 정수**를 하게되면 소수점 아래는 자동으로 날아가기 때문에 주의해야한다. _둘 중 하나를 실수로 변환하여 나누거나 연산 순서를 바꿔서_ 알맞은 근사값을 구할 수 있다.

- **결과**   
![Alt Text](/assets/images/posts_img/basics/cpp/flow-control/practice/rock-paper-scissors/result2.PNG)   

***

## 👻 글을 마치며
이번 시간엔 가위바위보 게임을 간단하게 만들어보았다. 간단한 문제도 많은 생각을 해야 효율적으로 만들 수 있다는 것을 느꼈다. 그래서 어떻게하면 더 짧고 간결하며 효율적으로 만들 수 있을지 고민하느라 시간이 조금 걸렸던 것 같다. 그래도 직접 코드를 짜보니 코드 흐름을 생각하고 보는 능력은 향상된 것 같다. 이제 다른 연습 문제도 열심히 풀면서 문제를 보자마자 머릿속에서 풀릴 수 있도록 더 노력해야겠다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/flow-control/practice/rock-paper-scissors)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   