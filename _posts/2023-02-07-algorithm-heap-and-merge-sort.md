---
title: "[Algorithm] 힙 정렬(Heap Sort)과 병합 정렬(Merge Sort)"
excerpt: "데이터를 정렬하는 방법 중 힙 정렬과 병합 정렬에 대해 알아보기"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, sort, heap sort, merge sort, merge, heap]

permalink: /algorithm/heap-and-merge-sort/

toc: true
toc_sticky: true

date: 2023-02-07 19:18:33+0900
last_modified_at: 2023-02-07 19:18:35+0900
---
 
## 👻 힙 정렬
**힙 정렬(Heap Sort)**는 힙의 특성을 이용해 정렬하는 방식이다. 힙 트리 구조에서는 **우선순위 큐**로 자연스럽게 가장 큰 수 혹은 가장 작은 수를 뽑아 접근하는 방식이다. 배열의 모든 데이터를 우선순위 큐에 담고 큐가 빌 때까지 다시 배열에 재배치 하는 방식을 사용한다.

```c++
void HeapSort(vector<int>& v)
{
    priority_queue<int, vector<int>, greater<int>> pq;

    // O(Nlog N)
    for (int num : v)
        pq.push(num);

    v.clear();

    // O(Nlog N)
    while (!pq.empty())
    {
        v.push_back(pq.top());
        pq.pop();
    }

    cout << "HeapSort : ";
    for (int i : v)
        cout << i << " ";
    cout << endl;

    // 2Nlog N => 결과적으로 O(Nlog N)
}
```

작은 수부터 오름차순으로 정렬해야 하기 때문에 템플릿의 ``` greater ```를 이용하였다(최소 힙 사용). 힙 구조의 시간 복잡도는 ``` O(log N) ```이고 데이터 수만큼 반복 실행하기 때문에 **힙 정렬의 시간 복잡도는** ``` O(Nlog N) ```이 된다.

***

## 👻 병합 정렬
**병합 정렬(Merge Sort, 혹은 합병 정렬)**은 **분할 정복(Divide and Conquer)** 방식을 이용해 배열의 데이터를 정렬하는 방식이다. 우선 데이터를 반씩 분할하여 정렬 후 다시 합치는 과정을 거치게 된다. 병합 정렬도 힙 정렬과 마찬가지로 ``` O(Nlog N) ```의 시간 복잡도를 가진다.

![Alt Text](/assets/images/posts_img/basics/algorithm/heap-and-merge-sort/merge-sort-anim.gif)   

위 이미지는 합병 정렬의 예시를 보여준다.

> 💡 **합병 정렬의 순서**   
1. **분할(Divide)** : 배열을 절반으로 분할한다.
2. **정복(Conquer)** : 재귀적으로 분할 및 정렬 과정을 거친다.
3. **결합(Combine)** : 분할하여 정렬된 배열을 다시 합친다. 각 부분 배열은 정렬된 상태이다.
4. **복사(Copy)** : 위 과정 모두 임시 배열에 저장하고 있는데, 이 배열을 원래 배열에 복사한다.

- ``` MergeSort ``` : 분할하고 정렬하라는 큰 알고리즘   
```c++
void MergeSort(vector<int>& v, int left, int right)
{
    cout << "Now MS(" << left << ", " << right << ")" << endl;
    if (left >= right)
    {
        cout << "Return MS(" << left << ", " << right << ")" << endl;
        return;
    }

    int mid = (left + right) / 2;
    MergeSort(v, left, mid);
    MergeSort(v, mid + 1, right);

    MergeResult(v, left, mid, right);
}
```

- ``` MergeResult ``` : 여기서 배열의 해당 범위만큼 정렬 및 값 복사가 이뤄진다.   
```c++
void MergeResult(vector<int>& v, int left, int mid, int right)
{
    cout << "MR(" << left << ", " << mid << ", " << right << ")" << endl;
    int leftIdx = left;
    int rightIdx = mid + 1;

    vector<int> temp;
    while (leftIdx <= mid && rightIdx <= right)
    {
        if (v[leftIdx] <= v[rightIdx])
        {
            temp.push_back(v[leftIdx]);
            leftIdx++;
        }
        else
        {
            temp.push_back(v[rightIdx]);
            rightIdx++;
        }
    }

    // 왼쪽이 먼저 끝났으면 오른쪽 나머지 데이터 복사
    if (leftIdx > mid)
    {
        while (rightIdx <= right)
        {
            temp.push_back(v[rightIdx]);
            rightIdx++;
        }
    }
    else
    {
        while (leftIdx <= mid)
        {
            temp.push_back(v[leftIdx]);
            leftIdx++;
        }
    }

    for (int i = 0; i < temp.size(); i++)
        v[left + i] = temp[i];
}
```

> 💡 여기서 많이 헤맸었는데 코드도 해당 순서대로 진행될 것이라는 예상으로 리뷰했지만 도저히 이해가 가지 않았다. 코드 자체를 직접 분석해보니 맨 처음 분할 했을 때 **앞 부분의 분할 및 정렬이 먼저 진행되어 끝난 상태로 대기**해 있으며 그 후에 **뒷 부분의 분할 및 정렬이 진행**된다고 해석할 수 있다.   
>
> **Q. 배열** ``` vector<int> v{ 1, 5, 3, 2, 4, 6, 8, 7 } ``` **을 병합 정렬되어지는 순서**   
> **A.**   
> ![Alt Text](/assets/images/posts_img/basics/algorithm/heap-and-merge-sort/merge-sort-result.PNG)   

***

## 👻 글을 마치며
이번 시간에는 힙 정렬과 병합 정렬에 대해 알아보았다. 힙 정렬은 쉬워서 금방 넘어갔었는데 병합 정렬에서 애 많이 먹었다. 😭 거의 모든 자료들이 분할 정복 과정을 잘 나타내고 있지만 코드상으로는 앞 반절 먼저, 그다음 뒷 반절 진행이었는데.. 여기서 많이 헤맸었던 것 같다. 그래도 완벽히 이해할 때까지 코드를 보고 보고 또 보니까 이해가 가는 게 나름 뿌듯했다. ~~정신 나갈 것 같아 😖~~

***

_출처_   
_[인프런 Rookiss님 강의](https://inf.run/1JwV)_