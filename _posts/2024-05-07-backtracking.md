---
title: 백트래킹(Backtracking)
date: 2024-05-07 12:07:00 +09:00
categories: [Algorithm]
use_math: true
---

## **백트래킹**
백트래킹은 promising한 상태(state)만 탐색하는 탐색 알고리즘이다. 여기서 promising이란 **유망한**이라는 의미로, 현재 상태에서 해를 찾을 수 있는 가능성이 있다는 의미이다. 쉽게 말해서 현재 상태가 문제의 제약 조건을 전부 만족할 경우 promising하다고 말한다.

백트래킹은 기본적으로 완전 탐색이다. 하지만 둘의 결정적인 차이점이 있다. 완전 탐색은 상태의 promising여부와 관계 없이 모든 상태를 탐색한다. 하지만 백트래킹은 현재 상태가 promising한 경우 탐색을 계속하고, non-promising한 경우 탐색을 중단하고 **이전 상태로 되돌아간다(pruning).** 

---

![](/assets/img/algorithm/backtracking/brute force sst.png)
<br>
{1, 2, 3, 4}에서 $_4P_3$을 구하는 문제가 있을 때, 순열의 첫 번째 원소는 1이고, 두 번째 원소와의 차이가 2이하여야 한다는 제약 조건이 붙어있다고 가정하자. 완전 탐색의 sst(state space tree)는 위와 같다. 순열이 {1, 4}가 된 순간 이미 non-promising하지만, 일단 모든 상태에 대해 탐색을 진행한다. 그리기 힘들어서 생략했지만 2, 3, 4로 시작하는 순열도 sst에 추가되어야 한다.

---

![](/assets/img/algorithm/backtracking/backtracking sst.png)
<br>
백트래킹의 sst는 위와 같다. {1, 4}에서 이미 문제의 제약 조건을 만족하지 않으므로 이후 상태에 대해 탐색을 진행하지 않는다.

[N과 M 시리즈](https://www.acmicpc.net/workbook/view/2052)가 대표적인 백트래킹 문제들이다.

---

## **백트래킹을 재귀 DFS로 구현하는 이유**
백트래킹의 두 가지 주요한 특징이 있다.

1. 현재 상태에서 다음 상태로 탐색을 진행한다.
2. non-promising한 상태일 경우, **이전 상태로 되돌아간다.**

1, 2번 모두 **이전 상태**에 대한 정보를 저장해야(혹은 가지고 있어야)한다는 특징이 있다. 다음 상태로 넘어갔을 때 이전 상태에 대한 정보가 있어야 연산을 진행할 수 있고, 이전 상태로 되돌아가기 위해서도 이전 상태의 정보를 저장해놔야 하기 때문이다.

이 때 bfs 혹은 스택 dfs로 구현하면 큐, 스택에 들어가는 각각의 데이터에 모든 정보가 포함되어 있어야 한다. 전역 변수로 구현할 수도 있지만, 메모리도 낭비되고 구현도 복잡하다.

재귀 dfs로 구현하면 다음 상태로 넘어갈 경우 매개 변수로 현재 상태의 정보를 넘겨주면 되고, 현재 함수가 종료되는 경우 자동으로 이전 상태로 되돌아 가기 때문에 신경 쓸 필요도 없다. bfs, 스택 dfs에 비해 속도가 느린 것도 아니다.

백트래킹에서 dfs가 가지는 의미는 백트래킹을 편하고 효율적으로 구현하기 위한 도구 그 이상도 이하도 아니다.

---

## **문제**
[9663번: N-Queen](https://www.acmicpc.net/problem/9663)은 가장 유명한 백트래킹 문제다. N * N 크기의 체스판에 퀸 N개를 서로 공격할 수 없게 놓는 문제이다.

---

### **아이디어**
N의 범위가 $1 \leq N \leq 14 $이므로 브루트 포스로 풀면 경우의 수는 <sub>196</sub>$C$<sub>14</sub> = 880530516383349192480개이다. 백트래킹을 사용해서 적당히 pruning해주자.

---

퀸이 서로 공격할 수 있는 경우는 총 3가지다.

1. 서로 다른 퀸이 같은 행에 존재하는 경우
2. 서로 다른 퀸이 같은 열에 존재하는 경우
3. 서로 다른 퀸이 같은 대각선상에 존재하는 경우

$col[r]$이라는 변수를 정의하자. $col[r] = c$는 r행의 퀸이 c열에 존재한다는 뜻이다. col 변수를 사용해서 각 행마다 하나의 퀸만 놓으면 promising여부를 판단할 때 열과 대각선만 체크해주면 된다. 

같은 상태(같은 단계의 dfs)에서는 퀸의 열만 바꿔서 배치하고, 다음 행을 매개변수로 넘겨주면서 백트래킹을 진행한다.

col 변수를 사용해서 열, 대각선만 체크하는 아이디어만 알면 구현 난이도는 어렵지 않다.

---

### **코드**

```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
using namespace std;

int N ,ans;
int col[15];


void Output() {
    cout << ans;
}


bool Promising(int row) {
    // 열 체크
    for(int i = 0; i < row; i++) {
        if(col[i] == col[row]) {
            return false;
        }
    }

    // 대각선 체크
    for(int i = 0; i < row; i++) {
        if(abs(i - row) == abs(col[i] - col[row])) {
            return false;
        }
    }

    return true;
}


void Dfs(int row) {
    // 모든 행에 퀸을 놓는데 성공하면 Answer++
    if(row == N) {
        ans++;
        return;
    }

    // 같은 상태(같은 단계의 dfs)에서는 해당 행에 존재하는 퀸의 열만 바꿔서 배치
    for(int i = 0; i < N; i++) {
        col[row] = i;

        if(Promising(row)) {
            Dfs(row + 1);
        }
    }
}


void Solve() {
    Dfs(0);
}


void Input() {
    cin >> N;
}


int main() {
    FastIO;

    Input();
    Solve();
    Output();
}
```
