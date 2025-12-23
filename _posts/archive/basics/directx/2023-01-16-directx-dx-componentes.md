---
title: "[DirectX 12] 다양한 컴포넌트(Component)들"
excerpt: "3D 개발을 위해 필요한 컴포넌트(부품)들에 대해 알아보기"

categories:
  - DirectX
tags:
  - [DirectX, dx, dx12, graphics, rendering pipeline, pipeline, input, timer, material, component, scene]

permalink: /directx/dx-components/

toc: true
toc_sticky: true

date: 2023-01-16 18:01:35+0900
last_modified_at: 2023-01-16 19:38:00+0900
---

## 👻 다양한 컴포넌트들
3D 작업엔 필연적으로 다양한 기능이 필요하다. 그 중 **카메라 기능**이 반드시 들어가는데, 해당 기능을 자유자재로 컨트롤하기 위해선 **입력값**이 필요하다. 또한 캐릭터 등이 이동할 때도 시간에 비례한 거리를 이동해야 하기 때문에 **시간을 재주는 기능**도 필요하다. 그 외에도 다양한 컴포넌트들에 대해 알아보자.

### 🌱 Input
입력값에 대해 알맞은 이벤트를 적용해줄 수 있는 함수이다. 입력값을 받는 기능을 이미 정의되어있다. 알맞은 기능을 골라서 사용하면 쉽게 입력값을 받을 수 있다.

- **Input.cpp**

```c++
// 해당 윈도우 창에서만 키 이벤트가 적용되도록 하기 위해 윈도우 핸들을 받음
void Input::Init(HWND hwnd)
{
	_hwnd = hwnd;
	_states.resize(KEY_TYPE_COUNT, KEY_STATE::NONE);
}

// 매 프레임마다 업데이트 호출
// 한 프레임에는 동일한 키 상태가 적용된다고 생각해야함
void Input::Update()
{
	HWND hwnd = ::GetActiveWindow();
	if (_hwnd != hwnd)
	{
		for (uint32 key = 0; key < KEY_TYPE_COUNT; key++)
			_states[key] = KEY_STATE::NONE;

		return;
	}

	BYTE asciiKeys[KEY_TYPE_COUNT] = {};
	if (::GetKeyboardState(asciiKeys) == false)
		return;

	for (uint32 key = 0; key < KEY_TYPE_COUNT; key++)
	{
		// 키가 눌려 있으면 true
		if (asciiKeys[key] & 0x80)
		{
			KEY_STATE& state = _states[key];

			// 이전 프레임에 키를 누른 상태라면 PRESS
			if (state == KEY_STATE::PRESS || state == KEY_STATE::DOWN)
				state = KEY_STATE::PRESS;
			else
				state = KEY_STATE::DOWN;
		}
		else
		{
			KEY_STATE& state = _states[key];

			// 이전 프레임에 키를 누른 상태라면 UP
			if (state == KEY_STATE::PRESS || state == KEY_STATE::DOWN)
				state = KEY_STATE::UP;
			else
				state = KEY_STATE::NONE;
		}
	}
}
```

``` KEY_TYPE ```과 ``` KEY_STATE ```은 ``` enum class ```로 설정된 상수값이고 매 프레임마다 키를 체크하여 상태를 변경하고 ``` _state ```를 설정한다.

- **Game.cpp : 이벤트 적용 부분**

```c++
{
    static Transform t = {};
    if (INPUT->GetButton(KEY_TYPE::W))
        t.offset.y += 1.f * 0.001;
    if (INPUT->GetButton(KEY_TYPE::A))
        t.offset.x -= 1.f * 0.001;
    if (INPUT->GetButton(KEY_TYPE::S))
        t.offset.y -= 1.f * 0.001;
    if (INPUT->GetButton(KEY_TYPE::D))
        t.offset.x += 1.f * 0.001;
}
```

***

### 🌱 Timer
위의 적용 코드에서 ``` * 0.001 ``` 부분이 시간(프레임)에 해당한다. 마찬가지로 함수를 따로 빼서 시간을 계산하여 적용해보자.

- **Timer.cpp**

```c++
#include "pch.h"
#include "Timer.h"

void Timer::Init()
{
	::QueryPerformanceFrequency(reinterpret_cast<LARGE_INTEGER*>(&_frequency));
	::QueryPerformanceCounter(reinterpret_cast<LARGE_INTEGER*>(&_prevCount)); // CPU 클럭
}

void Timer::Update()
{
	uint64 currentCount;
	::QueryPerformanceCounter(reinterpret_cast<LARGE_INTEGER*>(&currentCount));

	// 초(sec) 단위
	_deltaTime = (currentCount - _prevCount) / static_cast<float>(_frequency);
	_prevCount = currentCount;

	_frameCount++;
	_frameTime += _deltaTime;

	if (_frameTime > 1.f)
	{
		_fps = static_cast<uint32>(_frameCount / _frameTime);

		_frameTime = 0.f;
		_frameCount = 0;
	}
}
```

***

### 🌱 Material
매번 메쉬를 만들 때 쉐이더, 텍스처도 일일이 만들면 부하가 커질 수 있다. 텍스처를 포함해 사용할 쉐이더와 인자들을 모아서 하나의 클래스로 관리한 것을 **머테리얼(Material)**이라한다. 머테리얼을 하나 만들어서 메쉬에 적용만 시키면 프로그램의 부담이 훨씬 덜해질 것이다. 쉐이더, 텍스처, 머테리얼 변수들을 변수로 가진다.

- **Material.h**

```c++
#pragma once

class Shader;
class Texture;

enum
{
	MATERIAL_INT_COUNT = 5,
	MATERIAL_FLOAT_COUNT = 5,
	MATERIAL_TEXTURE_COUNT = 5,
};

struct MaterialParams
{
	void SetInt(uint8 index, int32 value) { intParams[index] = value; }
	void SetFloat(uint8 index, float value) { floatParams[index] = value; }

	// 범위 체크를 해주기 때문에 버그 잡기가 용이하다. C스타일 배열 사용은 지양.
	array<int32, MATERIAL_INT_COUNT> intParams;
	array<float, MATERIAL_FLOAT_COUNT> floatParams;
};

class Material
{
public:
	shared_ptr<Shader> GetShader() { return _shader; }

	void SetShader(shared_ptr<Shader> shader) { _shader = shader; }
	void SetInt(uint8 index, int32 value) { _params.SetInt(index, value); }
	void SetFloat(uint8 index, float value) { _params.SetFloat(index, value); }
	void SetTexture(uint8 index, shared_ptr<Texture> texture) { _textures[index] = texture; }

	void Update();

private:
	shared_ptr<Shader>									_shader;
	MaterialParams										_params;
	array<shared_ptr<Texture>, MATERIAL_TEXTURE_COUNT>	_textures;
};
```

- **Material.cpp**

```c++
#include "pch.h"
#include "Material.h"
#include "Engine.h"

void Material::Update()
{
	// CBV 업로드
	CONST_BUFFER(CONSTANT_BUFFER_TYPE::MATERIAL)->PushData(&_params, sizeof(_params));

	// SRV 업로드
	for (size_t i = 0; i < _textures.size(); i++)
	{
		if (_textures[i] == nullptr)
			continue;

		SRV_REGISTER reg = SRV_REGISTER(static_cast<int8>(SRV_REGISTER::t0) + i);
		GEngine->GetTableDescHeap()->SetSRV(_textures[i]->GetCpuHandle(), reg);
	}

	// 파이프라인 세팅
	_shader->Update();
}
```

***

### 🌱 Scene
다양한 **게임 오브젝트(Game Object)**들이 여러개 모여있는 것을 의미한다. 로그인, 로비, 게임 플레이 등이 각각 한 **씬(Scene)**에 해당된다. 여러가지의 게임 장면을 씬 단위로 만들어두고 유저가 상호작용을 하는 씬만 보여주는 방식으로 게임이 진행된다고 볼 수 있다.

> 💡 **Game Object**   
언리얼은 **상속 관계**를 주로 두고 개발이 이루어지고 유니티는 **컴포넌트를 조립하여** 만든다. **게임 오브젝트(Game Object)**는 유니티에서처럼 컴포넌트를 조립하여 만든 오브젝트를 의미한다.

> 💡 **싱글톤 패턴(Singleton Pattern)**   
**객체의 인스턴스가 오직 1개만 생성되는 패턴**을 의미한다.   
- **장점**   
    - **메모리 낭비를 방지**할 수 있다.
    - 이미 생성된 인스턴스를 사용하기 때문에 **속도 측면에서도 이점**이 있다.
    - 다른 클래스 간에 **데이터 공유가 쉽다.**
    - 도메인 관점에서 **인스턴스가 한 개만 존재하는 것을 보증**하고 싶은 경우에도 사용하기도 한다.
- **단점**   
    - 패턴을 구현하는 코드 자체가 많이 필요하다. (동시성 문제)
    - 테스트하기 어렵다.
    - 클라이언트가 구체 클래스에 의존하게 된다.
    - 자식 클래스를 만들 수 없다.
    - 내부 상태를 변경하기 어렵다.
>
> - **EnginePch.h**   
>
> ```c++
> #define DECLARE_SINGLE(type)          \
> private:	                      \
> 	type() {}                     \
> 	~type() {}	              \
> public:	                              \
> 	static type* GetInstance()    \
> 	{                             \
> 		static type instance; \
> 		return &instance;     \
> 	}		              \
> ```
> 
> 여러 줄을 정의하고 싶을 땐 한 줄 끝에 **역슬래시(\)**를 붙인다.
>
> - **SceneManager.h**
>
> ```c++
> class SceneManager
> {
> 	DECLARE_SINGLE(SceneManager);
> };
> ```

***

## 👻 글을 마치며
이번 시간에는 다양한 컴포넌트들에 대해 알아보았다. 부품 조립을 하면 된다고 생각하니 이해가 잘 됐던 것 같다. 그리고 엔진 에디터를 한 번씩 이미 경험해 봤기 때문에 머릿속에 구조가 그려지면서 코드로도 잘 풀어나갈 수 있었던 것 같다. 그래도 뭔가 엔진을 사용할 때보다 1부터 100까지 내가 직접 만들어야 하는 느낌이 강하게 들어서 적응이 쉽진 않은 것 같다. 😅 하다보면 익숙해지겠지..?

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/uEZu)_   
_[싱글톤(Singleton) 패턴이란?](https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/)_