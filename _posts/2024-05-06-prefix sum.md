---
title: 부분 합(Prefix Sum)
date: 2024-05-06 20:56:00 +09:00
categories: [Algorithm]
use_math: true
---

## **부분 합**
부분 합 알고리즘은 배열의 각 index에 대해 배열의 시작부터 현재 index까지의 원소의 합을 구하는 것이다.

---

## **1차원 구간 합**
![](/assets/img/algorithm/prefix sum/1.png)

배열 arr에 대한 구간 합 sum은 위 사진과 같고, 아래와 같이 정의할 수 있다.

$sum[i] = \displaystyle\sum_{j=0}^{i} arr[j]$

구현하는 방법은 다음과 같다.

$sum[i] = sum[i - 1] + arr[i]$ $(1 \leq i)$

---

부분 합 알고리즘을 사용하면 빠르게 arr[a] ~ arr[b] ($a \leq b$) 까지 원소의 합 pSum을 구할 수 있다. 또한 아래와 같이 정의할 수 있다. 

$pSum = \displaystyle\sum_{j=a}^{b} arr[j]$

구현하는 방법은 다음과 같다.

$pSum = sum[b] - sum[a - 1]$ $(1 \leq a)$

---

N개의 원소로 이루어진 배열의 구간 합을 구하는 쿼리가 M개 주어진다고 해보자. 브루트 포스로 풀 경우 시간복잡도가 $O(NM)$이지만, 부분 합 알고리즘을 이용하는 경우 초기화 $O(N)$, 쿼리당 $O(1)$의 시간복잡도를 가지므로 $O(N + M)$에 풀 수 있다.

---

### **문제**
[11659번: 구간 합 구하기 4](https://www.acmicpc.net/problem/24896)의 코드이다.


```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
#define pb push_back
using namespace std;

int N, M;
vector<int> v;
vector<int> sum;


void Output() {
    for(int i = 0; i < M; i++) {
        int start, end;
        cin >> start >> end;

        cout << sum[end] - sum[start - 1] << '\n';
    }
}


void Solve() {
    int sum = 0;

    sum.pb(0);

    for(int i = 0; i < N; i++) {
        sum += v[i];
        sum.pb(sum);
    }
}


void Input() {
    cin >> N >> M;

    for(int i = 0; i < N; i++) {
        int num;
        cin >> num;

        v.pb(num);
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

## **2차원 구간 합**
![](/assets/img/algorithm/prefix sum/2.png)

배열 arr에 대한 구간 합 sum은 위 사진과 같다. sum[x][y]는 arr[0][0]부터 arr[x][y]까지 포함된 모든 원소를 더하면 된다. 아래와 같이 정의할 수 있다.

$sum[x][y] =  \displaystyle\sum_{i=0}^{x}\displaystyle\sum_{j=0}^{y} arr[i][j]$

구현하는 방법은 다음과 같다.

$sum[x][y] = sum[x - 1][y] + sum[x][y - 1] - sum[x - 1][y - 1] + arr[x][y]$ $(1 \leq x, 1 \leq y)$

2차원 배열에서 포함-배제 원리를 이용한다고 생각하면 이해하기 편하다.

---

2차원 배열의 $arr[x_1][y_1] ~ arr[x_2][y_2]$ $(x1 \leq x_2, y1 \leq y_2)$ 까지의 구간 합 pSum는 아래와 같이 정의할 수 있다.

$pSum = \displaystyle\sum_{i=x_1}^{x_2}\displaystyle\sum_{j=y_1}^{y_2} arr[i][j]$

구현하는 방법은 다음과 같다.

$pSum = sum[x_2][y_2] - sum[x_1 - 1][y_2] - sum[x_2][y_1 - 1] + sum[x_1 - 1][y_1 - 1]$ $(1 \leq x_1, 1 \leq y_1)$

---

$N * M$ 크기의 2차원 배열에서 구간 합을 구하는 쿼리가 Q개 주어진다고 해보자. 브루트 포스로 구할 경우 시간 복잡도가 $O(NMQ)$지만, 부분 합 알고리즘을 이용하는 경우 초기화에 $O(NM)$, 쿼리당 $O(1)$의 시간 복잡도를 가지므로 총 시간 복잡도는 $O(NM + Q)$이다.

---

### **문제**
[11660번: 구간 합 구하기 5](https://www.acmicpc.net/problem/11660)의 코드이다.


```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
#define pb push_back
using namespace std;

int N, M;
int arr[1025][1025];
int sum[1025][1025];


void Output() {
    for(int i = 0; i < M; i++) {
        int x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;

        cout << sum[x2][y2] - sum[x1 - 1][y2] - sum[x2][y1 - 1] + sum[x1 - 1][y1 - 1] << '\n';
    }
}


void Solve() {
    for(int x = 1; x <= N; x++) {
        for(int y = 1; y <= N; y++) {
            sum[x][y] = sum[x - 1][y] + sum[x][y - 1] - sum[x - 1][y - 1] + arr[x][y];
        }
    }
}


void Input() {
    cin >> N >> M;

    for(int x = 1; x <= N; x++) {
        for(int y = 1; y <= N; y++) {
            cin >> arr[x][y];
        }
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