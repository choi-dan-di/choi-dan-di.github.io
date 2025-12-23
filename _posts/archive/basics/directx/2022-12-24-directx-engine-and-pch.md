---
title: "[DirectX 12] Engine 파일과 Pch 파일"
excerpt: "클라이언트단과 서버단으로 파일을 분리하고 Pch 파일에 대해 알아보기"

categories:
  - DirectX
tags:
  - [DirectX, dx, dx12, graphics, rendering pipeline, pipeline, summary]

permalink: /directx/engine-and-pch/

toc: true
toc_sticky: true

date: 2022-12-24 16:03:43+0900
last_modified_at: 2022-12-24 16:03:49+0900
---

## 👻 Windows 데스크톱 애플리케이션
DirectX를 사용하기 전 Windows 데스크톱 애플리케이션으로 클라이언트 단을 생성해보았다. 비주얼 스튜디오에서 **Windows 데스크톱 애플리케이션**을 선택 후 프로젝트를 생성하면 쉽게 생성할 수 있다.

**Windows 데스크톱 애플리케이션**이란, Windows 데스크톱 환경에서 돌아가는 애플리케이션으로, 해당 운영체제에서 돌아가는 모든 소프트웨어를 의미한다. 우리가 일상생활에서 흔히 사용하는 대부분의 데스크톱 기능이 이에 해당하는 것이다. 클라이언트단이라 생각할 수 있으며 DirectX를 사용하여 구현한 기능을 애플리케이션 형식으로 출력해 볼 것이다.

***

## 👻 Client와 Server
보통의 게임 개발에는 **클라이언트(Client)**와 **서버(Server)**로 나뉜다. 클라이언트는 네트워크로 연결된 서버로부터 정보를 제공받는 컴퓨터라는 뜻이며 그러한 컴퓨터를 사용하는 사용자까지 포괄하여 의미를 담고 있다. 서버는 사용자에게 보이지 않는 곳에 존재하며 클라이언트에게 네트워크를 통해 정보나 서비스를 제공하는 컴퓨터 시스템으로 컴퓨터 프로그램 또는 장치를 의미한다. **엔진(Engine)**은 서버단을 뜻한다. 이 두 프로젝트를 따로 분리하여 관리하면서 서로간의 연동을 통해 하나의 솔루션 파일을 만들어 볼 것이다.

***

## 👻 Engine
DirectX를 공부하기 전에 클라이언트단과 서버단으로 나누어서 코드를 관리할 것이고, 그 중 서버단에 포함되는 엔진에 대해 먼저 알아보자. 우선 DirectX12는 11과 다르게 초기화 하는 부분을 직접 설정해주어야한다. 그 부분을 천천히 진행해보자.

우선 엔진에는 프로그램의 주요 정보들이 포함된다. **각 사용자의 환경에 맞춰 그려질 화면 크기와 관련된 정보나 서로 간의 통신을 위한 스마트 포인터, 프로그램이 새로 실행될 때마다 초기화 정보 등**이 대표적이다.

- **Engine.h**

```c++
#pragma once

class Engine
{
public:

	void Init(const WindowInfo& info);
	void Update();

public:
	void Render();
	void RenderBegin();
	void RenderEnd();

	void ResizeWindow(int32 width, int32 height);

private:
	WindowInfo		_window;
	D3D12_VIEWPORT	_viewport = {};
	D3D12_RECT		_scissorRect = {};
};
```

헤더 파일에는 대부분 함수의 선언부가 멤버 함수로 포함되어 있다. 구현부는 cpp 파일에 작성해 줄 것이다.

프로그램이 실행될 때마다 초기화해주는 ``` Init() ```, 매 프레임마다 실행될 ``` Update() ```, 그리고 화면에 렌더링해주는 함수들이 포함되어있다. 또, 현재 프로그램을 실행한 컴퓨터의 정보를 받아 창을 자유롭게 조절할 수 있는 함수를 추가해주었다.

- **Engine.cpp**

```c++
#include "Engine.h"

void Engine::Init(const WindowInfo& info)
{
	_window = info;

	_viewport = { 0, 0, static_cast<FLOAT>(info.width), static_cast<FLOAT>(info.height), 0.0f, 1.0f };
	_scissorRect = CD3DX12_RECT(0, 0, info.width, info.height);

	ResizeWindow(info.width, info.height);
}

void Engine::Update()
{
	Render();
}

void Engine::Render()
{
	RenderBegin();

    // TODO

	RenderEnd();
}

void Engine::RenderBegin()
{
	
}

void Engine::RenderEnd()
{
	
}

void Engine::ResizeWindow(int32 width, int32 height)
{
	_window.width = width;
	_window.height = height;

	RECT rect = { 0, 0, width, height };
	::AdjustWindowRect(&rect, WS_OVERLAPPEDWINDOW, false);
	::SetWindowPos(_window.hwnd, 0, 100, 100, width, height, 0);

	_depthStencilBuffer->Init(_window);
}
```

DirectX에는 많은 함수들이 등장하는데 일일이 외울 필요는 없고 대략적으로 어떠한 기능을 가지는지 정도만 알아두고 넘어갈 것이다. 업데이트 함수에는 렌더 함수 하나만 존재한다는 것을 알 수 있다.

***

## 👻 Pch 파일
**Pch**란 **Pre-Compiled Header**의 약자로써 미리 컴파일된 헤더파일이라는 뜻이다. 프로그램이 실행될 때마다 컴파일, 빌드, 실행이 이루어지는데 **거의 바뀌지 않을 파일**을 매번 컴파일하는 것은 약간의 부담이 된다. 그러한 파일들을 미리 컴파일 해둔 후 빌드할 때마다 연결해주기 위해 사용한다. ``` .pch ``` 확장자로도 사용하는데 지금은 일반 파일로 관리해 볼 것이다.

- **pch.h**

```c++
// pch.h: 미리 컴파일된 헤더 파일입니다.
// 아래 나열된 파일은 한 번만 컴파일되었으며, 향후 빌드에 대한 빌드 성능을 향상합니다.
// 코드 컴파일 및 여러 코드 검색 기능을 포함하여 IntelliSense 성능에도 영향을 미칩니다.
// 그러나 여기에 나열된 파일은 빌드 간 업데이트되는 경우 모두 다시 컴파일됩니다.
// 여기에 자주 업데이트할 파일을 추가하지 마세요. 그러면 성능이 저하됩니다.

#ifndef PCH_H
#define PCH_H

// 여기에 미리 컴파일하려는 헤더 추가
#define WIN32_LEAN_AND_MEAN             // 거의 사용되지 않는 내용을 Windows 헤더에서 제외합니다.

#include "EnginePch.h"	// Client에서도 사용할 수 있도록 따로 만들어 둔 공용헤더

#endif //PCH_H
```

- **pch.cpp**

```c++
// pch.cpp: 미리 컴파일된 헤더에 해당하는 소스 파일

#include "pch.h"

// 미리 컴파일된 헤더를 사용하는 경우 컴파일이 성공하려면 이 소스 파일이 필요합니다.
```

사용법은 간단하다. pch 파일을 따로 만들어 미리 컴파일 할 부분만 작성해주면 된다. 위의 ``` EnginePch ``` 파일은 ``` Engine ``` 파일에 대한 pch 파일로, 따로 만들어주어 관리하도록 하였다.

***

## 👻 글을 마치며
다렉을 공부할 때는 소스코드 하나하나 세세하게 다룬다기 보단 프로그램 전체의 흐름을 익히는 방식으로 공부를 진행할 예정이다. 그래서 소스코드 분석은 따로 하지 않을 것이고 대략적인 흐름만 기록해두려한다. 아직까진 각각의 파일이 정확히 어떠한 기능을 하는지는 100% 이해하지 못하지만 계속 보다보면 이해할 수 있을 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/uEZu)_   