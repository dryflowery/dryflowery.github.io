---
title: 플로이드-워셜(Floyd-Warshall)
date: 2024-07-18 00:03:00 +09:00
categories: [Algorithm]
use_math: true
---

## **플로이드-워셜 알고리즘**
앞선 다익스트라와 벨만-포드 알고리즘은 한 정점에서 다른 모든 정점까지의 최단 경로를 구하는 알고리즘이었다. 그렇다면 모든 정점에 대하여 다른 모든 정점까지의 최단 경로를 구하려면 어떻게 해야할까? 

음수 간선의 존재 여부에 따라 모든 정점에 대해서 다익스트라 혹은 벨만-포드 알고리즘을 사용할 수도 있겠지만, 플로이드 워셜 알고리즘을 사용해서 구할 수도 있다.

플로이드-워셜 알고리즘은 그래프에서 가능한 모든 정점 쌍의 최단 거리를 구하는 알고리즘으로, 구현이 굉장히 간편하다는 장점이 있다. 

---

### **진행 방식**
>
1. Input Case에 따라 dist를 초기화 한다. 두 정점이 연결되어 있지 않은 경우, 해당 dist는 INF이다. 
2. dist(i, j)를 정점 i와 j의 현재 최단 경로라고 할 때, dist(i, j)를 dist(i, j)와 dist(i, k) + dist(k, j)중 작은 쪽으로 갱신한다.
3. 모든 정점에 대해서 k, i, j를 바꿔가며 2번을 반복한다
>

진행 방식은 위와 같다. 모든 정점을 경유점으로 현재 i에서 j로 가는 경로보다 k를 경유해서 가는 경로 **(i에서 k로, k에서 j로)** 가 더 짧다면, 최단 경로를 갱신해주면 된다.


플로이드-워셜 알고리즘에는 두 dist를 더하는 연산이 있으므로, INF값을 정의할 때 오버플로우가 발생하지 않도록 주의해야 한다.

---

### **구현**
[11404번: 플로이드](https://www.acmicpc.net/problem/11404)의 코드이다.

```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
#define ll long long
#define INF 1010101010
using namespace std;

int N, M;
int dist[100][100];

void Output() {
    for(int i = 0; i < N; i++) {
        for(int j = 0; j < N; j++) {
            if(dist[i][j] == INF) {
                cout << "0" << " ";
            }
            else {
                cout << dist[i][j] << " ";
            }
        }

        cout << '\n';
    }
}


void Solve() {
    // Floyd-Warshall algorithm
    for(int k = 0; k < N; k++) {
        for(int i = 0; i < N; i++) {
            for(int j = 0; j < N; j++) {
                if(dist[i][j] > dist[i][k] + dist[k][j]) {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }
}


void Input() {
    cin >> N >> M;

    // Initialization distance
    for(int i = 0; i < N; i++) {
        for(int j = 0; j < N; j++) {
            if(i == j) {
                dist[i][j] = 0;
            }
            else {
                dist[i][j] = INF;
            }
        }
    }

    // Input
    for(int i = 0; i < M; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        a--; b--;

        dist[a][b] = min(dist[a][b], c);
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

### **시간 복잡도**
모든 정점에 대해서 출발, 도착, 경유 총 3번의 반복문을 돌기 때문에 시간 복잡도는 $O(|V|^3)$이다. 더 최적화 할 수 있는 방법도 있다.

---

## **플로이드-워셜 알고리즘의 활용**
플로이드-워셜 알고리즘을 활용하여 모든 정점 쌍에 대해 두 정점을 잇는 경로의 존재 여부를 알 수 있다.

[10159번: 저울](https://www.acmicpc.net/problem/10159)은 두 물건 사이의 대소 관계가 주어질 때, 모든 물건에 대해 해당 물건과 비교 결과를 알 수 없는 물건의 개수를 출력하는 문제이다.

W(N)을 N번 물건의 무게라고 가정하자. **{W(1) > W(2), W(2) > W(3)}** 이라는 관계가 주어지면, 1번 물건은 3번 물건과 직접적인 관계가 없더라도 두 관계를 통해 **W(1) > W(3)** 이라는 것을 알 수 있다.

물건을 정점으로, 관계를 간선으로 생각하면 그래프로 표현할 수 있다. 문제의 예제 1번을 그래프로 만든 예시이다. 간선의 방향은 {무거운 물건 -> 가벼운 물건}이다. 간선의 방향은 {가벼운 물건 -> 무거운 물건}으로 해도 상관 없다.

![](/assets/img/algorithm/floyd warshall/graph.png)

위의 그래프를 보면 두 정점 사이에 경로가 존재한다는 것이 무슨 의미인지 알 수 있을 것이다. 두 정점 사이에 경로가 존재한다는 것은 두 물건의 대소 관계를 비교할 수 있다는 뜻이다.

두 물건 사이의 대소 관계를 비교할 수 있다면 두 정점 사이의 최단 경로는 1, 비교할 수 없다면 INF로 dist를 초기화 한 후 플로이드-워셜 알고리즘을 사용한다. 정점 u, v에 대해 dist[u][v], dist[v][u] 둘 중 하나라도 INF가 아니면 두 정점 사이에 경로가 존재한다는(대소 관계를 비교할 수 있다는)뜻이다.

비슷한 문제로 [11562번: 백양로 브레이크](https://www.acmicpc.net/problem/11562)도 있다.

---

## **구현**
[10159번: 저울](https://www.acmicpc.net/problem/10159)의 코드이다.

```cpp
#include <bits/stdc++.h>
#define FastIO ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
#define ll long long
using namespace std;

int N, M;
vector<int> graph[100];
int dist[100][100];

void Output() {
    for(int i = 0; i < N; i++) {
        int cnt = 0;

        for(int j = 0; j < N; j++) {
            if(i == j) {
                continue;
            }

            if(dist[i][j] == 0 && dist[j][i] == 0) {
                cnt++;
            }
        }

        cout << cnt << '\n';
    }
}


void Solve() {
    for(int k = 0; k < N; k++) {
        for(int i = 0; i < N; i++) {
            for(int j = 0; j < N; j++) {
                if(dist[i][k] == 1 && dist[k][j] == 1) {
                    dist[i][j] = 1;
                }
            }
        }
    }
}


void Input() {
    cin >> N >> M;

    for(int i = 0; i < M; i++) {
        int u, v;
        cin >> u >> v;
        u--; v--;

        graph[u].push_back(v);
        dist[u][v] = 1;
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