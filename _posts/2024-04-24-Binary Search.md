---
title: 이분 탐색(Binary Search)
date: 2024-04-24 16:08:00 +09:00
categories: [Algorithm]
use_math: true
---

## **이분 탐색**
이분 탐색은 $O(NlogN)$의 시간복잡도를 가지는 탐색 알고리즘으로, 원소들이 정렬된 상태에서만 사용할 수 있다는 특징이 있다. 알고리즘 진행 방식은 다음과 같다.

>
첫 번째 원소를 low, 마지막 원소를 high로 설정한다.
1. mid = (low + high) / 2
2. mid가 key보다 작은 경우, low = mid
3. mid가 key보다 큰 경우, high = mid

mid값이 key와 같아질 때까지 반복한다.
>


![](/assets/img/algorithm/binary search/binary search 1.png)
![](/assets/img/algorithm/binary search/binary search 2.png)
![](/assets/img/algorithm/binary search/binary search 3.png)




---

## **이분 탐색 구현**



----

## **Parametric Search**