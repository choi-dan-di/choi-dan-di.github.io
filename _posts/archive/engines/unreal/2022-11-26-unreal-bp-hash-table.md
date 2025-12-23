---
title: "[Unreal Engine] BP - 해시 테이블 이론과 맵(Map)"
excerpt: "해시 테이블의 기본 개념과 맵(Map)에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, data structure, hash table, hash]

permalink: /unreal/bp-hash-table/
 
toc: true
toc_sticky: true

date: 2022-11-26 00:00:16+0900
last_modified_at: 2022-11-26 00:00:20+0900
---

## 👻 해시 테이블
**해시 테이블(Hash Table)**이란 많은 양의 데이터를 관리할 때 **조금 더 빠른 탐색**을 하기 위해 만들어진 자료구조의 한 종류이다. **키(Key)**와 **값(Value)**을 한 쌍으로 묶어 관리하기 때문에 일반적인 배열에 비해 훨씬 더 많은 메모리를 차지하지만 키를 이용하면 배열보다 훨씬 빨리 결과값을 찾아낼 수 있다.

***

## 👻 맵(Map) 사용해보기
**맵(Map)**은 해시맵이라고도 하며 **키(Key)와 값(Value)**를 한 쌍으로 데이터를 가진다. 블루프린트에서 배열과 같이 설정할 수 있고 키와 값의 타입을 각각 지정할 수 있다. **키는 고유한 값을 가진다.**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-hash-table/map.PNG)   

해당 맵은 키는 ``` Integer ``` 타입, 값은 ``` String ``` 타입을 가지게 된다.

***

### 🌱 ADD
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-hash-table/add.PNG)   

해시맵에 값을 추가해주는 노드이다. 키와 값을 정해서 넣어줄 수 있다.

***

### 🌱 REMOVE
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-hash-table/remove.PNG)   

해시맵의 요소를 삭제시켜주는 노드이다. 삭제가 성공적으로 완료되면 불리언 값으로 참을 반환한다.

***

### 🌱 FIND
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-hash-table/find.PNG)   

해시맵 내의 요소를 찾아주는 노드이다. 키를 이용해서 값을 찾는다. 값을 찾으면 불리언 값으로 참을 반환한다. 배열보다 효율적이다.

***

### 🌱 CLEAR
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-hash-table/clear.PNG)   

해시맵의 전체 요소를 삭제시켜주는 노드이다.

***

### 🌱 IS EMPTY, IS NOT EMPTY
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-hash-table/empty.PNG)   

해시맵이 비었는지 안 비었는지 확인하는 노드이다. 각 조건에 부합하면 참, 아니면 거짓을 반환한다.

***

### 🌱 LENGTH
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-hash-table/length.PNG)   

해시맵의 길이를 반환하는 노드이다.

***

### 🌱 KEYS, VALUES
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-hash-table/key-value.PNG)   

해시맵의 키나 값들을 배열로 반환해주는 노드이다. ``` For Each Loop ``` 노드로 추출한 값에 접근할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/bp-hash-table/return.PNG)   

***

## 👻 글을 마치며
이번 시간에는 해시 테이블과 해시맵에 대해 간단하게 알아보았다. 배열로 힘들어 할 때쯤 맵을 접하고 얼마나 신세계를 경험했는지 모르겠다. 굉장히 편했던 것으로 기억하는데 블루프린트에서도 사용법이 간단해 쉽게 사용할 수 있을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/data-structure/BP_HashTable)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   