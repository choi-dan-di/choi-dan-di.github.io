---
title: "[Unreal Engine] 언리얼 엔진 최적화 - 1. 프로파일링"
excerpt: "언리얼 엔진의 최적화 방법에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, optimization, profiling]

permalink: /unreal/optimization-profiling/

toc: true
toc_sticky: true

date: 2023-04-26 15:52:55+0900
last_modified_at: 2023-04-26 15:52:58+0900
published: true
---

## 👻 언리얼 엔진 최적화
언리얼 엔진에서 사용할 수 있는 몇 가지 최적화 방법은 다음과 같다.

> - **Level Optimization**
  - 불필요한 요소를 제거하고 LOD 시스템을 적용하여 불필요한 면을 제거한다. 또한 지형을 배치할 때, 적절한 사이즈의 텍스처를 사용하고 복잡한 지형을 적용하는 대신 불규칙한 지형을 사용하는 것이 좋다.
- **Mesh Optimization**
  - 다각형 수를 최소화하고 적절한 LOD를 적용하여 메시 최적화를 수행한다. 또한 다각형이 너무 작은 메시를 생성하는 것보다는 적절한 다각형 수를 유지하는 것이 좋다.
- **Material Optimization**
  - 머티리얼을 최적화하여 불필요한 텍스처를 제거하고, 텍스처의 해상도를 줄여 메모리 사용을 최소화한다. 또한 사용하지 않는 머티리얼을 제거하는 것도 좋다.
- **LightMap Optimization**
  - 라이트맵 퀄리티를 조정해 불필요한 라이트맵을 생성하지 않고, 간단한 라이트맵을 적용하여 불필요한 처리를 최소화한다.
- **Shader Optimization**
  - 최적화가 필요한 쉐이더를 분석해 적절한 최적화 방법을 선택하고, 복잡한 쉐이더를 단순화하여 처리 속도를 높인다.
- **Code Optimization**
  - 코드를 최적화하여 불필요한 함수 호출을 제거하고, CPU 자원을 효율적으로 사용한다.
- **Profiling**
  - 언리얼 엔진 내의 프로파일링 도구를 사용하여 어떤 부분이 가장 많은 자원을 사용하는지 분석하고, 최적화할 부분을 찾는다.

여기서 마지막으로 소개된 프로파일링에 대해 정리해보았다.

***

## 👻 Profiling
**프로파일링(Profiling)** 또는 **성능 분석**은 프로그램의 시간 복잡도 및 공간(메모리), 특정 명령어 이용, 함수 호출의 주기와 빈도 등을 측정하는 동적 프로그램 분석의 한 형태이다.

언리얼 엔진에는 여러가지 기능이 제공되며 다양한 퍼포먼스적 특징이 있다. 컨텐츠나 약간의 코드를 최적화시켜 필요한 수준의 퍼포먼스를 내기 위해서는 어디서 퍼포먼스가 새는지 확인할 필요가 있다. 그 방법으로 엔진 프로파일링 툴을 사용한다. 프로파일링을 진행할 때에는 큰 범위에서부터 시작해 범위를 좁혀나가며 분석하는 것이 효율적이다.

***

### 🌱 Stat 확인하기
언리얼 엔진에서 제공하는 콘솔 명령어를 이용하면 전체 상태를 쉽게 확인할 수 있다. ``` stat ``` 명령어를 사용한다.

- **stat unit**

언리얼 엔진 에디터 하단의 콘솔에 stat unit을 입력한 후 엔터를 누르면 뷰포트에 아래와 같은 정보가 나타난다.

![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/unit1.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/unit2.PNG)   

60프레임을 목표로 한다면 Frame 시간이 16.66ms보다 작아야 하며, 30프레임을 목표로 한다면 33.33ms 보다 작아야한다.

|**Frame**|게임의 한 프레임을 생성하는 데 소요된 총 시간|
|**Game**|게임 스레드에서 사용된 시간   값이 Frame과 비슷하면 게임의 성능은 게임 스레드에 의해 병목 현상이 발생한 것으로 볼 수 있다.|
|**Draw**|렌더링 스레드에서 사용된 시간   값이 Frame과 비슷하면 게임 성능은 렌더링 스레드에 의해 병목 현상이 발생한 것으로 볼 수 있다.|
|**GPU**|씬 렌더링에 사용된 GPU에서 소요된 시간   Frame 시간에 동기화되므로 비슷한 값을 가진다.|
|**RHIT**|렌더링 하드웨어 인터페이스 스레드에서 소요된 시간(예: OpenGL, D3D12)|
|**DynRes**|동적 해상도가 지원되는 경우, 현재의 Dynamic Resolution 해상도를 표시|
|**Draws**|현재 DrawPrimitive 호출 수|
|**Prims**|현재 그려지는 삼각형 수|

> 대부분의 경우 프로파일링의 시작은 이 명령을 통해 진행된다.

- **stat fps**

현재 fps를 보여준다.

![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/fps1.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/fps2.PNG)   

- **stat unitGraph**

unit 목록들을 그래프화시켜 정보들을 시각적으로 보여준다. ``` stat unit ```으로 보여지는 목록들의 이름 색상이 각 그래프의 색상에 맞게 변경된다.

![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/graph1.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/graph2.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/graph3.PNG)   

- **stat gpu**

GPU가 처리 중인 작업들을 보여준다.

![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/gpu1.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/gpu2.PNG)   

- **stat game**

게임 스레드가 처리 중인 작업들을 보여준다.

![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/game1.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/game2.PNG)   

- **stat slow**

작업 중 느린 부분을 보여준다.

![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/slow1.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/slow2.PNG)   

- **stat SceneRendering**

드로우 콜 수, 그림자, 라이팅 등과 같이 씬 렌더링과 관련된 작업 내용을 보여준다.

![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/rendering1.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/rendering2.PNG)   

- **stat DumpFrame**

작업들이 어느 정도의 수행시간을 갖는지 보여준다. ``` -ms=0.1 ```은 수행 시간의 한계치를 의미한다. 결과는 Output Log에서 확인 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/dump1.PNG)   
![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/dump2.PNG)   

필요하다면 코드에 ``` QUICK_SCOPE_CYCLE_COUNTER ```를 추가하여 다음 예제와 같이 계층구조를 한층 더 정제시킬 수 있다.

```c++
virtual void DrawDynamicElements(FPrimitiveDrawInterface* PDI,const FSceneView* View) override
{
    QUICK_SCOPE_CYCLE_COUNTER( STAT_BoxSceneProxy_DrawDynamicElements );

    const FMatrix& LocalToWorld = GetLocalToWorld();
    const FColor DrawColor = GetSelectionColor(BoxColor, IsSelected(), IsHovered(), false);
    DrawOrientedWireBox(PDI, LocalToWorld.GetOrigin(), ... );
}
```

이 밖에도 다양한 명령어가 존재하며, 블루프린트를 이용해 해당 프로파일링을 온오프하거나 디버깅 조건을 설정할 수도 있다.

> 💡 모든 stat 정보를 끄고싶다면 ``` stat none ``` 명령어를 사용한다.

***

#### 🪐 최적화하기
``` stat ``` 명령으로 나타나는 **Mesh Draw Calls**은 3D 메시로 인한 드로우 콜을 보여주며, 아티스트가 직접적으로 다음과 같은 방법을 통해 수치를 줄여 최적화를 할 수 있다.

> - **오브젝트 수 감소**
  - Static/Dynamic Mesh
  - Mesh Particles
- **뷰 거리 감소**
  - SceneCaptureActor 또는 Object 별
- **뷰 조정**
  - 뷰 줌 아웃
  - 오브젝트가 같은 뷰에 있지 않도록 이동
- **SceneCaptureActor 피하기**
  - 씬을 다시 렌더링해야 하므로, fps를 낮게 설정하거나 필요할 때만 업데이트
- **분할화면 피하기**
  - 단일 뷰일 때보다 항상 CPU에 묶이며, 커스텀 엔진 퀄리티(Scalability) 세팅이나 좀 더 적극적인 컨텐츠 작업 필요
- **드로우 콜당 엘리먼트 수 감소**
  - 머테리얼을 합쳐 좀 더 복잡한 픽셀 쉐이더 받기
  - 단순 머테리얼 수 줄이기
  - 머테리얼 수가 줄어드는 경우에만 텍스처를 적은 수의 커다란 텍스처로 합치기
  - 엘리먼트가 적은 LOD 모델 사용하기
- **메시에 커스텀 뎁스나 그림자 드리우기같은 기능 끄기**
- **라이트 소스가 그림자를 드리우지 않게 하기**
- **바운딩 볼륨을 타이트하게 잡기**
  - 뷰 원뿔
  - 감쇠 반경

***

### 🌱 최적화 뷰모드 활용하기
언리얼 엔진 에디터의 뷰포트에서 해당 기능을 사용할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/optimization-profiling/viewmode.PNG)   

각각의 기능을 활용하면 어느 부분에 과부하가 걸리는지 확인이 가능하다.

***

### 🌱 프로파일링 툴
> - **Unreal Insight**
  - CPU 병목 구간을 파악할 수 있다.
- **GPU 시각화 툴**
  - 콘솔 명령어 ``` ProfileGPU ``` 사용 혹은 **단축키** ``` Ctrl + Shift + , ```
  - GPU가 어떤 비중으로 퍼포먼스 비용을 사용하고 있는지 파악할 수 있다.

***

## 👻 글을 마치며
언리얼 엔진에서의 최적화 방법 중 하나인 프로파일링에 대해 간단히 알아보았다. 프로젝트의 규모가 커지면 최적화를 피해갈 수 없는데 이렇게 프로파일링을 이용해서 문제점을 파악하고 어떤 식으로 해결해야하는지 알 수 있는 좋은 시간이었다😏 글을 정리하면서도 느꼈지만 가장 중요한 점은 어느 부분이 문제인지를 명확하게 찾는 것인 것 같다. ~~그래서 디버깅이 중요하다는 말을 많이 들은 듯~~ 그래도 그래픽스를 공부하니 이해되는 부분이 많아서 나름 재미있는 공부였다! 얼른 써먹어봐야징😋

***

_참고_   
_[UE 공식문서 - 퍼포먼스와 프로파일링(CPU 프로파일링, GPU 프로파일링)](https://docs.unrealengine.com/4.27/ko/TestingAndOptimization/PerformanceAndProfiling/)_   
_[티스토리 - 최적화를 위한 프로파일링 개요 및 편의 기능들](https://devjino.tistory.com/373)_