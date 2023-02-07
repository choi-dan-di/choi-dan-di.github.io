---
title: "[Algorithm] 기본 정렬(버블, 선택, 삽입)"
excerpt: "데이터를 정렬하는 방법 중 버블, 선택, 삽입 정렬에 대해 알아보기"

categories:
  - Algorithm
tags:
  - [Algorithm, data structure, sort, basic, bubble, selection, insertion]

permalink: /algorithm/basic-sort/

toc: true
toc_sticky: true

date: 2023-02-07 16:36:25+0900
last_modified_at: 2023-02-07 16:36:27+0900
---
 
## 👻 기본 정렬
이번 시간에는 알고리즘 중 **정렬(Sort)**에 대해 알아보자. 기본 정렬에는 **버블 정렬(Bubble Sort), 선택 정렬(Selection Sort), 삽입 정렬(Insertion Sort)**이 존재한다. 가장 간단한 방법이지만 모두 시간 복잡도는 등차수열의 합인 ``` O(N²) ```으로 비효율적인 정렬 방법이다.

***

### 🌱 버블 정렬
**버블 정렬(Bubble Sort)**은 배열의 첫 번째 수부터 바로 다음의 수를 비교하여 정렬하는 방법이다. 정렬 기준에 맞게 데이터를 교환하거나 하지않으며 다음으로 비교할 두 수는 두 번째, 세 번째에 존재하는 수이다. 모든 배열의 수의 비교가 끝나면 **가장 큰 수가 가장 마지막 원소로 존재**할 것이고, 다시 배열의 처음으로 돌아와 마지막 수를 제외하고 비교를 반복한다.

```c++
void BubbleSort(vector<int>& v)
{
    const int n = (int)v.size();

    // (N-1) + (N+2) + ... + 2 + 1
    // 등차수열의 합 = N * (N-1) / 2
    // 시간 복잡도는 O(N^2)
    for (int i = 0; i < n - 1; i++)
    {
        for (int j = 0; j < (n - 1 - i); j++)
        {
            if (v[j] > v[j + 1])
            {
                // swap(v[j], v[j + 1]);
                int temp = v[j];
                v[j] = v[j + 1];
                v[j + 1] = temp;
            }
        }
    }

    cout << "BubbleSort : ";
    for (int i : v)
        cout << i << " ";
    cout << endl;
}
```

***

### 🌱 선택 정렬
**선택 정렬(Selection Sort)**은 배열을 처음부터 한 바퀴 돌며 **가장 작은 수**를 뽑아낸다. 한 바퀴 모두 돌고나면 가장 작은 수는 첫 번째부터 위치하게 된다. 데이터의 교환이 이루어질 때 원래 가장 작은 수가 있던 자리에 첫 번째 수가 들어가는 방식으로 진행된다.

```c++
void SelectionSort(vector<int>& v)
{
    const int n = (int)v.size();

    // (N-1) + (N+2) + ... + 2 + 1
    // 등차수열의 합 = N * (N-1) / 2
    // 시간 복잡도는 O(N^2)
    for (int i = 0; i < n - 1; i++)
    {
        int bestIdx = i;
        for (int j = i + 1; j < n; j++)
        {
            if (v[j] < v[bestIdx])
                bestIdx = j;
        }

        int temp = v[i];
        v[i] = v[bestIdx];
        v[bestIdx] = temp;
    }

    cout << "SelectionSort : ";
    for (int i : v)
        cout << i << " ";
    cout << endl;
}
```

***

### 🌱 삽입 정렬
**삽입 정렬(Insertion Sort)**은 배열의 두 번째 수부터 비교를 시작하며 위의 두 정렬과는 반대로 이전 순서에 위치한 수들과 비교를 한다. 비교할 수와 그 앞자리에 있는 수를 비교하여 정렬이 되어있다면 바로 반복문을 빠져나오고 만약 비교할 수가 더 작아 데이터 교환이 이루어져야 한다면 앞자리 수에서 더 앞자리 수로 넘어가 다시 비교를 하게 된다. 지정된 자리를 찾아 데이터를 교환하는 방식이라 볼 수 있다.

```c++
void InsertionSort(vector<int>& v)
{
    const int n = (int)v.size();

    // 1 + 2 + ... + (N+2) + (N-1)
    // 등차수열의 합 = N * (N-1) / 2
    // 시간 복잡도는 O(N^2)
    for (int i = 1; i < n; i++)
    {
        int insertData = v[i];

        int j;
        for (j = i - 1; j >= 0; j--)
        {
            if (v[j] > insertData)
                v[j + 1] = v[j];
            else
                break;
        }

        v[j + 1] = insertData;
    }

    cout << "InsertionSort : ";
    for (int i : v)
        cout << i << " ";
    cout << endl;
}
```

***

## 👻 글을 마치며
이번 시간에는 정렬 중 기본 정렬 방법에 대해 알아보았다. 버블 정렬은 알고 있었던 터라 복습하는 의미로 간단히 넘어갔고, 선택 정렬과 삽입 정렬은 이번 시간에 새로 알게 되었다. 생각보다 간단하지만 그만큼 상당히 비효율적인 알고리즘이라 생각이 들어 실 사용 시에는 지양해야 할 것 같다.

***

_출처_   
_[인프런 Rookiss님 강의](https://inf.run/1JwV)_