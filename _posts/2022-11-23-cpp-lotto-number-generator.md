---
title: "[C++] 연습 문제 : 로또 번호 생성기"
excerpt: "로또 번호를 생성하는 프로그램을 만들어보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, pointer, practice, lotto, number, generator]

permalink: /cpp/lotto-number-generator/

toc: true
toc_sticky: true

date: 2022-11-23 20:39:42+0900
last_modified_at: 2022-11-23 20:39:44+0900
---

## 👻 로또 번호 생성기
이번 시간에는 지난 시간에 배웠던 배열을 이용해 로또 번호를 생성해주는 기능을 구현해 볼 것이다. **중복이 안 된 6개의 숫자**를 프로그램을 구동시킬 때마다 추출시켜주도록 만들어보았다.

***

### 🌱 정렬 기능 만들기
우선 로또 번호를 추출한 다음 오름차순으로 정렬해주기 위해 정렬 함수를 만들었다.

```c++
void Sort(int numbers[], int count) {
    int temp = 0;
    for (int i = 0; i < count - 1; i++) {
        for (int j = i + 1; j < count; j++) {
            if (numbers[i] > numbers[j]) {
                Swap(numbers[i], numbers[j]);
            }
        }
    }
}
```

배열과 배열 길이를 매개 변수로 받고 ``` for문 ```으로 탐색 후 정렬해주는 함수를 만들었다. 내가 생각한 건 둘둘씩 비교해서 조건에 맞게 값을 바꿔주는 알고리즘이었다. 찾아보니 **버블 정렬** 방식이라고 한다. 왼쪽 값을 기준으로, 오른쪽 값들을 비교하며 왼쪽 값이 더 크면 swap 해주는 방식이다.

- **Swap 함수**   
두 개의 값을 바꿔주는 **Swap 함수**는 참조 방식을 이용하여 간단하게 구현할 수 있었다.

```c++
void Swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}
```

***

### 🌱 로또 번호 생성기 만들기
우선 로직을 먼저 생각하는 시간을 가졌었다. ~~30분 정도 걸렸던 듯..~~   
랜덤으로 1~45 사이의 숫자 중 6개를 골라서 입력 받은 매개 변수 ``` numbers ``` 배열에 넣는 방식의 함수를 만들면 됐었다. 단, 중복이 없어야 하기 때문에 **무한 체크**를 하는 구간이 필요했다.

```c++
void ChooseLotto(int numbers[]) {
    // TODO : 랜덤으로 1~45 사이의 숫자 6개 골라서 numbers에 넣기 (단, 중복이 없어야 함)
    srand((unsigned)time(0));
    
    bool hasDuplication = false;
    for (int i = 0; i < 6; i++) {
        numbers[i] = (rand() % 45) + 1;
    }

    hasDuplication = CheckDuplication(numbers);

    // 중복 체크
    while (hasDuplication) {
        for (int i = 0; i < 6; i++) {
            if (numbers[i] == 0) numbers[i] = (rand() % 45) + 1;
        }

        hasDuplication = CheckDuplication(numbers);
    }

    // 정렬
    Sort(numbers, 6);
}
```

대략적인 코드 분석은 다음과 같다.

- **난수 시드 설정**   

```c++
srand((unsigned)time(0));
```

- **중복 체크 유무를 확인하기 위해 설정**

```c++
bool hasDuplication = false;
```

- **6개의 난수 생성 및 numbers 배열에 추가**

```c++
for (int i = 0; i < 6; i++) {
    numbers[i] = (rand() % 45) + 1;
}
```

- **중복 체크 유무 위한 불리언 값 설정**

```c++
hasDuplication = CheckDuplication(numbers);
```

여기서 중복 체크하는 부분을 따로 뺐다.   

```c++
bool CheckDuplication(int numbers[]) {
    bool ret = false;
    for (int i = 0; i < 5; i++) {
        for (int j = i + 1; j < 6; j++) {
            if (numbers[i] == numbers[j]) {
                // 중복 있으면 뒤 숫자 0으로 바꿈
                numbers[j] = 0;
                ret = true;
            }
        }
    }

    return ret;
}
```

중복이 있다면 오른쪽(비교 당하는) 수를 **0**으로 세팅해주고 중복이었다는 정보를 ``` true ```로 반환해주었다.

- **중복 없을 때까지 체크**   
중복이었던 수는 0으로 세팅되어 있는데, 이를 한 번 더 체크해서 0이면 랜덤 수를 한 번 더 다시 세팅해준다. 세팅이 끝나면 중복 체크를 다시 시도한다.

```c++
while (hasDuplication) {
    for (int i = 0; i < 6; i++) {
        if (numbers[i] == 0) numbers[i] = (rand() % 45) + 1;
    }

    hasDuplication = CheckDuplication(numbers);
}
```

***

## 👻 글을 마치며
지난 시간에 배웠던 배열을 확실히 익히기 위해 로또 번호를 생성해주는 프로그램을 구현해보았다. 아무래도 C++로 처음 배열을 다루다보니 배열을 여기저기에 선언해보기도하고 오류도 많이 경험해보기도 한 것 같다. 분명히 포인터로 바뀌니, 주소값이라는 것을 아는데도 cout으로 찍어보고 있다.. 그래도 다 만들고 나니까 어느정도 배열의 범위 개념을 익힐 수 있었던 것 같다. 배열은 원본값을 건드리니 값 변경이 쉽고 빨라서 편한 것 같다. 그대신 시점이 애매해서 활용하기가 조금 힘든 것 같았다. (사이즈 구하는 거 같은 거..)

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/pointer/practice/lotto-number-generator)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   