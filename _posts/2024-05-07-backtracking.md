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
[9663번: N-Queen](https://www.acmicpc.net/problem/9663)은 가장 유명한 백트래킹 문제이다. 






### **코드**

```cpp
```