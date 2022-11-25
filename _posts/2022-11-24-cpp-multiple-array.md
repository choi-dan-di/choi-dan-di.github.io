---
title: "[C++] 다차원 배열"
excerpt: "다차원 배열의 개념에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, pointer, multiple array, array]

permalink: /cpp/multiple-array/

toc: true
toc_sticky: true

date: 2022-11-24 15:11:42+0900
last_modified_at: 2022-11-24 15:11:43+0900
---

## 👻 다차원 배열
**다차원 배열**은 **일차원 배열을 중첩으로 사용**한 것이다. 다중 포인터와 같이 배열을 연결시켜서 사용하면 이중, 삼중으로도 선언이 가능하다.

```c++
// int first[5] = { 4, 2, 3, 4, 1 };
// int second[5] = { 1, 1, 5, 2, 2 };
int apartment2D[2][5] = { { 4, 2, 3, 4, 1 }, { 1, 1, 5, 2, 2 } };

cout << apartment2D[1][4] << endl;   // second의 index 4 : 2
cout << "---------------------\n";
for (int floor = 0; floor < 2; floor++) {
	for (int room = 0; room < 5; room++) {
		int num = apartment2D[floor][room];
		cout << num << " ";
	}
	cout << endl;
}
```

위의 코드는 **이차원 배열**을 나타내고 있고 왼쪽부터 오른쪽 순으로 배열의 깊이를 나타낸다. 가장 오른쪽에 있는 배열이 가장 깊은 곳에 위치한다. ``` apartment2D ```의 주소를 이용하여 메모리를 확인해보면 모두 연결되어 있는 것을 알 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/pointer/multiple-array/memory.PNG)   

```c++
int apartment1D[10] = { 4, 2, 3, 4, 1, 1, 1, 5, 2, 2 };
for (int floor = 0; floor < 2; floor++) {
	for (int room = 0; room < 5; room++) {
		int index = (floor * 5) + room;
		// apartment1D + index * 4를 한 주소
		// index = (floor * 20) + room;
		int num = apartment1D[index];
		cout << num << " ";
	}
	cout << endl;
}
```

위에서 봤던 2차원 배열은 1차원 배열로 표현해도 메모리 자체는 동일하게 할당되며 성능적으로도 동일하단 것을 알 수 있다.

***

### 🌱 2차원 배열의 사용
대표적으로 _2D 로그라이크 맵_ 등 **여러가지 다양한 게임의 맵(Map) 정보**를 나타내는 데 사용된다.

```c++
int map[5][5] = {
	{ 1, 1, 1, 1, 1 },
	{ 1, 0, 0, 1, 1 },
	{ 0, 0, 0, 0, 1 },
	{ 1, 0, 0, 0, 0 },
	{ 1, 1, 1, 1, 1 }
};

for (int y = 0; y < 5; y++) {
	for (int x = 0; x < 5; x++) {
		int info = map[y][x];
		cout << info;
	}
	cout << endl;
}
```

***

## 👻 글을 마치며
이번 시간에는 다차원 배열에 대해 알아보았다. 이미 알고 있었던 문법이라 간단하게 복습하는 느낌으로 정리했다. 그저께 알고리즘 공부하면서 정신 나갈 것 같았는데 지금 보니까 이차원 배열이 아름다워 보인다. ~~알고리즘에선 악마야..~~ 얼른 실제 프로젝트에 어떻게 적용되는지 알아보고 싶어진다. 😋

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/pointer/multiple-array)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   