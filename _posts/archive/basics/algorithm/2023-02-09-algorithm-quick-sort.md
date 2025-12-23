---
title: "[Algorithm] 퀵 정렬(Quick Sort)"
excerpt: "데이터를 정렬하는 방법 중 퀵 정렬에 대해 알아보기"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, sort, quick sort]

permalink: /algorithm/quick-sort/

toc: true
toc_sticky: true

date: 2023-02-09 17:05:53+0900
last_modified_at: 2023-02-09 17:05:54+0900
---
 
## 👻 퀵 정렬
**퀵 정렬(Quick Sort)**은 하나의 원소를 정해 나머지 원소와 비교하여 정렬하는 방식이다. 기준값이 되는 원소를 **피벗(Pivot)**이라 부르며 나머지 배열(리스트)에 속한 비교 인덱스는 ``` low ```와 ``` high ```, ``` i ```와 ``` j ``` 등 자유롭게 설정이 가능하다.

![Alt Text](/assets/images/posts_img/basics/algorithm/quick-sort/quick-sort-anim.gif)   
<span style="font-size: 0.7rem; color: gray;">파란 선은 피벗을 의미한다.</span>

> 퀵 정렬 방법은 다음과 같은 단계로 진행된다. 병합 정렬 방식(Divide and Conquer, 분할 정복)과 유사하다.
> 
> 1. 리스트 가운데서 하나의 원소를 골라 **피벗**을 설정해준다.
2. 나머지 원소의 첫 번째 인덱스를 ``` low ```, 마지막 인덱스를 ``` high ```라고 정의하고, ``` low ``` 인덱스의 값이 피벗보다 크면 ``` low + 1 ```, 작으면 멈춘다. ``` high ```는 반대로 피벗보다 크면 ``` high - 1 ```, 작으면 멈춘다.
3. ``` low ```가 ``` high ```보다 작으면 두 수를 교환한다.
4. ``` low ```가 ``` high ```보다 크거나 같으면 ``` high ```에 있는 값과 피벗을 교환한다.
5. 기존의 피벗값은 해당 인덱스에 고정으로 들어가게되며 피벗을 기준으로 왼쪽은 작은 수, 오른쪽은 큰 수들만 모여있게 된다.
6. 해당 피벗을 기준으로 왼쪽과 오른쪽 각각 퀵 정렬을 재귀적으로 실행한다.

- ``` QuickSort ```

```c++
void QuickSort(vector<int>& v, int left, int right)
{
    if (left > right)
        return;

    int pivot = Partition(v, left, right);
    QuickSort(v, left, pivot - 1);
    QuickSort(v, pivot + 1, right);
}
```

> ``` left ```는 배열의 첫 번째 인덱스, ``` right ```는 배열의 마지막 인덱스를 의미한다.

퀵 정렬을 한 번 실행하게되면 피벗의 인덱스가 정해지게 되니 새로운 피벗을 설정해주며 재귀적으로 정렬을 반복 실행하게 된다. 배열의 모든 원소를 확인하면 정렬이 종료되며 데이터를 비교, 교환하는 부분은 함수 ``` Partition ```에 구현하였다. ``` Partition ```의 코드는 다음과 같다.

- ``` Partition ```

```c++
int Partition(vector<int>& v, int left, int right)
{
    int pivot = v[left];
    int low = left + 1;
    int high = right;

    // O(N)
    while (low <= high)
    {
        while (low <= right && pivot >= v[low])
            low++;

        while (high >= left + 1 && pivot <= v[high])
            high--;

        if (low < high)
            swap(v[low], v[high]);
    }

    swap(v[left], v[high]);
    return high;
}
```

위의 코드에서는 피벗을 가장 첫 번째 원소로 설정하였다. 피벗을 결정하는 조건은 딱히 정해져있지 않기 때문에 배열 내의 아무 원소를 피벗으로 설정할 수 있다.

퀵 정렬도 병합 정렬과 마찬가지로 ``` O(Nlog N) ```의 시간 복잡도를 가진다. 해당 시간 복잡도는 **평균적인 상황**에서 적용이 되지만 **최악의 경우** ``` O(N²) ```의 시간 복잡도를 가질 수 있다.

> 💡 피벗이 배열 내에서 중간값이 아닌 최솟값 혹은 최댓값을 가지게 되면 그만큼 비교하는 과정이 N번 늘어나기 때문에 최악의 경우 위와 같은 시간 복잡도를 가지게 된다.

***

## 👻 글을 마치며
이번 시간에는 퀵 정렬에 대해 알아보았다. 시간 복잡도는 병합 정렬과 다르지 않고 어떻게 보면 제곱값까지 나오는 안 좋은 알고리즘처럼 보이는데 이름에 퀵이 들어가서 약간 이해하기 어려웠다. 그래도 두 수를 동시에 비교하고 한 번의 퀵 정렬이 끝나면 하나의 수는 무조건 자리가 확정되기 때문에 직관적으로 정렬이 되는 게 보이는 것 같다는 생각이 들었다. 이 정렬은 어떤 상황일 때 쓰이는건지 생각해 볼 필요가 있을 것 같다.

***

_출처_   
_[인프런 Rookiss님 강의](https://inf.run/1JwV)_