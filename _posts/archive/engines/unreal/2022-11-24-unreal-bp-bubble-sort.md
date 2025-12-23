---
title: "[Unreal Engine] BP - 연습 문제 : 버블 정렬"
excerpt: "블루프린터로 정렬하는 방법 중 버블 정렬 구현해보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, data structure, array, practice, bubble sort, sort, bubble]

permalink: /unreal/bp-bubble-sort/

toc: true
toc_sticky: true

date: 2022-11-24 19:44:22+0900
last_modified_at: 2022-11-24 19:44:27+0900
---

## 👻 버블 정렬
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/bubble-sort.png)   

**버블 정렬(Bubble Sort)**은 자료구조의 정렬 방법 중 하나이다. 주로 배열을 정렬하고 **두 수를 비교**하여 순서를 정리해주는 알고리즘이다. ~~자세한 내용은 다음 알고리즘 공부 시간에..~~

***

### 🌱 버블 정렬 구현
- **한 턴 만들기**   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/bp.PNG)   

우선은 한 턴을 먼저 만들었다. ``` 인덱스 0, 1번 비교 👉 1, 2번 비교 👉 ... ``` 이 순서를 먼저 구현했다.

``` For Loop ``` 노드로 **[배열의 길이 - 2]**만큼 범위를 지정해주었다. 현재 위치와 다음 위치를 비교하려면 배열의 길이에서 2를 빼야 **[마지막 인덱스 - 1]**이 되기 때문이다.

두 수를 비교해서 앞의 수가 더 크면 두 수를 Swap 해주었다. ``` Swap ``` 노드는 배열 내에서 해당 인덱스에 위치하는 값 두 개를 바꿔주는 기능을 한다.

이렇게 되면 비교 및 Swap 한 턴이 끝나게 된다.

> ``` For Loop ``` 노드의 마지막 인덱스는 포함되니 항상 주의해야한다.

- **반복**   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/bp2.PNG)   

범위는 위에서 말했듯 **[배열의 길이 - 2]**로 설정해주었고, 외부의 ``` For Loop ```은 배열의 총 길이만큼 반복하는 것이다. 맨 위에서 버블 정렬에 대해 정리할 때 있었던 이미지를 참고하면 첫 번째 반복, 두 번째 반복, ...을 담당하는 부분이다. 줄은 한 턴이다.

***

### 🌱 결과
단계를 나누고 천천히 생각해보니 금방 구현할 수 있었다. 이제 디버깅을 통해 올바르게 버블 정렬 방법으로 정렬이 되었는지 확인해보자.

- **시작**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/debug.PNG)   

맨 처음 설정했던 ``` Numbers ``` 배열은 ``` { 5, 1, 9, 7, 2, 3 } ```으로 설정되어있다.

- **한 턴이 끝난 후**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/debug2.PNG)   

한 턴 내에서도 첫 Swap이 끝난 후 ``` Numbers ``` 배열의 정렬 상황이다. (Swap이 안 될 수도 있다.)

**5**와 **1**이 Swap되면서 ``` { 1, 5, 9, 7, 2, 3 } ```이 된 것을 확인할 수 있다.

- **세 번째 턴**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/debug3.PNG)   

두 번째 Swap이 끝난 후 배열의 정렬 상황이다. 두 번째 턴에서 5와 9가 대상이지만 조건에 부합하지 않아 다음 턴(세 번째 턴)으로 넘어가 **9**와 **7**이 대상이 되고 두 수가 바뀐 것이다.

- **첫 번째 반복이 끝난 후**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/debug4.PNG)   

~~내가 브레이크 포인트를 잘 못 걸었나..?~~

약간 스킵이 되었는데, 첫 번째 반복이 끝난 후 ``` Numbers ``` 배열의 정렬 상황이다.

첫 번째 반복이 끝나고나면 배열 중에서 가장 큰 수가 맨 뒤로 정렬되게 된다.

> 1. **9**와 **2** 비교 및 Swap : ``` { ..., 2, 9, 3 } ```
2. **9**와 **3** 비교 및 Swap : ``` { ..., 2, 3, 9 } ```
3. 결과 : ``` { 1, 5, 7, 2, 3, 9 } ```

- **두 번째 반복 시작**   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/debug5.PNG)   

마지막 결과에서 다시 처음으로 돌아가 원래는 **1**과 **5**부터 비교하기 시작한다. 이미 정렬이 되어있으니 바로 넘어간 것처럼 보이지만 비교는 처음부터 다시 하게된다.

**7**과 **2**가 비교되어 Swap되었다.

- **한 턴이 끝난 후**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/debug6.PNG)   

``` { 1, 5, 2, 7, 3, 9 } ```에서 **7**과 **3**이 비교되며 Swap 되었다.

가장 큰 수와 두 번째로 큰 수인 7과 9가 맨 뒤에 잘 정렬되어있다.

- **세 번째 반복 시작**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/debug7.PNG)   

**5**와 **2**가 Swap되며 위 이미지처럼 정렬되었다.

- **마지막 턴이 끝난 후**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/debug8.PNG)   

마지막으로 **5**와 **3**이 Swap되며 두 수를 바꾸는 기능은 더 이상 실행되지 않는다.

***

### 🌱 코드 수정
포스팅을 하면서 아까 만들어두었던 코드를 정리하는데 생각해보니 반복이 끝나게되면 가장 마지막 값은 비교 대상이 안 된다는 것이 기억났다. 그래서 추가적으로 수정을 해보았다.

위에서 진행한 단계로 한다면 정렬이 되어도 배열의 마지막 인덱스까지 계속 비교를 하게 된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/fixed.PNG)   

위처럼 안쪽 ``` For Loop ```의 ``` Last Index ```를 1씩 줄이는 노드를 하나 추가해주었다. 그리고 디버깅을 실행시킨 후 한 반복씩 ``` Last Index ```를 확인해보았다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/index.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/index2.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/index3.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-bubble-sort/index4.PNG)   

인덱스가 잘 줄어드는 것을 확인할 수 있었다. ~~아까보단 좀 더 빨라졌겠지..?~~

***

## 👻 글을 마치며
이번 시간에는 버블 정렬을 블루프린트로 구현해보았다. 처음에 막막해서 손이 선뜻 움직이질 않았는데 차근차근 한 단계씩 생각해보니까 결국 만들 수 있었던 것 같다. 그 동안 알고리즘 좀 많이 공부해뒀어야 하는데.. 항상 발등에 불 떨어지고 시작한다. 😭 그래도 어제보다 오늘의 내가 더 발전했으니 그걸로 만족한다! 내일은 오늘의 나보다 더 좋은 코드를 구현하기를!!!

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/data-structure/practice/BP_BubbleSort)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   