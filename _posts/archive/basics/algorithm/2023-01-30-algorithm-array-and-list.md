---
title: "[Algorithm] 배열, 동적 배열, 연결 리스트"
excerpt: "선형 자료 구조인 배열, 동적 배열, 연결 리스트에 대해 알아보기"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, array, vector, linked list, list]

permalink: /algorithm/array-and-list/

toc: true
toc_sticky: true

date: 2023-01-30 21:25:43+0900
last_modified_at: 2023-01-30 21:25:45+0900
---
 
## 👻 선형 VS 비선형
자료 구조는 크게 **선형(Linear)**과 **비선형(NonLinear)**로 나뉜다. 데이터가 저장되는 모습에 따라 구분되는데 자료들 간의 관계가 일대일이면 선형, 일대다면 비선형이라 볼 수 있다. **배열, 동적 배열, 연결 리스트, 스택, 큐 등**의 자료 구조가 **선형 자료 구조**, **트리, 그래프 등**이 **비선형 자료 구조**에 속한다. 이번 시간에는 선형 자료 구조의 대표격인 배열, 동적 배열, 연결 리스트에 대해 알아볼 것이다.

## 👻 배열
**배열(Array)**는 메모리에서 연속된 공간을 할당 받아 데이터를 저장하는 구조이다. 데이터의 크기를 고정해서 선언하고 연속적인 공간을 사용한다.

![Alt Text](/assets/images/posts_img/basics/algorithm/array-and-list/array.png)   

> - **장점**   
    - 데이터가 연속적으로 존재하기 때문에 탐색이 쉽다.
- **단점**   
    - 크기가 고정되어 있어 추가 및 축소가 불가능하다.

***

## 👻 동적 배열
**동적 배열(Vector)**은 배열의 단점을 보완한 배열로, 말 그대로 메모리의 크기를 유동적으로 할당 받아 데이터를 연속으로 저장할 수 있다. 하지만 데이터가 자주 추가되면 새로 연속된 메모리로 이동해야하는데, 여기서 이사 비용이 발생하게 되니 주의해서 사용해야 한다.

![Alt Text](/assets/images/posts_img/basics/algorithm/array-and-list/vector.png)   

> - **동적 배열 할당 정책**   
    - 실제로 사용할 메모리보다 많이, 여유분을 두고 (대략 1.5~2배) 메모리를 할당하여 이사 횟수를 최소화 시킴
- **장점**   
    - 유동적인 메모리 사용이 가능하다. (동적 배열 할당 정책)
- **단점**   
    - 데이터의 중간 삽입, 삭제가 힘들다.

***

### 🌱 구현 연습
```c++
#include <iostream>
#include <vector>
using namespace std;

// 동적 배열 구현 연습

template<typename T>
class Vector
{
public:
    Vector() {}
    ~Vector()
    {
        if (_data)
            delete[] _data;
    }

    void push_back(const T& value)
    {
        if (_size == _capacity)
        {
            // 증설 작업
            int newCapacity = static_cast<int>(_capacity * 1.5);
            if (newCapacity == _capacity)
                newCapacity++;

            reserve(newCapacity);
        }

        // 데이터 저장
        _data[_size] = value;
        // 데이터 개수 증가
        _size++;
    }

    void reserve(int capacity)
    {
        if (_capacity >= capacity)
            return;

        _capacity = capacity;

        T* newData = new T[_capacity];

        // 데이터 복사
        for (int i = 0; i < _size; i++)
            newData[i] = _data[i];

        if (_data)
            delete[] _data;

        // 교체
        _data = newData;
    }

    T& operator[](const int pos) { return _data[pos]; }

    int size() { return _size; }
    int capacity() { return _capacity; }

    void clear()
    {
        if (_data)
        {
            delete[] _data;
            _data = new T[_capacity];
        }

        _size = 0;
    }
private:
    T*      _data = nullptr;
    int     _size = 0;
    int     _capacity = 0;
};

int main()
{
    Vector<int> v;

    for (int i = 0; i < 100; i++)
    {
        v.push_back(i);
        cout << v[i] << " " << v.size() << " " << v.capacity() << endl;
    }

    v.clear();
    cout << v.size() << " " << v.capacity() << endl;
}
```

***

## 👻 연결 리스트
**연결 리스트(Linked-List)**는 배열과 비슷하나 연속되지 않은 메모리를 사용한다는 차이점이 있다. 수 많은 노드들이 앞뒤 메모리(노드)의 정보를 함께 가지게 되면서 연결이 되어있는 형태라고 볼 수 있다.

![Alt Text](/assets/images/posts_img/basics/algorithm/array-and-list/linked-list.png)   

> - **장점**   
    - 데이터의 중간 삽입, 삭제가 편리하다. (단, 해당 위치를 알고 있을 때에만 적용된다.)
- **단점**
    - N번째 메모리를 바로 찾을 수가 없다. (임의 접근(Random Access)이 불가하다.)

***

### 🌱 구현 연습
```c++
#include <iostream>
#include <list>
using namespace std;

// 연결 리스트 구현 연습

template<typename T>
class Node
{
public:
    Node() : _prev(nullptr), _next(nullptr), _data(T())
    {

    }

    Node(const T& value) : _prev(nullptr), _next(nullptr), _data(value)
    {

    }

public:
    Node*   _prev;
    Node*   _next;
    T       _data;
};

template<typename T>
class Iterator
{
public:
    Iterator() : _node(nullptr)
    {

    }

    Iterator(Node<T>* node) : _node(node)
    {

    }

    ~Iterator()
    {

    }

    // ++it
    Iterator& operator++()
    {
        _node = _node->_next;
        return *this;
    }

    // it++
    Iterator operator++(int)
    {
        Iterator<T> temp = *this;
        _node = _node->_next;
        return temp;
    }

    // --it
    Iterator& operator--()
    {
        _node = _node->_prev;
        return *this;
    }

    // it--
    Iterator& operator--(int)
    {
        Iterator<T> temp = *this;
        _node = _node->_prev;
        return temp;
    }

    // *it
    T& operator*()
    {
        return _node->_data;
    }

    bool operator==(const Iterator& other)
    {
        return _node == other._node;
    }

    bool operator!=(const Iterator& other)
    {
        return _node != other._node;
    }
public:
    Node<T>* _node;
};

template<typename T>
class List
{
public:
    List() : _size(0)
    {
        // [head] <-> [tail]
        _head = new Node<T>();
        _tail = new Node<T>();
        _head->_next = _tail;
        _tail->_prev = _head;
    }

    ~List()
    {
        while (_size > 0)
            pop_back();

        delete _head;
        delete _tail;
    }

    void push_back(const T& value)
    {
        AddNode(_tail, value);
    }

    void pop_back()
    {
        RemoveNode(_tail->_prev);
    }

public:
    using iterator = Iterator<T>;

    iterator begin() { return iterator(_head->_next); }
    iterator end() { return iterator(_tail); }

    // it '앞에' 추가
    iterator insert(iterator it, const T& value)
    {
        Node<T>* node = AddNode(it._node, value);
        return iterator(node);
    }

    // 
    iterator erase(iterator it)
    {
        Node<T>* node = RemoveNode(it._node);
        return iterator(node);
    }

private:
    // [head] <-> [1] <-> [prevNode] <-> [before] <-> [tail]
    // [head] <-> [1] <-> [prevNode] <-> [newNode] <-> [before] <-> [tail]
    Node<T>* AddNode(Node<T>* before, const T& value)
    {
        Node<T>* newNode = new Node<T>(value);
        Node<T>* prevNode = before->_prev;
        // Node<T>* nextNode = before;

        prevNode->_next = newNode;

        newNode->_prev = prevNode;
        newNode->_next = before;

        before->_prev = newNode;

        _size++;

        return newNode;
    }

    Node<T>* RemoveNode(Node<T>* node)
    {
        Node<T>* prevNode = node->_prev;
        Node<T>* nextNode = node->_next;

        prevNode->_next = nextNode;
        nextNode->_prev = prevNode;

        delete node;

        _size--;

        return nextNode;
    }

    int size() { return _size; }

private:
    Node<T>*    _head;
    Node<T>*    _tail;
    int         _size;
};

int main()
{
    List<int> li;

    List<int>::iterator eraseIt;

    for (int i = 0; i < 10; i++)
    {
        if (i == 5)
        {
            eraseIt = li.insert(li.end(), i);
        }
        else
        {
            li.push_back(i);
        }
    }

    li.pop_back();

    li.erase(eraseIt);

    for (List<int>::iterator it = li.begin(); it != li.end(); ++it)
    {
        cout << (*it) << endl;
    }
}
```

***

## 👻 글을 마치며
이번 시간엔 선형 자료 구조인 배열, 동적 배열, 연결 리스트에 대해 개념 복습과 구현 연습을 해보았다. 강의 파트 1때 C++이론을 공부하면서 잠깐 다뤘었던 부분인데 구현을 다시 복습하는 과정에서 기억이 잘 나지 않았었다. 아무래도 개념 이해만 하고 확실히 한 다음 넘어가지 않아서 그랬던 것 같다. 지금 다시 만들면 만들 수는 있으나 시간이 오래 걸릴 것 같은데 바로 만들 수 있도록 연습이 많이 필요할 것 같다.

***

_출처_   
_[인프런 Rookiss님 강의](https://inf.run/1JwV)_