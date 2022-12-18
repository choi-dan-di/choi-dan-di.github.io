---
title: "[C++] 람다(Lambda)"
excerpt: "람다 문법에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, modern cpp, modern, lambda]

permalink: /cpp/lambda/

toc: true
toc_sticky: true

date: 2022-12-18 22:07:28+0900
last_modified_at: 2022-12-18 22:07:30+0900
---

## 👻 람다(Lambda)
**람다(Lambda)**는 **함수 객체를 손쉽고 빠르게 만드는 문법**이다. 람다 자체로 C++11에 새로운 기능이 들어간 것은 아니고 일종의 **익명 함수(이름이 지어지지 않은 함수)**라고 볼 수 있다.

- **람다 표현식(Lambda Expression)**   

```c++
[캡처](인자값) {구현부}
```

함수 객체가 필요한 부분에 조금 더 간편하게 구현할 수 있는 장점이 있다.

```c++
// Functor 이용
struct IsUniqueItem {
    bool operator()(Item& item) {
        return item._rarity == Rarity::Unique;
    }
};

auto findIt = find_if(v.begin(), v.end(), IsUniqueItem());
if (findIt != v.end())
    cout << "아이템 ID : " << findIt->_itemId << endl;
```

기존의 함수 객체를 이용하면 비록 한 번만 사용하더라도 구조체를 만들어 지정을 해줬어야 했는데, 람다 표현식을 사용하면 한 줄로 구현이 가능하다.

```c++
// 람다 이용
auto isUniqueLambda = [](Item& item) { return item._rarity == Rarity::Unique; };
auto findIt2 = find_if(v.begin(), v.end(), isUniqueLambda);
if (findIt2 != v.end())
    cout << "아이템 ID : " << findIt2->_itemId << endl;
```

여기서 ``` isUniqueLambda ```는 **클로저(closure)**라고도 부른다. **람다에 의해 만들어진 실행 시점 객체**를 의미한다. ``` find_if ```에 람다식을 바로 넣을 수도 있다.

- **반환 타입 설정**

```c++
// 반환 타입 추가
auto isUniqueLambda = [](Item& item) -> int { return item._rarity == Rarity::Unique; };
```

***

### 🌱 캡처 모드
람다식의 **대괄호** ``` [ ] ```에 들어가는 부분을 **캡처**라고 한다. 해당 부분은 사진을 캡처 하듯, 일종의 스냅샷을 찍는다고 이해하면 편하며 함수 객체 내부에 변수를 저장하는 개념과 유사하다.

외부에 있는 변수를 사용할 수 있는데, 캡처 부분에서 값을 어떻게 가져올건지 방식을 정해줄 수 있다.

```c++
int itemId = 4;
auto findByItemIdLambda = [=](Item& item) { return item._itemId == itemId; };
itemId = 10;
```

> - ``` = ``` : 값 복사 방식
- ``` & ``` : 값 참조 방식

위의 코드가 있다고 가정했을 때, 복사 방식을 사용하게되면 람다식 뒤에 ``` itemId ```를 10으로 바꿨다 해도 이미 **4**가 저장되어 변하지 않는다. 반면에 참조 방식을 사용하게되면 ``` itemId ```의 참조 주소값을 가지고 있기 때문에 람다식 이후에 값이 변경되면 변경된 값을 가리키게 된다.

해당 캡처 부분의 방식 중엔 위처럼 ``` = ```, ``` & ```를 통해 모든 변수를 통틀어 관리할 수 있기도 하지만 변수 하나하나씩 방식을 지정해줄 수도 있다. 전자처럼 한데 묶어 관리하는 방식은 **지양**해야 하며 변수가 많더라도 하나씩 지정해주는 방식이 더 선호된다.

```c++
int itemId = 4;
Rarity rarity = Rarity::Unique;
ItemType type = ItemType::Weapon;

auto findItemLambda = [&itemId, rarity, type](Item& item) { 
    return item._itemId == itemId && item._rarity == rarity && item._type == type; 
};
```

> - **변수마다 캡처 모드를 지정해서 사용이 가능하다.**
  - ``` [변수명] ``` : 값 복사 방식
  - ``` &[변수명] ``` : 값 참조 방식

***

### 🌱 주의할 점
- 람다는 호출되는 범위 내에 존재하는 모든 변수들을 캡처할 수 있다. 클래스 멤버 함수 내에서 선언되면 ``` this ``` 포인터를 켑처할 수 있는데, 외부에서 호출할 때 문제가 발생할 수 있다.

```c++
class Knight {
public:
    auto ResetHpJob() {
        auto f = [this]() {
            this->_hp = 200;
        };

        return f;
    }

public:
    int _hp = 100;
};

Knight* k = new Knight();
auto job = k->ResetHpJob();
delete k;
job();
```

위와 같은 코드가 있다고 가정했을 때, 람다식이 ``` Knight ```의 멤버 함수 ``` ResetHpJob ``` 내부에서 선언되어있는 상태다. 람다식은 수행 시점과 선언 시점이 달라 위처럼 구현할 수 있는 것이다.

하지만 클래스 외부에서 객체를 만들고, 함수 실행문도 따로 저장해둔 다음 객체를 삭제한 후 함수를 실행시키면 이미 없어진 객체를 가리키는 람다식 내의 ``` this ```가 전혀 상관없는 메모리를 가리키게 된다. 이렇게 되면 메모리 오염이 발생하게 되는것이다.

이렇게 람다식을 사용할 때는 시점이 바뀌면서 유효함이 사라지지 않는지 신중하게 생각하고 구현해야 위 같은 에러를 방지할 수 있다. 해당 에러는 바로 알기가 힘들어 시간이 지난 후에 크래시가 날 확률이 높다.

***

## 👻 글을 마치며
이번 시간에는 람다 표현식에 대해 알아보았다. 예전부터 공부해보고 싶었던 부분인데 워낙 복잡해서 혼자서는 알기 힘들었던 기억이 난다. 그래도 강의를 듣고 여러 예제로 익히다보니 문법의 각 구문이 의미하는 것들도 하나 둘씩 이해가 되었다. 하지만 아직 캡처 블록에 대한 개념이 확실하게 잡히지 않아서 더 많은 예제를 풀어봐야 할 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/modern-cpp/lambda)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   