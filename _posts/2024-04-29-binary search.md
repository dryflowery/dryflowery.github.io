---
title: 이분 탐색(Binary Search)
date: 2024-04-29 18:54:00 +09:00
categories: [Algorithm]
use_math: true
---

## **이분 탐색**
이분 탐색은 $O(logN)$의 시간복잡도를 가지는 탐색 알고리즘으로, 원소들이 정렬된 상태에서만 사용할 수 있다는 특징이 있다. 알고리즘 진행 방식은 다음과 같다.

>
첫 번째 원소를 low, 마지막 원소를 high로 설정한다.
1. mid = (low + high) / 2
2. mid가 key보다 작은 경우, low = mid
3. mid가 key보다 큰 경우, high = mid
4. mid값이 key와 같아질 때까지 반복한다.
>



![](/assets/img/algorithm/binary search/binary search 1.png)
![](/assets/img/algorithm/binary search/binary search 2.png)
![](/assets/img/algorithm/binary search/binary search 3.png)

---

### **이분 탐색 헷갈리지 않게 구현하기**
[Goat](https://www.acmicpc.net/blog/view/109)

---

### **코드**

```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
using namespace std;

vector<int> v{2, 8, 44, 56, 87, 99, 104, 143, 226, 786};


int BinarySearch(int key) {
    int lo = 0, hi = v.size() - 1;

    while(lo + 1 < hi) {
        int mid = (lo + hi) / 2;

        if(v[mid] < key) {
            lo = mid;
        }
        else if(v[mid] > key) {
            hi = mid;
        }
        else {
            return mid;
        }
    }
}


int main() {
    FastIO;

    cout << BinarySearch(104);
}
```

---

## **lower_bound**
정렬된 배열의 원소 중 key보다 같거나 큰 첫 번째 원소의 index를 return하는 함수이다. 배열의 원소가 모두 key보다 작은 경우, 마지막 index + 1을 return한다.

배열의 모든 원소가 key보다 큰 경우 [F(-1), T(0), T(1), T(2) ... T(9)]이고, key보다 작은 경우 [F(0), F(1), F(2) ... F(9), T(10)]이 되므로 lo, hi를 각각 v.begin() - 1, v.end()로 설정해야 한다. upper_bound도 마찬가지다.

### **코드**
```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
using namespace std;

vector<int> v{2, 8, 44, 56, 87, 99, 104, 143, 226, 786};


int LowerBound(int key) {
    int lo = -1, hi = v.size();

    while(lo + 1 < hi) {
        int mid = (lo + hi) / 2;

        if(v[mid] < key) {
            lo = mid;
        }
        else if(v[mid] >= key) {
            hi = mid;
        }
    }

    return hi;
}


int main() {
    FastIO;

    cout << LowerBound(104);
}
```

---

## **upper_bound**
정렬된 배열의 원소 중 key보다 큰 첫 번째 원소의 index를 return하는 함수이다. 배열의 원소가 모두 key보다 작은 경우, 마지막 index + 1을 return한다.

### **코드**
```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
using namespace std;

vector<int> v{2, 8, 44, 56, 87, 99, 104, 143, 226, 786};


int UpperBound(int key) {
    int lo = -1, hi = v.size();

    while(lo + 1 < hi) {
        int mid = (lo + hi) / 2;

        if(v[mid] <= key) {
            lo = mid;
        }
        else if(v[mid] > key) {
            hi = mid;
        }
    }

    return hi;
}


int main() {
    FastIO;

    cout << UpperBound(104);
}
```
