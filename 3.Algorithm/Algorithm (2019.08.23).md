# Algorithm (2019.08.23)

## 달팽이

수평 -> 수직: 길이 감소

인덱스가 증가하는 방향

+1



수직 -> 수평

인덱스가 감소하는 방향

-1



NxN 달팽이 배열



초기값 : 이동거리 K = N, 이동방향 dir = 1



모든 칸이 채워질 때까지

(1) 수평 이동 (+1)

(1-2) 이동거리 1 감소 K = K-1

(2) 수직 이동 (+1) 후 이동 방향 반전. dir *= -1

(1) 반복



```python
import pprint
T = int(input())
for tc in range(1, T+1):
    c = 1
    N = int(input())
    k = N
    dir = 1
    arr = [[0]*N for _ in range(N)]

    i = 0 # 시작 칸의 인덱스
    j = -1 # 현재 위치로 부터 k번 이동해야 하므로

    while(1):
        # 수평이동
        for h in range(k):
            j += dir
            arr[i][j] = c
            c += 1
        k -= 1 # 이동 거리 감소
        if k == 0: # 이동 거리가 0이면 중단
            break
        # 수직이동
        for v in range(k):
            i += dir
            arr[i][j] = c
            c += 1
        dir *= -1 # 수직 -> 수평으로 바뀔때 인덱스 증감 바꾸기
    # pprint.pprint(arr, indent=4, width=50)

    for i in range(N):
        for j in range(N):
            arr[i][j] = str(arr[i][j])
    # print(arr)
    print('#{}'.format(tc))
    for i in range(N):
        print(' '.join(arr[i]))
```

