---
title: "[DirectX 12] 다양한 기능의 함수들"
excerpt: "다양한 기능을 하는 대표 함수들에 대해 알아보기"

categories:
  - DirectX
tags:
  - [DirectX, dx, dx12, graphics, rendering pipeline, pipeline, summary, device, swapchain, descriptorheap, commandqueue, constant buffer, root signature, table desc heap, index buffer, mesh, shader, texture mapping, texture, uv, depth stencil view]

permalink: /directx/dx-functions/

toc: true
toc_sticky: true

date: 2023-01-15 16:53:25+0900
last_modified_at: 2023-01-16 16:23:25+0900
---

## 👻 다양한 함수들
함수를 기능별로 따로 나누면 관리하기 편해지는데 크게 네 가지 정도로 분류할 수 있다. 그 외에 여러 기능을 함수로 분리하여 생성하고 명령어 하나하나 자세히 알기 보단 대략적으로 각각의 함수가 어떻게 연결되어있고 어떤 동작을 수행하는지 간단하게 알아보자.

***

### 🌱 Device
CPU, GPU에 해당하며 디스플레이 어댑터(그래픽 카드)와 연결시켜주는 함수이다. 제조사에 상관없이 표준화되어있어 DirectX 라이브러리 하나만 사용하여 별도의 설정 없이 개발을 할 수 있도록 해준다. 다양한 기능을 하는 나머지 함수들을 실행시킬 수 있도록 도와준다.

***

### 🌱 Swap Chain
``` Device ``` 함수에서 내가 동작시킬 장치의 정보를 담았다면, 그 장치에 전달할 내용을 도와주는 함수이다. 현재 게임 세상에 있는 상황을 묘사하고, 어떤 공식으로 어떻게 계산할지 정해줘야한다. 그런 다음 GPU가 계산을 완료하면 결과물이 나오게 되는데, 이 결과물을 어디에 받을지 정해주는 기능을 한다. 결과물은 보통 **버퍼(Buffer)**를 통해 받게 되는데 두 개 이상의 버퍼를 이용해 결과물을 받고, 다음 결과물을 다른 버퍼에 그리는 동작을 반복 수행한다. 이러한 작업을 **더블 버퍼링(Double Buffering)**이라고 한다. 현재 출력되는 버퍼가 아닌 다음 결과물을 그리는 버퍼를 **백 버퍼(Back Buffer)**라고 한다.

***

### 🌱 Descriptor Heap
``` SwapChain ``` 함수에서 버퍼를 정해서 결과물을 담았다면, 이 버퍼가 어떠한 역할을 하는지 정의해주는 함수이다. 버퍼는 다양한 역할을 수행할 수 있지만 정확히 어떤 역할인지는 알 수 없다. 이 부분을 보완시켜주는 함수라고 볼 수 있다. 기안서와 비슷한 개념이다. 아래의 뷰들에 따라 설정이 바뀐다.

- RTV(RenderTargetView)
- DSV(DepthStencilView)
- CBV(ConstantBufferView)
- SRV(ShaderResourceView)
- UAV(UnorderedAccessView)

***

### 🌱 Command Queue
GPU에게 건네줄 실행 목록을 작성해주는 함수이다. 명령어가 호출되었을 때 그 즉시 실행해야 할 명령어도 있지만 나중에 실행해야 할 명령어도 존재할 것이다. 또한 실행해야 할 명령어들이 너무 많으면 CPU가 놀고 반대로 명령어들이 너무 없으면 GPU가 놀게 될 것이다. 이러한 일의 비율을 맞춰주기 위해 커맨드 큐를 사용한다. 이러한 과정에서 CPU와 GPU의 실행 차이가 생기는데, 둘의 동기화를 위해 ``` Fence ``` 라는 간단한 도구를 사용한다.

***

### 🌱 Constant Buffer
**일반적인 상수를 저장하기 위해 사용하는 버퍼(Buffer)**이다. 정점 버퍼와 비슷하지만 상수를 저장하기 때문에 훨씬 더 범용적으로 사용할 수 있다. 그 부분을 제외하고는 크게 다른 부분이 없다. 전역 변수처럼 사용이 가능하다.

상수 버퍼는 **256바이트의 배수**로 만들어야 한다. (0, 256, 512, 768, ...) 이러한 크기를 정하는 공식은 다음과 같다.

> **(size + 255) & ~255**

``` & ~255 ``` 연산은 **하위 8비트를 전부 0으로 세팅**해준다. 숫자가 사이에 끼어있다고 가정했을 때 왼쪽(낮은 쪽)에 있는 수를 선택하고 256 미만의 수는 0으로 세팅되므로 오른쪽(높은 쪽)에 있는 수를 선택하기 위해 사이즈에 255를 더해주어 반올림을 한다.

> 일반적으로 상수 버퍼는 이름이 ``` b ```로 시작하는 레지스터를 사용한다.

***

### 🌱 Root Signature
내가 사용할 **레지스터나 버퍼를 GPU 쪽에 명시해주는 기능**을 한다. 계약서와 비슷한 개념이다. 상수 버퍼의 수가 적으면 일일이 명시해도 상관없지만 버퍼의 수가 많아져서 테이블 형식(다음에 나올 개념:Table DescriptorHeap)으로 생성된다면 배열 형식으로 버퍼의 사이즈만큼 지정해주어 명시(서명)해줘야한다.

```c++
void RootSignature::CreateRootSignature()
{
	CD3DX12_DESCRIPTOR_RANGE ranges[] =
	{
		CD3DX12_DESCRIPTOR_RANGE(D3D12_DESCRIPTOR_RANGE_TYPE_CBV, CBV_REGISTER_COUNT, 0),	// b0 ~ b4
		CD3DX12_DESCRIPTOR_RANGE(D3D12_DESCRIPTOR_RANGE_TYPE_SRV, SRV_REGISTER_COUNT, 0),	// t0 ~ t4
	};

	CD3DX12_ROOT_PARAMETER param[1];
	param[0].InitAsDescriptorTable(_countof(ranges), ranges);

	D3D12_ROOT_SIGNATURE_DESC sigDesc = CD3DX12_ROOT_SIGNATURE_DESC(_countof(param), param, 1, &_samplerDesc);
	sigDesc.Flags = D3D12_ROOT_SIGNATURE_FLAG_ALLOW_INPUT_ASSEMBLER_INPUT_LAYOUT; // 입력 조립기 단계

	ComPtr<ID3DBlob> blobSignature;
	ComPtr<ID3DBlob> blobError;
	::D3D12SerializeRootSignature(&sigDesc, D3D_ROOT_SIGNATURE_VERSION_1, &blobSignature, &blobError);
	DEVICE->CreateRootSignature(0, blobSignature->GetBufferPointer(), blobSignature->GetBufferSize(), IID_PPV_ARGS(&_signature));
}
```

***

#### 🪐 Table Descriptor Heap
``` DescriptorHeap ```이 여러개 붙어있는 구조를 의미한다. 메모리의 할당 받을 부분을 테이블 형식으로 쓴다는 의미이며 개수를 입력받음으로써 사용할 메모리의 범위를 지정해줄 수 있다. 일차원 배열과 다차원 배열의 차이와 비슷하다고 보면 될 것 같다.

> 💡 **참고 : 흐름도**   
![Alt Text](/assets/images/posts_img/basics/directx/dx-functions/desc-heap.PNG)   
왼쪽이 CPU, 오른쪽이 GPU이다.   
- **Render 순서**   
  1. Buffer에 데이터 세팅
  2. Table Desciptor Heap에 _CBV_ 전달
    - Constant Buffer를 가리키는 View(일종의 서명서)
  3. 세팅이 모두 끝났으면 Table Descriptor Heap 커밋

***

### 🌱 Index Buffer
``` Vertex Buffer ```를 이용하여 삼각형을 만들고, 더 나아가 사각형을 만들 수 있다. 하지만 사각형을 만들게 되면 겹치는 정점이 존재하게 된다. 삼각형과 같은 단순한 메쉬는 큰 문제가 없지만 다양한 삼각형이 합쳐진 복잡한 메쉬같은 경우 프로그램에 큰 부하가 일어날 수 있다. 이러한 문제를 방지하기 위해 ``` Index Buffer ```를 추가하여 **함께 사용**한다. 해당 버퍼는 정점의 정보(인덱스 번호)만 담고 있기 때문에 중복을 방지한다.

```c++
// 사각형 만들기
// Vertex only
vector<Vertex> vec(6);
vec[0].pos = Vec3(-0.5f, 0.5f, 0.5f);
vec[0].color = Vec4(1.f, 0.f, 0.f, 1.f);
vec[1].pos = Vec3(0.5f, 0.5f, 0.5f);
vec[1].color = Vec4(0.f, 1.f, 0.f, 1.f);
vec[2].pos = Vec3(0.5f, -0.5f, 0.5f);
vec[2].color = Vec4(0.f, 0.f, 1.f, 1.f);

vec[3].pos = Vec3(0.5f, -0.5f, 0.5f);
vec[3].color = Vec4(0.f, 0.f, 1.f, 1.f);
vec[4].pos = Vec3(-0.5f, -0.5f, 0.5f);
vec[4].color = Vec4(0.f, 1.f, 0.f, 1.f);
vec[5].pos = Vec3(-0.5f, 0.5f, 0.5f);
vec[5].color = Vec4(1.f, 0.f, 0.f, 1.f);

// Vectex + Index Buffer
vector<Vetrex> vec(4);
vec[0].pos = Vec3(-0.5f, 0.5f, 0.5f);
vec[0].color = Vec4(1.f, 0.f, 0.f, 1.f);
vec[1].pos = Vec3(0.5f, 0.5f, 0.5f);
vec[1].color = Vec4(0.f, 1.f, 0.f, 1.f);
vec[2].pos = Vec3(0.5f, -0.5f, 0.5f);
vec[2].color = Vec4(0.f, 0.f, 1.f, 1.f);

vec[3].pos = Vec3(-0.5f, -0.5f, 0.5f);
vec[3].color = Vec4(0.f, 1.f, 0.f, 1.f);

// 사용할 버퍼 변경도 필수이다!!

// Indices
vector<uint32> indexVec;
{
  indexVec.push_back(0);
  indexVec.push_back(1);
  indexVec.push_back(2);
}
{
  indexVec.push_back(0);
  indexVec.push_back(2);
  indexVec.push_back(3);
}
```

> 💡 **Vertex vs Vertex + Index**   
![Alt Text](/assets/images/posts_img/basics/directx/dx-functions/index-buffer.png)   

***

### 🌱 Mesh
**정점으로 이루어진 물체**를 의미한다. 게임상에서 캐릭터, 몬스터 등과 같은 물체들이 모두 메쉬에 속한다. 여러 개의 폴리곤이 모여 만들어진 메쉬도 존재하기 때문에 해당 단위로 관리하기 위해 함수를 따로 만들어주었다. 여기에 다양한 텍스처나 쉐이더를 적용하여 하나의 메쉬로 정의할 수 있다.

***

### 🌱 Shader
메쉬에 **적용할** 여러가지 기능들을 기술해놓은 함수이다. 컬러, 위치 등 적용할 기능들이 모두 담겨있는 부분이다. 보통 파일을 따로 빼서 관리하며 로드 시에 파일을 읽어와 기능을 적용한다. 대부분 ``` Vertex Shader ```와 ``` Pixel Shader ```로 분리하여 관리한다.

***

### 🌱 Texture Mapping
**텍스처(Texture)**는 3D 그래픽의 게임에서 폴리곤(Polygon)으로 제작한 3D 오브젝트(3D Object)에 **UV 좌표** 값을 맞춰 렌더링(Rendering)했을 때 보이게 되는 **2D 이미지**를 뜻한다. 3D 게임에서 보여지는 다양한 캐릭터들도 대부분 텍스처를 이어붙여 만든다. 주로 모델러들이 작업하는 부분이다. **UV 좌표로 정의된 텍스처를 XYZ 좌표로 정의된 기하에 매핑**하는 것을 **텍스처 매핑(Texture Mapping)**이라 한다.

> 💡 **사전 작업 : 이미지 로드 라이브러리 추가**   
DirectX에서 이미지를 로드하려면 [👉 ``` DirectXTex ``` **라이브러리** 👈](https://github.com/microsoft/DirectxTex)를 추가해줘야한다. **ZIP 파일**을 다운로드하고 각 버전과 사양에 맞는 솔루션을 실행시켜 헤더파일과 라이브러리를 추가해주면 된다. 솔루션을 빌드하면 파일이 설치가 되고 설치된 파일을 드래그 앤 드롭으로 실습 중인 프로젝트 내로 옮겨 환경 설정을 해주면 완료된다.

***

#### 🪐 Texture
텍스처를 만들고 로드하는 함수이다.

```c++
class Texture
{
public:
	void Init(const wstring& path);

	D3D12_CPU_DESCRIPTOR_HANDLE GetCpuHandle() { return _srvHandle; }

public:
	void CreateTexture(const wstring& path);
	void CreateView();

private:
	// 텍스처 만드는 부분
	ScratchImage _image;
	ComPtr<ID3D12Resource>	_tex2D;

	// 로드 부분
	ComPtr<ID3D12DescriptorHeap> _srvHeap;
	D3D12_CPU_DESCRIPTOR_HANDLE	_srvHandle;
};
```

텍스처 같은 경우 한 번 로드 후에 변경 없이 사용할 것이기 때문에 뷰를 하나만 만들어도 된다.

> 파일 확장자 얻기 : ``` wstring ext = fs::path(path).extension(); ```   
  - ``` path ``` : 파일 경로
  - ``` fs ``` : file system. 파일에 접근할 수 있다. C++17 버전 이후로 추가되었다.

***
#### 🪐 UV 좌표
**UV 좌표**란 **3차원 공간에서 폴리곤에 텍스처를 입히기 위한 기준이 되는 2차원 좌표**이다. 최소 0, 최대 1의 값을 가지고 있으며 폴리곤의 비율값과 동일하다. 좌측 상단이 0을 의미하는 기준값이며 오른쪽, 아래로 내려갈수록 좌표값이 증가한다.

![Alt Text](/assets/images/posts_img/basics/directx/dx-functions/uv.png)   

```c++
// vectex vector에 UV 좌표값 추가해주기
vec[0].uv = Vec2(0.f, 0.f);
vec[1].uv = Vec2(1.f, 0.f);
vec[2].uv = Vec2(1.f, 1.f);
vec[3].uv = Vec2(0.f, 1.f);
```

정점에 대한 UV 좌표만 매핑해주면 쉽게 텍스처를 붙일 수 있다.

***

### 🌱 Depth Stencil View
렌더링 파이프라인에서 마지막 부분에 해당되는 뷰이며 3D 물체를 나타낼 때 필요한 과정이다. **3D 물체를 투영했을 때** 겹쳐지는 부분에 데이터가 많으면 로드할 때 부하가 많이 될 수 있다. 이러한 문제를 보완하려면 앞에 겹치는 물체가 있을 때 뒤에 있는 물체는 표시할 필요가 없어야한다. 이러한 작업을 할 수 있도록 해주는 부분이 바로 ``` Depth Stencil View ```이다. 깊이는 앞쪽이 0, 뒤쪽이 1이며 UV 좌표와는 달리 **투영 좌표계**를 사용하며 정중앙이 (0, 0)이고 가로축은 X, 세로축은 Y값을 의미한다.

겹쳐지는 물체가 없다면 **1.0(초기값. 변경 가능)**, 있다면 더 얕은 물체의 ``` depth ``` 값을 버퍼에 추가한다. 그렇게되면 앞에 있는 물체만 보여지고 뒤쪽의 물체는 그려지지 않을 것이다.

> **스텐실(Stencil)** : 판에 구멍을 뚫고 여기에 잉크를 통과시켜 찍어내는 공판화 기법의 하나.

``` Depth Buffer ```에 ``` Stencil ``` 옵션값을 추가할 수 있다. 스텐실로 들어간 값은 데이터를 분류할 때 사용하는 등의 **조건값**으로 사용할 수 있다.

***

## 👻 글을 마치며
이번 시간에는 다이렉트X의 동작 순서에서 대략적으로 어떠한 기능이 어떤식으로 연결되어있는지 다양한 함수를 통해 알아보았다. 머리로는 이해가 쉬운데 막상 직접 만들려고 하면 쉽지 않을 것 같다. 또한 오랜만에 복습을 한 부분이라 기억이 좀 흐릿했는데 그래도 첫 이론 공부 때 완벽히 이해를 하고 넘어갔더니 금세 기억들을 떠올릴 수 있었다. 꼭 완벽히 이해해서 내가 이길 것임! 😎

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/uEZu)_   