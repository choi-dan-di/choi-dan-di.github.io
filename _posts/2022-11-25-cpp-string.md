---
title: "[C++] 연습 문제 : 문자열"
excerpt: "문자열과 관련된 함수를 직접 만들어보며 C++ 익히기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, pointer, practice, string, const char, char]

permalink: /cpp/string/

toc: true
toc_sticky: true

date: 2022-11-25 19:35:06+0900
last_modified_at: 2022-11-25 19:35:09+0900
---
 
## 👻 문자열
**문자열**은 **문자가 여러개 모인 것**을 의미한다. 1바이트의 문자는 ``` char ``` 타입으로 값을 담고, 문자열은 ``` char* ``` 타입으로 문자가 들어있는 배열의 첫 주소를 지정해주는 식으로 만든다.

문자열과 관련된 함수들을 직접 만들어보면서 문자열을 좀 더 이해하고 문법을 익혀보자.

***

### 🌱 문자열 관련 함수 만들어보기
문자열과 관련된 함수들을 직접 만들어보면서 기능을 익혀보자.

***

#### 🪐 문자열 길이 출력
``` BUF_SIZE ``` 크기 만큼을 가지는 배열 ``` a ```를 하나 생성하고 길이를 출력해보자.

```c++
const int BUF_SIZE = 100;

char a[BUF_SIZE] = "Hello";
```

해당 배열은 크기 100을 가지는 배열이다. ``` sizeof ```를 사용해 배열의 길이를 출력하면 5가 아닌 **100**이 나오게 된다. 이러한 문자열의 길이를 출력하려면 ``` sizeof ``` 대신에 ``` strlen ``` 함수를 사용해야한다. 해당 함수를 사용하면 **5**가 출력된다.

``` strlen ``` 함수를 직접 만들어보자.

```c++
// 문자열의 길이를 반환해주는 함수
int StrLen(const char* str) {
    int ret = 0;

    while (str[ret])
        ret++;

    return ret;
}
```

``` while문 ```을 이용하여 쉽게 만들 수 있다. 해당 값이 ``` NULL ```이면 자동으로 빠져나오기 때문이다.

나는 ``` For문 ```을 이용해서 계속 만들려다보니 막혔었다. ``` sizeof ```를 써도 길이가 똑바로 안 나왔기 때문이다. 범위가 정확히 정해져있지 않을 땐 ``` while문 ```을 쓰도록 해야겠다.

***

#### 🪐 문자열 복사
``` BUF_SIZE ``` 크기 만큼을 가지는 배열 ``` b ```를 만들어 ``` a ```의 값을 복사하는 함수를 만들어보자.

기존에는 ``` strcpy ```라는 함수가 존재한다. 그냥 복사하려고 하면 **크기(버퍼) 문제**로 에러가 나기 때문에 ``` strcpy_s ```라는 함수를 사용하는 게 더 좋다. 

지금은 간단하게 복사를 하는 기능만 구현해보자.

```c++
// 문자열 복사 함수
void StrCpy(char* dest, char* src) {
    // src -> dest
    int i = 0;
    while (src[i]) {
        dest[i] = src[i];
        i++;
    }

    dest[i] = '\0';
}

// 포인터 사용 버전
void StrCpyByPtr(char* dest, char* src) {
    // src -> dest
    char* ret = dest;

    while (*src) {
        *dest = *src;
        dest++;
        src++;

        // *dest++ = *src++;
    }

    *dest = '\0';

    // 반환 타입이 있을 경우
    // return ret;
}
```

배열을 이용하면 배열의 번호를 이용해 쉽게 값을 덮어씌울 수 있다. 포인터 버전을 사용하게되면 포인터 주소값에 1을 더해 다음 주소로 넘어가서 값을 덮어씌워주는 작업을 하게된다. 둘은 사실상 같은 기능, 의미를 지닌다.

> ``` #pragma warning(disable: 4996) ```   
👉 디버깅 시 4996번 에러를 무시하라는 의미. 4996번 에러는 복사 시 오버플로우를 우려한 에러이다.

***

#### 🪐 문자열 붙이기
두 개의 문자열을 이어 붙이는 함수를 만들어보자.

```c++
char a[BUF_SIZE] = "Hello";
char b[BUF_SIZE] = "World";
```

다음과 같이 두 개의 문자열이 있을 때, a의 뒤에 b를 이어붙어 ``` HelloWorld ``` 라는 문자열을 배열 ``` a ```에 담으면 된다.

기존에는 ``` strcat ``` 함수가 해당 기능을 한다.

```c++
// 문자열 덧붙이는 함수
void StrCat(char* dest, char* src) {
    // dest 뒤에 src 붙이기
    int i = 0;
    int len = StrLen(dest);
    while (src[i]) {
        dest[len + i] = src[i];
        i++;
    }

    dest[len + i] = '\0';
}

// 포인터 사용 버전
void StrCatByPtr(char* dest, char* src) {
    // dest 뒤에 src 붙이기
    while (*dest) dest++;
    while (*src) *dest++ = *src++;
    *dest = '\0';
}
```

``` src ```의 값의 유무를 체크하면서 있으면 ``` dest ```의 NULL 부분부터 이어붙여주는 식으로 진행했다.

***

#### 🪐 문자열 비교
두 개의 문자열을 비교하여 그에 따른 알맞은 결과를 반환해주는 함수를 만들어보자.

다음과 같은 코드가 있다.

```c++
char a[BUF_SIZE] = "Hello";
char b[BUF_SIZE] = "Hello";
```

여기서 ``` a == b ```는 참일까 거짓일까? 정답은 **거짓**이다. 두 배열의 내용물은 같지만 a와 b 자체는 주소를 가리키고 있다. 두 문자열의 주소는 서로 다르기 때문에 식이 성립되지 않는 것이다.

기존엔 ``` strcmp ``` 함수가 존재한다. 두 문자열의 아스키코드를 비교하여 앞의 수가 크면 1을, 두 수가 같으면 0을, 뒤의 수가 크면 -1을 반환하는 식이다. 해당 함수와 같은 기능을 하는 함수를 구현해보자.

```c++
// 두 개의 문자열 비교
int StrCmp(char* a, char* b) {
    int aLen = StrLen(a);
    int bLen = StrLen(b);

    if (aLen > bLen) return 1;
    else if (aLen < bLen) return -1;

    // 길이 같으면
    for (int i = 0; i < aLen; i++) {
        if (a[i] > b[i]) return 1;
        else if (a[i] < b[i]) return -1;
    }

    return 0;
}

// 포인터 사용 버전
int StrCmpByPtr(char* a, char* b) {
    int aLen = StrLen(a);
    int bLen = StrLen(b);

    if (aLen > bLen) return 1;
    else if (aLen < bLen) return -1;

    // 길이 같으면
    while (*a) {
        if (*a > *b) return 1;
        else if (*a < *b) return -1;

        a++;
        b++;
    }

    return 0;
}
```

나는 길이를 먼저 비교했다. 그런 다음 길이가 같을 때 값을 하나씩 비교했다. a가 크면 1, b가 크면 -1을 반환하고 모두 같으면 0이 반환되는 형식이다.

***

#### 🪐 문자열 반전
문자열을 뒤집는 함수를 만들어보자. ``` Hello World ```를 입력한다면 ``` dlroW olleH ```를 출력해주는 기능을 하는 함수이다.

```c++
// 문자열을 뒤집는 함수
void ReverseStr(char* str) {
    // Hello World -> dlroW olleH
    int len = StrLen(str);

    for (int i = 0; i < len / 2; i++) {
        int temp = str[i];
        str[i] = str[len - 1 - i];
        str[len - 1 - i] = temp;
    }
}

// 포인터 사용 버전
void ReverseStrByPtr(char* str) {
    // Hello World -> dlroW olleH
    int len = StrLen(str);

    for (int i = 0; i < len / 2; i++) {
        char temp = *(str + i);
        *(str + i) = *(str + len - 1 - i);
        *(str + len - 1 - i) = temp;
    }
}
```

해당 문자열의 절반만 반복해서 돌며 가장 뒤부터 서로 Swap 하는 식으로 만들었다. 쉽게 만들 수 있었고 ``` temp ```라는 변수 타입을 ``` int ```로 해서 값을 받게 되면 아스키코드가 들어갔다가 다시 배열에 들어갈 때는 자동으로 ``` char ``` 형식으로 변환되어 들어가는 것을 확인할 수 있었다. 처음부터 ``` char ``` 타입으로 ``` temp ```를 만들면 문자가 이동되는 방식이다.

***

## 👻 글을 마치며
이번 시간엔 문자열을 건드리는 다양한 함수의 기능을 재현해보며 문자열에 대해 자세하게 알아보았다. 직접 함수를 구현해보며 어떻게 동작할지 생각해보니 이해가 더 잘 됐던 것 같다. 앞으로 여러가지 함수를 사용할텐데 이해가 잘 가지 않을 때 직접 그 기능을 구현해보며 학습하면 도움이 많이 될 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/pointer/practice/string)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   