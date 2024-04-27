---
title: 구현(Implementation)
date: 2024-04-27 18:16:00 +09:00
categories: [Algorithm]
use_math: true
---

## **구현 테크닉**
알고리즘을 구현하는데 도움이 되는 여러가지 테크닉들

---

## **상하좌우 탐색**
![](/assets/img/algorithm/implementation/search.png)
(x, y)를 기준으로 상하좌우 좌표를 생각해보자. x의 변화량은 [-1, 1, 0, 0]이고, y의 변화량은 [0, 0, -1, 1]이다. 이것을 dx, dy배열로 만들어서 상하좌우 좌표를 계산하는 테크닉이다. 2차원의 4방향뿐만 아니라, N차원의 M방향은 전부 표현 가능하다.


### **코드**
```cpp
int dx[4] = {-1, 1, 0, 0};
int dy[4] = {0, 0, -1, 1};
bool visited[r][c];

void search(int x, int y) {
    for(int i = 0; i < 4; i++) {
        int nx = x + dx[i];
        int ny = y + dy[i];

        if(nx < 0 || nx >= r || ny < 0 || ny >= c || visited[nx][ny]) {
            continue;
        }

        visited[nx][ny] = true;
    }
}
```


---

## **벽 유무 판별**



---

## **순열**
```cpp
int n, r; // init
int arr[n] // init
bool visited[n]; 

void nPr(int depth) {
    if(depth == r) {
        for(int i = 0; i < n; i++) {
            if(visited[i]) {
                cout << i << " ";
            }
        }
        cout << '\n';
    }

    for(int i = 0; i < n; i++) {
        if(visited[i]) {
            continue;
        }

        visited[i] = true;
        nPr(depth + 1);
        visited[i] = false;
    }
}
}
```

---

## **조합**
```cpp
int n, r; // init
int arr[n]; // init
bool visited[n]; 

void nCr(int depth, int last) {
    if(depth == r) {
        for(int i = 0; i < n; i++) {
            if(visited[i]) {
                cout << i << " ";
            }
        }
        cout << '\n';
    }

    for(int i = last; i < n; i++) {
        if(visited[i]) {
            continue;
        }

        visited[i] = true;
        nCr(depth + 1, i + 1);
        visited[i] = false;
    }
}
```