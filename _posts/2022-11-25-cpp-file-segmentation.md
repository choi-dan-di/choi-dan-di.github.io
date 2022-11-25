---
title: "[C++] 파일 분할 관리"
excerpt: "파일을 분할하고 관리하는 방법 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, pointer, practice, snail]

permalink: /cpp/file-segmentation/

toc: true
toc_sticky: true

date: 2022-11-25 22:34:30+0900
last_modified_at: 2022-11-25 22:34:34+0900
---

## 👻 파일 분할 관리
이제껏 공부해오면서 오로지 한 파일에서만 작업을 진행해왔다. 앞으로 프로젝트의 양이 거대해지면 한 파일에 모든 코드를 넣을 수 없다. 많은 양의 코드를 관리하기 위해서는 파일의 분할이 필요하다. 이번 시간에는 기능에 따라서 **파일을 나누고 관리하는 방법**에 대해 알아보자.

***

### 🌱 헤더 파일 만들기
**헤더 파일**을 하나 만들어보자. 왼쪽 솔루션 탐색기에서 ``` 소스 파일 우클릭 👉 추가 👉 새 항목 ```을 선택해서 추가해주거나 단축키 ``` Ctrl + Shift + A ```를 눌러 바로 추가해 줄 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/file-segmentation/new.PNG)   
![Alt Text](/assets/images/posts_img/basics/cpp/file-segmentation/new2.PNG)   

보통은 소스 파일 내부에 모든 파일을 넣고 관리하기 때문에 **헤더 파일 폴더**는 삭제해도 무관하다.

***

### 🌱 cpp 파일 만들기
다음으로 이름이 같은 ``` cpp ``` 파일도 하나 생성하자. 아까와 같은 방법으로 추가해주면 된다.

![Alt Text](/assets/images/posts_img/basics/cpp/file-segmentation/file-list.PNG)   

***

### 🌱 코드 나누기
이제 헤더 파일엔 함수 선언부를 넣어주고 같은 이름으로 만든 소스 파일(cpp)엔 함수 구현부를 추가해주자.

![Alt Text](/assets/images/posts_img/basics/cpp/file-segmentation/code.PNG)   

하지만 이렇게는 사용할 수 없다. 소스 파일에 헤더 파일의 정보를 넘겨 주어야 사용이 가능하다. ``` #include ```를 이용하여 정보를 가져올 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/file-segmentation/test1.PNG)   

이렇게 사용하면 무리없이 빌드할 수 있다. 

메인함수가 있는 본 소스파일에서도 마찬가지이다. 똑같이 헤더 파일을 include 해주면 메인 함수 내에서 다른 파일에 구현해 둔 함수를 불러다 사용할 수 있게 된다.

![Alt Text](/assets/images/posts_img/basics/cpp/file-segmentation/main.PNG)   

> 💡 **중복 정의를 방지해주는 기능**   
> - ``` #pragma once ```   
동일한 헤더 파일을 중복으로 include 했을 때 중복 정의를 방지해주는 기능을 한다.
> 
> - ``` #ifndef ~ #define ~ #endif ```   
만약 해당 이름이 정의되지 않았으면 정의를 해주는 기능을 한다.   
>
> ```c++
> #ifndef _TEST1_H__
> #define _TEST1_H__
> ...
> #endif
> ```

다른 파일을 include 하게되면 코드를 그대로 복사해서 붙여넣는 방식으로 가져와 코드를 분석하게 된다. 그러니 중복 정의를 항상 유의하며 파일을 분할해야한다.

***

## 👻 글을 마치며
이번 시간에는 드디어 파일을 분할하고 관리하는 방법에 대해 알아보았다. 파일 경로에 약해서 약간은 걱정이지만 정리를 잘 할 수 있다는 자신감을 가지고 꾸준히 연습해야겠다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/file-sementation)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   