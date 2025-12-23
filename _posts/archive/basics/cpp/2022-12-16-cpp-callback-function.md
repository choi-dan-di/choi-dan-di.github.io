---
title: "[C++] 콜백 함수(Callback Function)"
excerpt: "콜백 함수에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, function, callback]

permalink: /cpp/callback-function/

toc: true
toc_sticky: true

date: 2022-12-16 20:12:08+0900
last_modified_at: 2022-12-16 20:12:12+0900
---

## 👻 콜백 함수
**콜백 함수(CallBack Function)**이란, 쉽게 말해 함수의 생성 시점과 호출 시점이 달라 일반 함수와는 다른 호출 방법을 갖는 함수이다. 일반 함수는 호출하는 즉시 실행되지만 콜백 함수는 미리 등록해 둔 다음 추후에 호출할 수도 있고 바로 호출할 수도 있다는 차이점이 있다. 물론 함수의 생성 장소와 다른 외부에서도 호출이 가능하며 주로 특정한 이벤트가 발생할 때 호출된다. **함수 포인터, 함수 객체, 템플릿**이 혼합된 개념이다.

***

### 🌱 콜백 함수 예시

```c++
class Item
{
public:

public:
    int _itemId = 0;
    int _rarity = 0;
    int _ownerId = 0;
};

// 함수 객체(Functor) 사용
class FindByOwnerId
{
public:
    bool operator()(const Item* item)
    {
        return (item->_ownerId == _ownerId);
    }
public:
    int _ownerId;
};

class FindByRarity
{
public:
    bool operator()(const Item* item)
    {
        return (item->_rarity >= _rarity);
    }
public:
    int _rarity;
};

// 템플릿 사용
template<typename T>
Item* FindItem(Item items[], int itemCount, T selector)
{
    for (int i = 0; i < itemCount; i++)
    {
        Item* item = &items[i];
        // TODO : 조건 체크
        if (selector(item))
            return item;
    }

    return nullptr;
}

int main()
{
    Item items[10];
    items[3]._ownerId = 100;
    items[8]._rarity = 2;

    FindByOwnerId functor1;
    functor1._ownerId = 100;

    FindByRarity functor2;
    functor2._rarity = 1;

    Item* item1 = FindItem(items, 10, functor1);
    Item* item2 = FindItem(items, 10, functor2);

    return 0;
}
```

***

## 👻 글을 마치며
이번 시간엔 앞서 배운 개념들을 통해 콜백 함수의 개념을 간단하게 알아보았다. 일반 함수와는 다르게 생성 시점과 호출 시점이 모두 다르고 호출되는 장소도 다르게 사용할 수 있는 것 같다.(클라이언트와 서버의 차이 정도?)

개념이 약간 미흡한 것 같아 조금 더 구글링을 해본 결과, 일반 함수는 클라이언트가 서버 측에 있는 함수를 가져와 호출하여 사용하는 반면, 콜백 함수는 클라이언트가 서버 측으로 함수를 전달하여 호출해달라는 의미라고 한다. 그래서 어떠한 상황(이벤트)이 일어났을 때 특정 기능을 호출해 준다는 상황이 성립된 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/callback-function/callback-function)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   