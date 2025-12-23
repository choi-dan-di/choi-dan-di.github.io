---
title: "[C++] 포인터의 타입 변환"
excerpt: "포인터의 타입 변환에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, type casting, type, type conversion, pointer]

permalink: /cpp/type-casting-pointer/

toc: true
toc_sticky: true

date: 2022-12-13 16:30:54+0900
last_modified_at: 2022-12-13 16:30:58+0900
---
 
## 👻 포인터의 타입 변환
이번 시간에는 타입 변환 중 포인터의 타입 변환에 대해 알아보자.

***

### 🌱 연관성이 없는 클래스 사이의 포인터 변환
예제로 알아보기위해 아래처럼 두 클래스를 정의했다.

```c++
class Knight
{
public:
    int _hp = 0;
};

class Item
{
public:
    Item()
    {
        cout << "Item()" << endl;
    }

    Item(const Item& item)
    {
        cout << "Item(const Item&)" << endl;
    }

    ~Item()
    {
        cout << "~Item()" << endl;
    }
public:
    int _itemType = 0;
    int _itemDbId = 0;

    char _dummy[4096] = {}; // 이런 저런 정보들로 인해 비대해진 공간
};
```

그런 다음, 메인 함수에서 다음과 같이 타입을 변환 해보았다고 가정해보자.

```c++
int main()
{
  Knight* knight = new Knight();

  Item* item = (Item*)knight;
  item->_itemType = 2;
  item->_itemDbId = 1;

  delete knight;
}
```

``` knight ```는 힙 영역에 생성된 **Knight 클래스**를 가리키는 주소이고 ``` item ```은 스택 영역에서 **knight의 주소**를 담고 있다.

현재 ``` knight ```는 4바이트이고 ``` item ```은 4000바이트 이상의 크기를 가지게 되는데 위처럼 강제 타입 변환 후 값을 변경시켜버리면 해당 범위를 벗어난 메모리의 값을 변경시키게 되는 위험성이 존재한다.

***

### 🌱 부모, 자식 간의 변환
우선 아래와 같은 코드를 추가로 작성해주었다.

```c++
enum ItemType
{
    IT_WEAPON = 1,
    IT_ARMOR = 2
};

class Weapon : public Item
{
public:
    Weapon() : Item(IT_WEAPON)
    {
        cout << " Weapon()" << endl;
    }

    ~Weapon()
    {
        cout << "~Weapon()" << endl;
    }
public:
    int _damage = 0;
};

class Armor : public Item
{
public:
    Armor() : Item(IT_ARMOR)
    {
        _itemType = 2;
        cout << "Armor()" << endl;
    }

    ~Armor()
    {
        cout << "~Armor()" << endl;
    }
public:
    int _defence = 0;
};
```

``` Weapon ```과 ``` Armor ```는 ``` Item ```을 상속받는다. 이전 시간에 간단하게 알아본 내용과 동일하다. 부모에서 자식으로 변환 시엔 **명시적**으로만 가능하고, 변환은 가능하나 원활하게 연결되지 않아 엉뚱한 메모리를 건들 위험성이 존재한다. 반대로 자식에서 부모로 변환 시엔 암시적으로도 가능하다.

여기서, **명시적으로 타입을 변환할 때에는 항상 조심**해야 한다는 것을 알 수 있었다. 그렇다면 **암시적으로 될 때는 항상 안전할까?** 평생 명시적으로 타입 변환(캐스팅)은 안 하면 되는 거 아닐까?

```c++
Item* inventory[20] = {};

srand((unsigned int)time(nullptr));
for (int i = 0; i < 20; i++)
{
    int randValue = rand() % 2; // 0 ~ 1
    switch (randValue)
    {
    case 0:
        inventory[i] = new Weapon();
        break;
    case 1:
        inventory[i] = new Armor();
        break;
    }
}
```

위와 같은 코드가 있다고 가정했을 때, 암시적으로 ``` 자식 👉 부모 ``` 타입 변환이 적용된 상황이다.

```c++
for (int i = 0; i < 20; i++)
{
    Item* item = inventory[i];
    if (item == nullptr)
        continue;

    if (item->_itemType == IT_WEAPON)
    {
        Weapon* weapon = (Weapon*)item;
    }
}
```

반대로 값을 꺼내와 확인 후 변환한다고 가정한다면 명시적으로 타입 변환을 해줘야 할 것이다. 위의 상황은 ``` 부모 👉 자식 ``` 방향의 타입 변환이지만 ``` Weapon ``` 클래스를 확실시 할 수 있으므로 나름 안전한 변환이라 볼 수 있다.

***

### 🌱 상속 관계에서의 소멸자 virtual
힙 영역에 메모리를 할당받으면 반드시 할당 해제를 시켜야 한다고 했었다. 그리고 위의 코드에서 부모 자식 간의 암시적, 명시적으로 타입 변환이 일어났었다. 할당받은 메모리를 해제시켜보자.

```c++
for (int i = 0; i < 20; i++)
{
    Item* item = inventory[i];
    if (item == nullptr)
        continue;

    delete item;
}
```

위처럼 코드를 작성하면 부모 클래스인 Item의 소멸자가 호출된다. 하지만 Weapon과 Armor 클래스를 생성해주었기 때문에 자식 클래스의 소멸자도 호출되어야 한다. 이전 시간에 멤버 함수를 공부했을 때 ``` virtual ```을 붙여주면 자동으로 자식의 함수를 호출시켜주는 걸 알 수 있었는데, 마찬가지로 상속 관계에 있는 클래스 내에서, 부모 소멸자 앞에 반드시 ``` virtual ```을 붙여주어 자식 클래스의 소멸자도 호출되도록 해주어야 한다.

```c++
virtual ~Item() { ... }
```

virtual을 붙여주지 않으면 위의 for문은 다음과 같이 작성되어야 올바른 동작을 하게된다.

```c++
for (int i = 0; i < 20; i++)
{
    Item* item = inventory[i];
    if (item == nullptr)
        continue;

    if (item->_itemType == IT_WEAPON)
    {
        Weapon* weapon = (Weapon*)item;
        delete weapon;
    }
    else
    {
        Armor* armor = (Armor*)item;
        delete armor;
    }
}
```

> 부모 클래스 내에서만 virtual을 설정해주어도 자식 클래스에 그대로 상속된다.

***

## 👻 글을 마치며
이번 시간에는 일반적인 타입 변환에 이어 포인터의 타입 변환까지 알아보았다. 처음엔 이해를 잘 하고 있는거라 생각했는데 점점 알아볼수록 헷갈리는 것 같다. 아마도 개념 정리가 완벽하게 되지 않아서 그런 것 같다. 내용이 워낙 어렵기도 하고 복잡하다는 생각이 들어서 그런지 확실하게 개념 정리를 할 필요가 있어보인다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/heap/type-casting-pointer)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   