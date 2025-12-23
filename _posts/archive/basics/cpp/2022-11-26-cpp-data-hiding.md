---
title: "[C++] 은닉성(Data Hiding)"
excerpt: "객체 지향 프로그래밍의 특징 중 하나인 은닉성에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, oop, data hiding, hiding, encapsulation]

permalink: /cpp/data-hiding/

toc: true
toc_sticky: true

date: 2022-11-26 18:38:47+0900
last_modified_at: 2022-11-26 18:38:51+0900
---
 
## 👻 은닉성
**은닉성(Data Hiding)**이란 **몰라도 되는 것은 깔끔하게 숨기겠다**는 의미이다. **캡슐화(Encapsulation)**라고도 한다. (엄연히 말하자면 다른 의미를 가지지만 대부분은 비슷한 의미로 사용한다.)

> **왜 숨길까?**   
- 정말 위험하고 건드리면 안 되는 경우
- 다른 경로로 접근하길 원하는 경우

**접근 지정자**를 사용하여 해당 클래스 내의 속성에 대한 접근 범위를 지정할 수 있고 범위에 따라 **캡슐화**가 가능해진다.

***

### 🌱 건드리면 안 되는 경우

> 자동차를 설계해보자.

자동차는 여러가지 부품이 조립되어 만들어져있다. 일반 구매자 입장에서 사용하는 것은 **핸들, 페달, 문 등**이 있고 몰라도 되는 것들은 **엔진, 각종 전기선 등**을 이야기할 수 있다. 몰라도 되는 것들은 오히려 건드리면 큰일나는 것들이기 때문에 숨기는 것이 더욱 안전할 것이다.

```c++
class Car
{
public:
    void MoveHandle() {}
    void PushPedal() {}
    void OpenDoor() {}

    // 건드리면 안 되는 것들
    void DisassembleCar() {}    // 차를 분해한다.
    void RunEngine() {} // 엔진을 구동시킨다.
    void ConnectCircuit() {}    // 전기선을 연걸시킨다.

public:
    // 핸들
    // 페달
    // 엔진
    // 문
    // 각종 전기선
};
```

이렇게 그대로 만들게 되면 건드리면 안 되는 것들에 접근이 쉽게 가능해진다. 이러한 기능들을 접근 불가능하도록 막기 위해(특정한 경로로만 접근할 수 있도록 하기 위해) **접근 지정자**를 설정해줘야 한다.

***

#### 🪐 접근 지정자
우리가 클래스 내에서 계속 사용해왔던 ``` public ```이 바로 **(멤버) 접근 지정자**이다.

> - **public** : 전체 공개
- **protected** : 자식 클래스에서만 사용 가능
- **private** : 클래스 내부에서만 사용 가능

**public**에서 **private**으로 갈수록 접근 범위가 좁아지고, 이러한 접근 지정자를 사용하여 함수의 접근 범위를 지정할 수 있다.

```c++
class Car
{
public:
    void MoveHandle() {}
    void PushPedal() {}
    void OpenDoor() {}

private:
    // 건드리면 안 되는 것들
    void DisassembleCar() {}    // 차를 분해한다.
    void RunEngine() {} // 엔진을 구동시킨다.
    void ConnectCircuit() {}    // 전기선을 연걸시킨다.

public:
    // 핸들
    // 페달
    // 엔진
    // 문
    // 각종 전기선
};
```

``` private ```으로 지정해 준 함수들은 외부에서 호출할 수 없다.

중간 단계인 ``` protected ```는 외부에선 사용할 수 없지만 상속을 받았을 경우 자식 클래스 내에서 사용이 가능한 정도의 접근 범위를 가진다.

```c++
class Car
{
public:
    void MoveHandle() {}
    void PushPedal() {}
    void OpenDoor() {}

protected:
    // 건드리면 안 되는 것들
    void DisassembleCar() {}    // 차를 분해한다.
    void RunEngine() {} // 엔진을 구동시킨다.
    void ConnectCircuit() {}    // 전기선을 연걸시킨다.

public:
    // 핸들
    // 페달
    // 엔진
    // 문
    // 각종 전기선
};

class SuperCar : public Car     // 상속 접근 지정자
{
public:
    void PushRemoteController()
    {
        // private은 사용불가. protected는 사용 가능
        RunEngine();
    }
}
```

``` protected ```로 접근 지정자를 설정하면 해당 클래스를 상속받은 자식 클래스 내부에서까지 사용가능하고 외부에선 여전히 호출 불가능하다.

***

### 🌱 다른 경로로 접근하길 원하는 경우

> 버서커의 특징을 설계해보자.

RPG 게임 중, 버서커라는 직업 하나가 있다. 해당 직업은 체력이 특정 수 이하로 떨어지게 되면 능력치가 더 높아진다거나 모드를 발동시키는 조건을 가지고 있다. 해당 기능을 구현해보며 은닉성의 필요성에 대해 조금 더 알아보자.

우선 기본 체력은 100으로 세팅되어 있고 체력이 50 이하로 떨어지면 버서커 모드가 발동된다고 가정해보자.

```c++
class Berserker
{
public:
    // 사양 : 체력이 50 이하로 떨어지면 버서커 모드 발동 (강해짐)

    void SetBerserkerMode()
    {
        cout << "매우 강해짐!" << endl;
    }

public:
    int _hp = 100;
};

int main()
{
    Berserker b;

    b._hp = 20;
    if (b._hp < 50)
        b.SetBerserkerMode();
}
```

버서커 모드 발동 조건의 로직은 이렇다. ``` _hp ```가 변경되는 순간마다 체크를 해서 버서커 모드의 발동여부를 정해줘야하는데, 혹시나 나중에 체크하는 부분이 사라지거나 다르게 수정되면 로직이 제대로 동작되지 않게된다.

결국, 문제는 ``` _hp ```에 직접 접근을 하게 되면서 발생하게 되는데, 이러한 문제를 해결하기 위해 **캡슐화**를 진행한다.

***

#### 🪐 캡슐화
**캡슐화(Encapsulation)**란, **연관된 데이터와 함수를 논리적으로 묶어놓은 것을 의미**한다. 위의 예시에서 캡슐화를 진행하게되면 ``` _hp ```를 건드리는 순간, ``` _hp ```의 값을 체크하는 부분까지 한 데 묶어진 상태로 만들 수 있게된다.

```c++
class Berserker
{
public:
    int GetHp() { return _hp; }

    void SetHp(int hp)
    {
        _hp = hp;
        if (_hp <= 50)
            SetBerserkerMode();
    }

    // 사양 : 체력이 50 이하로 떨어지면 버서커 모드 발동 (강해짐)
private:
    void SetBerserkerMode()
    {
        cout << "매우 강해짐!" << endl;
    }

private:
    int _hp = 100;
};
```

``` _hp ``` 와 버서커 모드를 ``` private ```으로 숨긴다. 그런 다음 ``` _hp ```의 값이 변경될 때마다 체크해주는 부분을 추가하여 묶어주었다. 이렇게 되면 외부에선 ``` _hp ```의 값을 가져오거나 설정까지만 하게되고 나머지 부분은 내부에서 자동으로 해주기 때문에 오류가 날 가능성이 적어지게된다. 조금 더 안정화된 작업이라 볼 수 있다.

***

### 🌱 상속 접근 지정자
이전에 봤던 접근 지정자는 클래스 내에서 사용한 **멤버 접근 지정자**이다. **상속 접근 지정자**는 상속 받을 부모 클래스의 앞에 붙이게 되는 접근 지정자를 의미한다. 일반 접근 지정자와 같이 ``` public ```, ``` protected ```, ``` private ``` 세 종류가 있다.

내가 상속 받은 부모 클래스를 **나의 자식 클래스**에게 어떤 방식으로 상속할지에 대한 기준을 정해준다.

> - **public** : 공개적인 상속
    - 부모님의 유산 설계 그대로 상속된다.
    - public 👉 public
    - protected 👉 protected
- **protected** : 보호받은 상속
    - **자손** 클래스에게만 상속된다. 외부 접근이 불가능하다.
    - public 👉 protected
    - protected 👉 protected
- **private** : 개인적인 상속
    - **자손** 클래스에게도 물려주지 않는다. 상속받은 나까지만 사용할 수 있다.
    - public 👉 private
    - protected 👉 private

다음은 ``` private ``` 상속을 사용한 경우이다.

```c++
class SuperCar : private Car
{
public:
    void PushRemoteController()
    {
        // 나 까지만 사용하고 막아버릴 것임.
        RunEngine();
    }
};

class TestSuperCar : public SuperCar
{
public:
    void PushRemoteController()
    {
        // 사용 불가능
        RunEngine();
    }
};
```

이렇게 되면 ``` SuperCar ```에서는 ``` RunEngine ```을 사용할 수 있지만 결국 나 까지만 사용하게 되고, 나를 상속받은 나의 자식 클래스, 즉 자손 클래스에서는 ``` public ```으로 나를 상속받았다해도 ``` RunEngine ```을 사용할 수 없다.

***

## 👻 글을 마치며
이번 시간에는 객체 지향 프로그래밍의 특징 중 은닉성에 대해 알아보았다. 접근 사용자를 지정하는 게 중요한 것 같은데 상당히 헷갈리는 부분인 것 같다. 특히 상속 접근 지정자는 상속 받는 관계까지 포함되어 있다보니 조금 어려운 것 같다. 제대로 이해한 게 맞는지 연습을 좀 더 해보면서 확실히 익혀야 할 것 같다. 😂

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/oop/data-hiding)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   