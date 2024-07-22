---
title: 투 포인터(Two Pointer)
date: 2024-07-20 23:03:00 +09:00
categories: [Algorithm]
use_math: true
---

## **투 포인터 알고리즘**
투 포인터 알고리즘은 배열에서 각 원소를 가리키는 두 개의 포인터를 조작하여 $O(N)$에 문제를 해결하는 알고리즘이다. 

일반적으로 투 포인터는 두 가지 종류가 있다. 두 포인터가 양 끝에서 시작하는 경우와 한 점(일반적으로 시작점)에서 시작하는 경우다.

---

## **한 점에서 시작하는 경우**
[2003번: 수들의 합2](https://www.acmicpc.net/problem/2003)는 배열에서 연속된 원소의 합이 M이 되는 경우의 수를 구하는 문제이다.

이 문제를 naive하게 풀면 가능한 모든 쌍에 대해 합을 구해야 하므로 시간 복잡도는 $O(N^2)$이다. 하지만 투 포인터 알고리즘을 사용하면 $O(N)$에 풀 수 있다.

배열의 원소 두 개를 가리키는 포인터를 각각 **left, right (left $\leq$ right)** 라고 정의하고, left부터 right까지 원소의 합을 **sum(left, right)** 라고 정의하자. left, right 모두 배열의 첫 번째 원소를 가리킨 상태로 시작한다. 그리고 다음 두 가지 경우에 따라 알고리즘을 진행하면 된다.

첫 번째는 **sum(left, right) $\ge$ M**인 경우이다. 이 경우 left를 오른쪽으로 한 칸 이동한다. sum(left - 1, right)와 sum(left, right + 1)은 sum(left, right)보다 크기 때문에 left를 왼쪽으로 움직이거나 right를 오른쪽으로 움직일 이유가 없다. 또한 sum(left, right - 1)은 이전 단계에서 이미 구했기 때문에 right를 왼쪽으로 움직일 이유도 없다. 하지만 sum(left + 1, right)는 sum(left, right)보다 작기 때문에 M과 같거나 작아질 가능성이 있다. 다시 말해서 해가 될 가능성이 있다는 뜻이다. 

두 번째는 **sum(left, right) < M**인 경우이다. 이 경우에는 right를 오른쪽으로 한 칸 이동한다. 이유는 위와 같다. 

로직에 맞게 적절히 구현하면 AC를 받을 수 있다.

---

### **코드**
```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
#define ll long long
using namespace std;

int N, M, ans;
vector<int> v;

void Output() {
    cout << ans;
}


void Solve() {
    int left = 0, right = 0, sum = v[0];

    while(true) {
        if(sum == M) {
            ans++;
        }

        if((left == right) || (sum < M)) {
            if(right == N - 1) {
                break;
            }

            sum += v[++right];
        }
        else {
            sum -= v[left++];
        }
    }
}


void Input() {
    cin >> N >> M;

    for(int i = 0; i < N; i++) {
        int num;
        cin >> num;

        v.push_back(num);
    }
}


int main() {
    FastIO;

    Input();
    Solve();
    Output();
}
```

---

## **양 끝에서 시작하는 경우**
[2470번: 두 용액](https://www.acmicpc.net/problem/2470)은 N개의 원소가 주어졌을 때, 합이 0에 가장 가까운 두 원소를 찾는 문제이다.

이 문제도 위 문제와 동일하게 naive하게 풀면 $O(N^2)$이지만, 투 포인터 알고리즘을 사용하면 $O(N)$에 풀 수 있다.

left는 배열의 첫 번째 원소를, right는 마지막 원소를 가리킨 상태로 시작한다. sum(left, right)는 left와 right의 합이라고 정의하자. 이제 **배열을 정렬하고** 문제를 풀면 된다.

**sum(left, right) $\ge$ 0**인 경우에는 right를 왼쪽으로 옮기고, **sum(left, right) $\lt$ 0**인 경우에는 left를 오른쪽으로 옮겨주면 된다.

투 포인터 알고리즘을 모든 문제에서 사용할 수 있는 것은 아니다. 위의 문제들처럼 모든 sum(left, right)쌍을 다 확인하지 않아도 되는 경우에만 사용할 수 있다.

---

### **코드**

```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
#define ll long long
using namespace std;

int N;
vector<int> v;
int ans[2];

void Output() {
    cout << ans[0] << " " << ans[1];
}


void Solve() {
    sort(v.begin(), v.end());

    int left = 0, right = v.size() - 1, diff = 2000000001;

    while(left < right) {
        int sum = v[right] + v[left];

        if(abs(sum) < abs(diff)) {
            diff = abs(sum);
            ans[0] = v[left];
            ans[1] = v[right];
        }

        if(sum > 0) {
            right--;
        }
        else {
            left++;
        }
    }
}


void Input() {
    cin >> N;

    for(int i = 0; i < N; i++) {
        int num;
        cin >> num;

        v.push_back(num);
    }
}


int main() {
    FastIO;

    Input();
    Solve();
    Output();
}
```

---