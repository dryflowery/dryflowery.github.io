---
title: 매개 변수 탐색(Parametric Search)
date: 2024-05-06 00:26:00 +09:00
categories: [Algorithm]
use_math: true
---


## **최적화 문제를 결정 문제로 변환**
파라메트릭 서치는 "최적화 문제"를 "결정 문제"로 바꾸고, 이분 탐색을 이용해서 푸는 알고리즘이다. 
<br>
종만북의 "최적화 문제 결정 문제로 바꿔 풀기" 파트에서 파라메트릭 서치를 설명하면서 딱히 이름이 붙어 있지 않다고 하는 걸 보니 꽤 최근에 네이밍이 된 듯 하다. 최적화 문제와 결정 문제의 정의는 다음과 같다.

>
최적화 문제: 가능한 모든 솔루션 중에서 최상의 솔루션을 찾는 문제이다. optimize()로 표현한다.
<br>
결정 문제: 예 혹은 아니오 형태의 답만이 나오는 문제이다. decision()으로 표현한다.
>

---

## **최적화 문제를 결정 문제로 변환하는 이유**
```cpp
int optimize() {
    // solve
    return ret;
}

bool decision() {
    // solve
    return solve() <= M;
}
```

위 코드에서 알 수 있듯이, 최악의 경우에도 결정 문제는 최적화 문제의 결과에 따라 true or false를 반환하면 된다.



따라서 같은 문제를 두 가지 형태로 표현할 경우, 결정 문제의 시간 복잡도는 최적화 문제의 시간 복잡도보다 항상 같거나 작다. 

---

## **문제**

[13397번: 구간 나누기 2](https://www.acmicpc.net/problem/2110)는 대표적인 파라메트릭 서치 문제이다. 먼저 위 문제를 "최적화 문제"와 "결정 문제"로 바꾸면 다음과 같다.

>
optimize(v, M): 배열 v를 M개 이하의 구간으로 나누었을 때, 구간의 점수의 최댓값의 최솟값은 얼마인가?
<br>
decision(v, M, mid): 배열 v의 구간의 점수의 최댓값의 최솟값이 mid이하인 방법의 총 구간의 개수가 M개 이하인가?
>

decision(v, M, mid)에서 v와 M은 이미 주어져 있으므로, 우리는 조건을 만족하는 mid를 "이분 탐색"을 이용하여 구하면 된다.

---

### **lo, hi값 설정하기**

이분 탐색을 통해 최적의 mid를 찾기 위해선 lo와 hi를 적절히 설정해야 한다. mid가 될 수 있는 값들의 집합을 mid_arr이라고 하고, mid_arr의 분포가 [T, T, T ... F, F]인지 [F, F, F, ... T, T]인지 생각해보자.

mid_arr의 최솟값은 0이다. 구간 내의 최댓값 - 최솟값은 음수가 될 수 없기 때문이다. 또한 최댓값은 v전체에서의 최댓값 - 최솟값이다. 문제의 조건에 따르면 9999이다.

먼저 mid가 최솟값(0)인 경우, M은 N과 같아야 한다. 원소의 개수만큼 구간을 나눠야 구간의 점수가 0이 나올 수 있기 때문이다.
다음으로 mid가 최댓값(9999)인 경우, M이 1이상이면 항상 성립한다. 임의로 나눠진 구간 내에서 또 구간을 나눌 경우 최댓값은 같거나 작아지고 최솟값은 같거나 커지기 때문에 구간의 점수는 항상 작아진다.

M의 범위가 $1 \leq M \leq N$이기 때문에 mid_arr은 [F, F, F, ... T, T]의 분포를 가지고 hi를 반환하는 것을 알 수 있다. 이제 lo, hi값을 설정해보자.

[F, F, F, ... T, T]분포에서 hi를 반환하므로 hi는 9999로 설정하면 된다. lo가 9998, hi가 9999일 때 hi를 반환하면 되기 때문이다. 그렇다면 lo도 0으로 설정하면 될까? 

mid_arr의 모든 원소가 T일 때, 실제로는 0을 반환해야 한다. 하지만 lo가 0인 경우 lo는 0, hi는 1을 가리키기 때문에 hi를 반환하면 WA를 받는다. 그래서 lo는 -1로 설정해야 한다. 그래야 위와 같은 경우에도 lo는 -1, hi는 0을 가리키므로 올바른 mid값을 반환할 수 있다.

---

### **코드**

```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
#define pb push_back
using namespace std;

int N, M, ans;
vector<int> v;


void Print() {
    cout << ans;
}


bool Possible(int mid) {
    int cur_max = v[0], cur_min = v[0], cnt = 0;

    for(int i = 1; i < N; i++) {
        cur_max = max(cur_max, v[i]);
        cur_min = min(cur_min, v[i]);


        if(cur_max - cur_min > mid) {
            cnt++;
            cur_max = cur_min = v[i];
        }
    }
    if(cur_max - cur_min <= mid) {
        cnt++;
    }

    if(cnt <= M) {
        return true;
    }
    else {
        return false;
    }
}


void Solve() {
    int lo = -1, hi = 9999;

    while(lo + 1 < hi) {
        int mid = (lo + hi) / 2;

        if(Possible(mid)) {
            hi = mid;
        }
        else {
            lo = mid;
        }
    }

    ans = hi;
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
    Print();
}
```


---

## **여담**
파라메트릭 서치에서 lo, hi값 설정하는 법

1. 정답 배열의 분포를 구한다. 
2. lo를 반환하는지 hi를 반환하는지 구한다.
3. 1, 2번의 결과에 알맞게 lo, hi값을 설정한다.