---
title: "[C++] 호출 스택"
excerpt: "다중 함수 호출 시에 스택의 변화에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, function, call stack, stack]

permalink: /cpp/call-stack/

toc: true
toc_sticky: true

date: 2022-11-16 18:57:08+0900
last_modified_at: 2022-11-16 18:57:09+0900
---

## 👻 호출 스택
여러 함수를 호출(다중 함수 호출)했을 때 스택에서 데이터가 어떻게 이동하는지에 대해 알아보자.

```c++
void Func1()
{
    cout << "Func1" << endl;

    Func2(1, 2);
}

void Func2(int a, int b)
{
    cout << "Func2" << endl;

    Func3(10);
}

void Func3(float a)
{
    cout << "Func3" << endl;
}

int main()
{
    cout << "main" << endl;
    Func1();
}
```

다중 함수 호출을 위해 함수를 3개 만들어주고 메인에서 1번 함수를 호출하는 코드를 작성했다. 우선 ``` Ctrl + Shift + B ```를 눌러 **빌드**를 먼저 해보자.

![Alt Text](/assets/images/posts_img/basics/cpp/function/call-stack/error.PNG)   

그러면 오류가 나게 되는데, **C++**은 위에서부터 차례대로 빌드를 진행하기 때문이다. 1번 함수 내에 있는 ``` Func2 ```와 2번 함수 내에 있는 ``` Func3 ```이 **빌드 시에는 만들어져있지 않아서 생기는 오류**라고 볼 수 있다. 이러한 오류를 해결하기 위해 함수의 위치를 변경 후 다시 진행해보자.

```c++
void Func3(float a)
{
    cout << "Func3" << endl;
}

void Func2(int a, int b)
{
    cout << "Func2" << endl;

    Func3(10);
}

void Func1()
{
    cout << "Func1" << endl;

    Func2(1, 2);
}

int main()
{
    cout << "main" << endl;
    Func1();
}
```

이렇게 되면 빌드가 정상적으로 동작하는 것을 확인할 수 있다. 하지만 함수의 수가 많아지면 매번 프로젝트를 진행할 때마다 함수의 순서를 맞춰주기는 **불가능**에 가깝다.

어셈블리에서 함수 호출은 ``` call ```이라는 명령어를 사용했었다. 함수의 위치만 알면 해당 위치로 이동이 가능하기 때문에 위의 오류를 방지하기 위해서는 함수의 순서를 맞춰주기보다 **함수 선언**을 먼저 진행해주어 해당 함수가 있다는 것만 알려주면 된다.

***

### 🌱 함수 선언
함수를 통으로 정의하는 것보다 선언을 우선적으로 해주면 해당 함수의 위치만 우선 저장되어 정의하기 전에도 해당 함수로 이동할 수 있게 된다.

```c++
// [반환타입] [함수이름]([인자타입 매개 변수]);
void Func1();
void Func2(int a, int b);
void Func3(float a);
```

> 매개 변수의 이름은 생략 가능하지만 가능한 어떻게 사용되어지는지를 알게 하기위해 정해두는 편이 좋다.

함수의 정확한 기능은 아직 알 수 없지만 위치는 알기 때문에 어디로 이동해야할지 알 수 있게 된다. 기능은 함수를 정의하면서 정해진다. 함수 선언을 하게되면 함수의 위치는 크게 중요하지 않다.

> 💡 **링크 에러**   
함수를 선언만 해두고 정의하지 않게되면 링크 에러가 난다. ``` 전처리 👉 컴파일 👉 링크 ``` 과정의 빌드 시에 마지막인 **링크**부분에 해당 함수의 주소를 연결해줄 수 없으니 에러가 나게 되는 것이다.   
![Alt Text](/assets/images/posts_img/basics/cpp/function/call-stack/link-error.PNG)   

***

## 👻 호출 스택 살펴보기
프로젝트 크기가 커질수록 일일이 스택 하나하나를 열어보며 코드를 분석할 수 없다. 시간이 많이 걸리기 때문이다. 이러한 과정을 간단하게 알아볼 수 있도록 디버거에는 해당 기능이 있는 탭이 이미 만들어져있다. 디버깅 중 우측 하단에 있는 **호출 스택** 탭을 이용해 확인할 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/call-stack/call-stack.PNG)   

이 탭을 이용하면 브레이크 포인트에서 어떤 순서로 코드가 진행되는지, 실행되는 함수의 순서가 어떤지 알 수 있고 각 리스트를 더블클릭하면 해당 위치로 이동하면서도 볼 수 있다. 가장 위쪽이 마지막으로 스택에 쌓인 데이터를 의미한다.

``` Func1 ``` 내부에 ``` Func3 ```을 호출하는 코드를 추가한 뒤 디버깅을 다시 진행해보자.

![Alt Text](/assets/images/posts_img/basics/cpp/function/call-stack/break-point.PNG)   

브레이크 포인트는 ``` Func3 ``` 내에 잡은 후, F5를 눌러 진행해보면 처음엔 호출 스택에 메인에서 시작해 들어온 정보가 뜨게 되고, 한 번 더 F5를 누르면 ``` Func1 ```에서 호출이 되었을 때의 스택 정보가 보여지게 된다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/call-stack/call-stack2.PNG)   
👆 F5를 한 번 더 눌러 진행했을 때의 호출 스택

해당 기능을 잘 사용하면 코드의 진행 방식을 알 수 있게되어 디버깅 속도가 빨라진다.

***

## 👻 글을 마치며
이번 시간에는 호출 스택에 대해 알아보았다. 함수를 여러번 호출했을 때의 순서를 디버거로 편리하게 볼 수 있다는 것을 알게되었다. 프로젝트를 진행하다가 헷갈리는 부분이 있을 때 적극 활용하면 디버깅 시간이 단축되어 좀 더 효율적으로 일을 진행시킬 수 있을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/function/call-stack)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   