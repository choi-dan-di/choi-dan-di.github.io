---
title: "[C++] 전역 변수, 지역 변수와 값 전달"
excerpt: "범위에 따른 변수의 개념과 값을 어떻게 전달하는지에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, function, scope, local variable, variable, local, pass value, global variable, global]

permalink: /cpp/scope-variables/

toc: true
toc_sticky: true

date: 2022-11-14 17:15:12
last_modified_at: 2022-11-14 17:15:14
---

## 👻 전역 변수(global variable)
어느 곳에서나 접근하여 사용할 수 있는 변수를 의미한다. 선언 방법은 변수 선언과 동일하고 함수 외부에서 선언하면 전역 변수에 포함된다.

메모리에서 **데이터 영역**에 들어가지만 초기화 여부에 따라 .data, .bss, .rodata로 나눠진다.

```c++
int globalValue = 0;

voit Test()
{
    cout << "전역 변수의 값은 " << globalValue << endl;
    globalValue++;
}

int main()
{
    cout << "전역 변수의 값은 " << globalValue << endl;
    globalValue++;
    
    Test();
}
```

- 결과   
![Alt Text](/assets/images/posts_img/basics/cpp/function/scope-variables/global-variable.PNG)   

연속된 같은 변수를 사용한다는 것을 알 수 있다.

***

## 👻 지역 변수(local variable)
전역 변수와는 반대로 함수 내에서 선언되어 해당 함수 범위에서만 사용 가능한 변수를 의미한다. 선언 방법은 동일하나 선언 위치가 전역 변수와는 반대로 함수 내부에 위치한다. 외부에서 접근이 불가하기 때문에 전역 변수보다 다루기가 까다롭다.

메모리에서 **스택 영역**에 들어간다.

```c++
int main()
{
  int localValue = 1;
}
```

> 💡 **전역 변수와 지역 변수는 메모리에서 할당받는 영역이 다르다.**   
![Alt Text](/assets/images/posts_img/basics/cpp/function/scope-variables/address.PNG)   

***

### 🌱 범위 확인해보기
간단한 테스트를 통해 지역 변수의 값이 어떻게 전달되는 지 알아보자.

```c++
void IncreaseHp(int hp)
{
    hp = hp + 1;
}

int main()
{
    int hp = 1;

    cout << "Increase 호출 전 : " << hp << endl;
    IncreaseHp(hp);
    cout << "Increase 호출 후 : " << hp << endl;
}
```

이러한 코드가 있을 때, 콘솔에 찍히는 hp의 값은 각각 어떻게 될까?   
1과 2가 뜰 것 같지만 1과 1이 뜨게 된다.

- 결과   
![Alt Text](/assets/images/posts_img/basics/cpp/function/scope-variables/local-variable.PNG)   

> **왜?**   
스택 프레임의 데이터 이동 방식을 생각해야한다. ``` main ``` 함수에서 선언한 지역변수 hp와 ``` IncreaseHp ``` 함수가 받는 매개변수 hp는 서로 다른 주소값을 할당받은 **다른 변수**이다. 변수 이름이 같다고 혼동하지 말아야한다.

***

## 👻 글을 마치며
이번 시간에는 전역 변수와 지역 변수의 차이점, 그리고 지역 변수의 값이 어떻게 이동하는지에 대해 알아보았다. 범위는 알고 있었는데 지역 변수의 자세한 데이터 이동은 잘 몰랐었다. 저번 시간부터 계속 배워왔던 스택 프레임을 떠올리고 머릿속으로 그림을 그리다보니 데이터 이동에 대해 이해가 잘 된 것 같다. 앞으로 변수 이름이 같아도 혼동하지 않을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/function/scope-variables)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   