---
title: "[C++] 스마트 포인터(Smart Pointer)"
excerpt: "스마트 포인터에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, modern cpp, modern, smart pointer, pointer, smart]

permalink: /cpp/smart-pointer/

toc: true
toc_sticky: true

date: 2022-12-18 23:28:46+0900
last_modified_at: 2022-12-18 23:28:48+0900
---

## 👻 스마트 포인터
**스마트 포인터(Smart Pointer)**는 **포인터를 알맞은 정책에 따라 관리하는 객체**를 의미하며 포인터를 래핑해서 사용한다. 우리가 이제껏 배워왔던 일반 포인터는 물론 잘 사용하면 편리하게 사용할 수 있지만 **엉뚱한 메모리를 건드릴 수도 있다는 위험성**이 존재한다. 이러한 실수는 한 번만 해도 프로그램이 뻗는 치명적인 결과를 낳게된다. 이러한 일들을 방지하기 위해서 스마트 포인터를 사용하는 것이 좋다.

스마트 포인터에는 ``` shared_ptr ```, ``` weak_ptr ```, ``` unique_ptr ```가 있다.

***

### 🌱 shared_ptr
``` shared_ptr ```는 **참조 정보를 관리하고 객체의 생명 주기를 자동으로 관리**해주는 포인터이다. 참조 카운트를 관리하며 해당 포인터가 가리키는 객체를 다른 포인터들이 얼마나 참조하고 있는지 확인하기 위한 용도로 카운트 정보를 담고 있다. 참조 카운트가 0이 되면 메모리 해제를 고려한다.

```c++
shared_ptr<Knight> k = make_shared<Knight>();
```

> - ``` make_shared ```
    - 메모리 블록을 한 번에 만들어줘서 ``` new ```로 하는 객체 생성보다 훨씬 성능이 좋다.
    - ``` k(new Knight()) ```와 같은 의미이다.

해당 포인터를 사용하면 범위가 작아 원래라면 소멸될 객체도 누군가가 참조를 하고 있다면 소멸되지 않는 특징이 있다. 단, 사이클 발생시 삭제가 되지 않는 문제가 발생하게 된다.

```c++
shared_ptr<Knight> k5 = make_shared<Knight>();
{
    shared_ptr<Knight> k6 = make_shared<Knight>();
    k5->_target = k6;
    k6->_target = k5;    // 사이클 발생시 삭제가 되지 않는 문제 발생
    
    // 소멸 전에 nullptr로 밀어주는 부분이 필요하다.
    k5->_target = nullptr;
    k6->_target = nullptr;
}
```

***

### 🌱 weak_ptr
``` weak_ptr ```는 위에서 본 ``` shared_ptr ```의 단점을 보완해주는 포인터이다. 객체가 날라갔는지 유무를 확인하게 되고 경우에 따라 ``` shared_ptr ```를 반환해줘서 사이클 발생 같은 문제를 막아줄 수 있다.

```c++
// weak_ptr
void Attack() {
    if (_target.expired() == false) {
        shared_ptr<Knight> sptr = _target.lock();

        sptr->_hp -= _damage;
        cout << "HP : " << sptr->_hp << endl;
    }
}
```

> - **expired** : 해당 객체가 날라갔는지 확인. 아직 있으면 ``` false ```를 반환한다.
- **lock** : 해당 객체의 ``` shared_ptr ``` 타입을 반환한다.

***

### 🌱 unique_ptr
``` unique_ptr ```는 말 그대로 한 객체를 오로지 자기 자신만 가리키고 있어야 하는 포인터이다. 중복으로 가리키는 건 불가능하나 오른값 참조를 이용해 이동 연산은 가능하다.

```c++
unique_ptr<Knight> uptr = make_unique<Knight>();
// unique_ptr<Knight> uptr2 = uptr; // 불가능
unique_ptr<Knight> uptr2 = move(uptr);  // 오른값 참조 캐스팅
```

***

## 👻 글을 마치며
이번 시간에는 스마트 포인터에 대해 알아보았다. 이러한 문법이 있는지도 몰랐는데.. 사용 방법만 익숙해지면 요긴하게 잘 쓸 수 있을 것 같다. 처음엔 shared_ptr의 참조 카운터 계산법이 와닿지 않아서 고민을 많이 했었는데 단순하게 포인터 생성시 1부터 시작한다고 생각하면 그나마 이해가 가는 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/modern-cpp/smart-pointer)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   