---
title: "[C++] 디버깅(Debugging)"
excerpt: "C++ 코드를 디버깅하는 방법에 대해 알아보고 연습 문제 풀어보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, debug, debugging]

permalink: /cpp/debugging/

toc: true
toc_sticky: true

date: 2022-12-15 15:20:21+0900
last_modified_at: 2022-12-15 15:20:23+0900
---
 
## 👻 디버깅
C++ 코드를 디버깅하는 방법에 대해 알아보자.

- **디버그 시작** : F5

- **디버그 중지** : Shift + F5

- **프로시저 단위 실행** : F10

- **한 단계씩 코드 실행** : F11
  - 함수 실행시 내부로 들어갈 수 있다.

- **Break Point(중단점)** : F9

- **조사식**
  - 일반적으로 할 수 있는 모든 것들을 실행해볼 수 있다. (주소값, 값, 계산식 등)

- **호출 스택**
  - 함수 실행 순서를 알 수 있다.

- **로그 찍기** : 중단점 우클릭 + 작업
  - 출력코드 대신 로그를 출력해줄 수 있다.   
![Alt Text](/assets/images/posts_img/basics/cpp/debugging/break-point.PNG)   
![Alt Text](/assets/images/posts_img/basics/cpp/debugging/result.PNG)   

> 문제가 발생했을 때 눈으로 찾기보단 디버깅을 우선으로 실행하려는 자세가 필요하다.

***

## 👻 새 프로젝트 추가
한 솔루션 안에 여러개의 프로젝트를 생성해 독립적으로 실행시킬 수 있다.    
~~난 왜 이걸 이제 알았나 😭~~

![Alt Text](/assets/images/posts_img/basics/cpp/debugging/solution.PNG)   

솔루션 우클릭 👉 추가 👉 새 프로젝트를 이용해 추가해줄 수 있다.

> - 각 프로젝트를 우클릭하여 시작 프로젝트로 설정할 수 있다.
- 여러 개의 프로젝트를 동시에 실행시키고 싶다면 솔루션의 속성으로 들어가 설정할 수 있다.

***

## 👻 연습 문제
<span style="font-size: 0.7em; color: gray;">문제는 총 10문제고 풀이는 대표적으로 한 문제만 기록하였다.</span>

- Exercise.cpp

```c++
#include <iostream>
using namespace std;
#include "Knight.h"

int main()
{
	Knight* k1 = new Knight();
	k1->PrintInfo();
	
	Knight* k2 = new Knight(200);
	k2->PrintInfo();

	delete k1;
	delete k2;
}
```

- Knight.h

```c++
#pragma once

class Knight
{
public:
	Knight();
	Knight(int hp);
	~Knight();

	void PrintInfo();

public:
	int _hp;
	int _attack;
};
```

- Knight.cpp

```c++
#include "Knight.h"
#include <iostream>
using namespace std;

// [사양서] 기본값 Hp=100, Attack=10
Knight::Knight() : _hp(100), _attack(10)
{

}

Knight::Knight(int hp) : _hp(hp)
{

}

Knight::~Knight()
{

}

void Knight::PrintInfo()
{
	cout << "HP: " << _hp << endl;
	cout << "ATT: " << _attack << endl;
}
```

> - [사양서]
  - 기사는 생명력(hp), 공격력(attack)을 갖고 있으며 기본값은 Hp = 100, Attack = 10입니다. 원활한 게임 진행을 위해 기사를 2개 생성하고 1번 기사는 기본값으로, 2번 기사는 Hp를 200으로 올려서 설정합니다.
- [Bug Report]
  - 2번 기사의 정보가 사양서와 일치하지 않습니다.
  - 2번 기사의 공격력이 엉뚱한 값(음수)로 되어 있습니다.
    - 공격력이 음수로 설정된 원인을 찾아주세요.
    - 2번 기사의 공격력이 기본값(10)으로 설정되길 희망합니다.

***

### 🌱 풀이
위 코드의 실행 결과는 다음과 같다.

![Alt Text](/assets/images/posts_img/basics/cpp/debugging/result2.PNG)   

생각한 결과 2번 기사의 공격력이 이상하므로 클래스가 생성될 때에 중단점을 정하고 디버깅을 실행해보았다.

```c++
Knight* k2 = new Knight(200);
```

그런다음 **F11**을 이용해 어떤 함수를 호출하는 지 알아보았다. 확인 결과 ``` hp ```를 인자로 받는 생성자 ``` Knight(int hp) ```가 호출되는 것을 알았다. 해당 생성자는 ``` _hp ```의 값만 세팅해주고 ``` _attack ```은 따로 세팅해주지 않는다. 이 부분에서 공격력의 초기값이 세팅되지 않아 쓰레기 값이 들어가게 된 것 같다.

이러한 문제점을 해결해주기 위해 공격력의 기본값을 세팅해주는 코드를 추가해주었다.

```c++
Knight::Knight(int hp) : _hp(hp), _attack(10)
{

}
```

> 헤더 파일에서, 클래스 생성시 멤버 변수를 바로 초기화해줄 수 있으나 C++ 11 이상의 문법이라 우선은 생성자에서 초기값을 세팅해주었다.

- 수정 후의 결과

![Alt Text](/assets/images/posts_img/basics/cpp/debugging/result3.PNG)   

***

## 👻 글을 마치며
이번 시간에는 C++ 디버깅에 대해 조금 더 자세하게 알아보았다. 포스팅에는 하나 밖에 적지 않았지만 약 10개의 연습 문제를 통해 디버깅 연습을 많이 할 수 있었다. 확실히 코드가 길어질수록 에러를 찾기 힘들었는데 디버깅을 이용하니 쉽게 찾을 수 있었다. 코드를 작성하는 것도 중요하지만 버그를 잡기 위한 디버깅 응용법도 중요한 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/debugging)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   