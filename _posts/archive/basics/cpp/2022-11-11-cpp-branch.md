---
title: "[C++] 분기문"
excerpt: "조건에 따라 다양한 분기로 나뉘는 분기문에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, switch, case, if, branch]

permalink: /cpp/branch/

toc: true
toc_sticky: true

date: 2022-11-11 15:06:34+0900
last_modified_at: 2022-11-11 15:06:36+0900
---

## 👻 분기문
조건에 따라 다양한 분기로 나눠지는 구문을 뜻한다. 예/아니오를 판별하거나 일치/불일치를 판별하는 조건문이 있다. 보통 ``` if ```, ``` switch-case ``` 문을 사용한다. 어셈블리에서는 **CMP, JMP**와 동일한 의미이다.

***

### 🌱 if문
```c++
// 실행될 코드가 한 줄일 때
if (조건문)
    조건이 참일 경우 실행될 코드
else
    조건이 거짓일 경우 실행될 코드

// 실행될 코드가 여러 줄일 때
if (조건문)
{
    조건이 참일 경우 실행될 코드
}
else if (조건문2)
{
    조건2가 참일 경우 실행될 코드
}
else 
{
    조건이 모두 거짓일 경우 실행될 코드
}
```

***

### 🌱 swtich-case문
if문과 동일하게 작동하나 반복적인 코드가 많을 때 주로 사용한다. 조건에 해당하면 case 문 안의 코드가 실행되고 ``` break; ```를 만나면 빠져나오게 되는데, 만약 적어두지 않았다면 ``` break; ```를 만날 때까지 아래쪽으로 쭉 실행된다.   

```c++
// 조건문엔 정수 계열만 넣을 수 있다.
switch (조건문)
{
    case 조건1:
        break;
    case 조건2:
        break; 
    case 조건3:
        break;
    default:
        // 모든 조건에 해당하지 않을 때
        break;  // 걸어도 되고 안 걸어도 됨
}
```

***

## 👻 글을 마치며
이번 시간엔 분기문을 공부해보았다. switch-case 문의 개념이 살짝 헷갈렸었는데 복습하면서 정리가 확실하게 된 것 같다. break의 중요성도 알게되었다. 확실히 어셈블리로 보면서 공부하니 어떤 코드가 더 효율적인지도 알 수 있어서 좋은 것 같다. :)

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/flow-control/branch)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   