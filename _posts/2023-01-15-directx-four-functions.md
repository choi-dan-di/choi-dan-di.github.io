---
title: "[DirectX 12] 다양한 함수들(Device, SwapChain, DescriptorHeap, CommandQueue)"
excerpt: "다양한 기능을 하는 대표 함수들에 대해 알아보기"

categories:
  - DirectX
tags:
  - [DirectX, dx, dx12, graphics, rendering pipeline, pipeline, summary, device, swapchain, descriptorheap, commandqueue]

permalink: /directx/four-functions/

toc: true
toc_sticky: true

date: 2023-01-15 16:53:25+0900
last_modified_at: 2023-01-15 16:53:27+0900
---

## 👻 다양한 함수들
함수를 기능별로 따로 나누면 관리하기 편해지는데 크게 네 가지 정도로 분류할 수 있다. 명령어 하나하나 자세히 알기 보단 대략적으로 각각의 함수가 어떻게 연결되어있고 어떤 동작을 수행하는지 간단하게 알아보자.

***

### 🌱 Device
CPU, GPU에 해당하며 디스플레이 어댑터(그래픽 카드)와 연결시켜주는 함수이다. 제조사에 상관없이 표준화되어있어 DirectX 라이브러리 하나만 사용하여 별도의 설정 없이 개발을 할 수 있도록 해준다. 다양한 기능을 하는 나머지 함수들을 실행시킬 수 있도록 도와준다.

***

### 🌱 SwapChain
``` Device ``` 함수에서 내가 동작시킬 장치의 정보를 담았다면, 그 장치에 전달할 내용을 도와주는 함수이다. 현재 게임 세상에 있는 상황을 묘사하고, 어떤 공식으로 어떻게 계산할지 정해줘야한다. 그런 다음 GPU가 계산을 완료하면 결과물이 나오게 되는데, 이 결과물을 어디에 받을지 정해주는 기능을 한다. 결과물은 보통 **버퍼(Buffer)**를 통해 받게 되는데 두 개 이상의 버퍼를 이용해 결과물을 받고, 다음 결과물을 다른 버퍼에 그리는 동작을 반복 수행한다. 이러한 작업을 **더블 버퍼링(Double Buffering)**이라고 한다. 현재 출력되는 버퍼가 아닌 다음 결과물을 그리는 버퍼를 **백 버퍼(Back Buffer)**라고 한다.

***

### 🌱 DescriptorHeap
``` SwapChain ``` 함수에서 버퍼를 정해서 결과물을 담았다면, 이 버퍼가 어떠한 역할을 하는지 정의해주는 함수이다. 버퍼는 다양한 역할을 수행할 수 있지만 정확히 어떤 역할인지는 알 수 없다. 이 부분을 보완시켜주는 함수라고 볼 수 있다. 기안서와 비슷한 개념이다. 아래의 뷰들에 따라 설정이 바뀐다.

- RTV(RenderTargetView)
- DSV(DepthStencilView)
- CBV(ConstantBufferView)
- SRV(ShaderResourceView)
- UAV(UnorderedAccessView)

***

### 🌱 CommandQueue
GPU에게 건네줄 실행 목록을 작성해주는 함수이다. 명령어가 호출되었을 때 그 즉시 실행해야 할 명령어도 있지만 나중에 실행해야 할 명령어도 존재할 것이다. 또한 실행해야 할 명령어들이 너무 많으면 CPU가 놀고 반대로 명령어들이 너무 없으면 GPU가 놀게 될 것이다. 이러한 일의 비율을 맞춰주기 위해 커맨드 큐를 사용한다. 이러한 과정에서 CPU와 GPU의 실행 차이가 생기는데, 둘의 동기화를 위해 ``` Fence ``` 라는 간단한 도구를 사용한다.

***

## 👻 글을 마치며
이번 시간에는 다이렉트X의 동작 순서에서 대략적으로 어떠한 기능이 어떤식으로 연결되어있는지 다양한 함수를 통해 알아보았다. 머리로는 이해가 쉬운데 막상 직접 만들려고 하면 쉽지 않을 것 같다. 또한 오랜만에 복습을 한 부분이라 기억이 좀 흐릿했는데 그래도 첫 이론 공부 때 완벽히 이해를 하고 넘어갔더니 금세 기억들을 떠올릴 수 있었다. 꼭 완벽히 이해해서 내가 이길 것임! 😎

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/uEZu)_   