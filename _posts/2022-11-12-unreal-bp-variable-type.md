---
title: "[Unreal Engine] BP - 변수 타입"
excerpt: "블루프린트의 변수 타입에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, variable]

permalink: /unreal/bp-variable-type/

toc: true
toc_sticky: true

date: 2022-11-12 19:30:47+0900
last_modified_at: 2022-11-12 19:30:49+0900
---

## 👻 변수 생성하기
변수는 유저와 컴퓨터 사이의 약속이다. 데이터를 담고, 가공하기 위해 필요한 부분이다.

블루프린트 화면에서 좌측의 ``` VARIABLES ```에서 **+**버튼을 눌러 변수를 추가할 수 있다.   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-variable-type/create-variable.PNG)   

변수는 이름과 타입을 지정해줘야하고, ``` F2 ```로 간단하게 변수의 이름을 변경할 수 있다. 변수 이름을 지정할 땐 따로 규칙이 정해져있지 않아 큰 이상만 없으면 대체로 잘 만들어진다.   
컴파일과 세이브를 해야 사용할 수 있다.

> 보통 언리얼에선 대부분 변수 이름을 **대문자로 시작**하는 단어로 짓고 ``` boolean ``` 타입 같은 경우 **소문자 b**를 앞에 붙인다.

***

## 👻 변수 타입
블루프린트의 변수 타입에 대해 알아보자. ``` C++ ```에서 보았듯 크게 다르지 않다.

- ``` Boolean ``` : 참/거짓 두 가지 상태만 가짐
- ``` Byte ``` : 정수. 가장 크기가 작음(0~255)
- ``` Integer ``` : 정수. 일반적인 정수(-21억~21억)
- ``` Integer64 ``` : 정수. 가장 크기가 큼
- ``` Float ``` : 실수. 정밀도가 더 우월함(double-precision)
- ``` Name ``` : 문자열. 엔진 내부에서 사용, 불변인 경우
- ``` String ``` : 문자열. 일반적인 문자열 (ex. 플레이어명)
- ``` Text ``` : 문자열. 퀘스트 설명같이 다국어 변환이 필요한 경우

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-variable-type/variables.PNG)   

위 캡쳐의 변수명들은 대표적으로 사용되는 예시를 든 것들이다.

***

## 👻 글을 마치며
이번시간엔 블루프린트 변수 타입에 대해 알아봤다. 비슷한 것도 많고 처음 본 타입도 있었다. 각 데이터에 따라 사용될 변수 타입이 다르니 변수를 생성할 때 어떤 용도로 쓰일지 정확히 파악한 후 알맞은 변수 타입을 생성해야할 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   