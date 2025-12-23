---
title: "[C++] set, multimap, multiset"
excerpt: "STL의 컨테이너 중 하나인 셋과 멀티맵, 멀티셋에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, stl, container, set, multimap, multiset, multi]

permalink: /cpp/set-multimap-multiset/

toc: true
toc_sticky: true

date: 2022-12-18 00:59:39+0900
last_modified_at: 2022-12-18 00:59:42+0900
---

## 👻 Set
**셋(Set)**은 맵에서 **키와 데이터가 동일**하다는 점 빼고는 동일하다.

```c++
#include <set>

set<int> s;
```

사용법은 다른 STL 문법과 동일하며, ``` set ``` 라이브러리를 사용한다.

***

### 🌱 문법
- **삽입**   
``` insert ```를 사용한다.

```c++
s.insert(10);
s.insert(30);
s.insert(20);
s.insert(50);
s.insert(40);
s.insert(70);
s.insert(90);
s.insert(80);
s.insert(100);
s.insert(60);
s.insert(60);
```

- **삭제**   
``` erase ```를 사용한다.

```c++
s.erase(40);
```

- **조회**   
``` find ```를 사용한다. 맵과 동일하게 이터레이터를 반환하고 찾지 못하면 ``` end() ``` 값을 반환한다.

```c++
set<int>::iterator findIt = s.find(50);
if (findIt == s.end()) cout << "못 찾음" << endl;
else cout << "찾음" << endl;
```

- **순회**   
``` * ``` 연산자를 이용하여 값에 접근한다.

```c++
for (set<int>::iterator it = s.begin(); it != s.end(); ++it)
    cout << (*it) << endl;
```

자동 오름차순으로 정렬되며 중복된 값이 사라진다.

> ``` s[10] = 10; ``` 과 같은 접근은 불가능하다. (``` Key == Value ```기 때문이다.)

***

## 👻 Multimap
키 값이 중복 허용되는 맵이다. 추가적으로 키 값과 일치하는 **처음 위치**와 **마지막 다음 위치**를 ``` Pair ``` 타입으로 반환해주는 함수 ``` equal_range ```가 있다. 각각 따로도 반환해주는 함수 ``` lower_bound ```와 ``` upper_bound ```도 존재한다. 나머지는 일반 맵의 사용 방법과 동일하다.

```c++
// 선언 방법
multimap<int, int> mm;

// 조회
pair<multimap<int, int>::iterator, multimap<int, int>::iterator> itPair;
itPair = mm.equal_range(2);

// begin, end와 비슷한 느낌
multimap<int, int>::iterator itBegin = mm.lower_bound(1);  // 첫 주소 반환
multimap<int, int>::iterator itEnd = mm.upper_bound(1);  // 마지막 주소의 다음 주소 반환

for (multimap<int, int>::iterator it = itPair.first; it != itPair.second; ++it)
    cout << it->first << " " << it->second << endl;
```

***

## 👻 Multiset
데이터가 중복 허용되는 셋이다. 멀티맵과 마찬가지로 ``` equal_range ```, ``` lower_bound ```, ``` upper_bound ``` 함수가 추가로 존재한다. 나머지는 일반 셋의 사용 방법과 동일하다.

```c++
// 선언 방법
multiset<int> ms;

// 조회
pair<multiset<int>::iterator, multiset<int>::iterator> itPair2;
itPair2 = ms.equal_range(100);

// begin, end와 비슷한 느낌
multiset<int>::iterator itBegin2 = ms.lower_bound(100);  // 첫 주소 반환
multiset<int>::iterator itEnd2 = ms.upper_bound(100);  // 마지막 주소의 다음 주소 반환
```

***

## 👻 글을 마치며
이번 시간에는 셋과 멀티맵, 멀티셋에 대해 알아보았다. 헷갈리던 부분들이 정리되었다. 더불어 사용 방법도 자연스레 정리되었다. auto를 사용하던 대신에 직접 타입을 입력해보면서 여러번 코드를 작성하니 STL 문법들이 꽤나 익숙해진 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/STL/set-multimap-multiset)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   