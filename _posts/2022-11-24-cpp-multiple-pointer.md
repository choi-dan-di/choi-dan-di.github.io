---
title: "[C++] 다중 포인터"
excerpt: "다중 포인터의 개념에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, pointer, ref, basic, reference, multiple pointer]

permalink: /cpp/multiple-pointer/

toc: true
toc_sticky: true

date: 2022-11-24 13:54:44+0900
last_modified_at: 2022-11-24 13:54:47+0900
---

## 👻 다중 포인터
**다중 포인터**는 **포인터를 중첩하여 사용**하는 것을 의미한다. ``` * ```을 두 번, 세 번 붙여서 각각 이중, 삼중 포인터를 만들 수 있다. 이렇게 만들게 되면 **포인터 주소값이 담긴 바구니의 주소값**을 가리키게 된다.

```c++
void SetMessage(const char* a) {
	a = "Bye";
}

int main() {
	const char* msg = "Hello";
	SetMessage(msg);
}
```

위와 같은 코드가 있다. 언뜻 보면 매개 변수로 포인터를 넘겨주기 때문에 ``` msg ```의 값이 ``` Bye ```로 변경될 것 같지만 결과는 ``` Hello ```가 출력된다.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/multiple-pointer/stack-frame.jpg)   

스택 프레임을 살펴보면 다음과 같다.

``` main ``` 함수에서 ``` SetMessage(msg) ```가 실행이 되면 ``` main ``` 스택 프레임의 지역 변수에 ``` msg ```가 담기게 되고, 함수가 호출되면서 함수 내부 스택 프레임의 매개 변수가 세팅이 된다.(``` a = msg ```)

하지만 함수 내부에서 실행하는 코드 ``` a = "Bye"; ```는 a의 값을 Bye로 바꾸라는 말인데 포인터가 들어가면 a의 값을 **Bye의 시작 주소**로 변경하라는 말이 된다. 일반 변수의 값을 복사하던 방법과 동일한 기능을 하는 함수가 된 것이다.

그래서 a가 가리키는 주소의 값 ``` Hello ```가 아닌, a의 값 자체를 건드리게 되면서 애꿎은 주소만 변경하고 있었던 것이다. 함수 내부에서 ``` msg ``` 값만 바뀌다가 함수 호출이 종료되면 해당 스택 프레임도 사라지면서 변경된 내역이 사라지게 되니 말이다. 

이러한 점을 개선하기 위해 **다중 포인터**를 사용한다.

```c++
void SetMessage(const char** a) {
	*a = "Bye";
}

int main() {
	const char* msg = "Hello";
	SetMessage(&msg);
}
```

이렇게 되면 위의 문제점을 해결할 수 있게 된다. ``` 값 복사 👉 원본 직접 접근 ```과 동일한 개선 단계라고 볼 수 있다. 

다중 포인터를 사용함으로써 함수 내의 ``` a ```는 ``` Hello ```의 주소를 담는 대신 ``` &msg ```. 즉, ``` Hello ```**의 시작 주소를 담고 있는** ``` msg ```**의 주소**를 담게 된다. 

이렇게 되면 함수 스택 프레임 내의 매개 변수에서도 ``` msg ```로의 접근이 가능하게 되면서 ``` Hello ```라는 값도 직접 건드릴 수 있게 되는 것이다. **주소의 주소**를 담는다고 생각하면 이해가 쉽다.

> 물론 **참조 방식**도 사용 가능하다. 기본적으로 기능은 동일하기 때문이다.   
>
> ```c++
> void SetMessage2(const char*& a) {
>   a = "Ref Bye";
> }
> 
> int main() {
>   SetMessage2(msg);
> }
> ```

***

## 👻 글을 마치며
이번 시간에는 다중 포인터에 대해 알아보았다. 이 부분은 처음 접하는 것이었어서 초반에는 살짝 헷갈렸었는데 포인터의 개념을 이해하고 있고 여러번 반복해서 보다보니 이해하는 데 크게 어렵지 않았다. 주소의 주소를 타고 간다고 생각하니 재미있었던 것 같고 예전에는 몰랐지만 지금 보니 숨겨져있던 프로그래밍 규칙들이 하나 둘 보이는 것 같다. 그 점이 굉장히 신기하다. ㅎㅎ 

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/pointer/multiple-pointer)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   