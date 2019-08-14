# Algorithm (2019.08.14)

## 문제 풀이

### View (조망권 확인 구간)

```python
가로 길이 : N
건물 높이 : h = [0, 0, 3, 5, 2, 0, 0]
s = 0
for i : 2 -> N-3
	if h[i] > h[i-1] and h[i] > h[i-1] and h[i] > h[i+1] and h[i] > h[i+2]
    	diff = h[i]-h[i-1]
        if diff > h[i]-h[i-2]
        	diff = h[i]-h[i-2]
        if diff > h[i]-h[i+1]
        	diff = h[i]-h[i+1]
        if diff > h[i]-h[i+2]
        	diff = h[i]-h[i+2]
        s = s + diff
```



### Flatten

덤프 횟수가 남아있는 동안,

​	최고점과 최저점의 차이가 1 이내면 중단,

​	아니면 최고점에서 1을 빼고 최저점에 1을 더함.



연습) 평탄화 하는데 필요한 최소 덤프 횟수는? 평탄화는 최고점과 최저점의 차이가 1 이내인 경우를 말함.

(주어진 덤프 횟수는 무시)

```python
for tc in range(1, 11):
    dump = int(input())
    box = list(map(int, input().split()))
    # for i in range(0, dump):
    diff = 0
    while dump > 0:
        maxIdx = 0
        minIdx = 0
        for i in range(1, 100):
            if box[maxIdx] < box[i]:
                maxIdx = i
            if box[minIdx] > box[i]:
                minIdx = i
        diff = box[maxIdx] > box[minIdx]
        if diff <= 1:
            break
        else:
            box[maxIdx] = box[maxIdx] - 1
            box[minIdx] = box[minIdx] + 1
    dump = dump - 1
    print('#{} {}'.format(tc, diff))
```



## 2차원 배열

### 행 우선 순회

```python
# i 행의 좌표
# j 행의 좌표
for i in range(len(Array)):
    for j in range(len(Array[i])):
        Array[i][j]
```



### 행 우선 순회

```python
# i 행의 좌표
# j 행의 좌표
for j in range(len(Array[0])):
    for i in range(len(Array)):
        Array[i][j]
```



### 지그재그 순회

```python
# 짝수 홀수를 나워서 하기
```



### 전치 행렬

대각선으로 나눈 후 왼쪽 혹은 오른쪽을 기준으로 바꿔준다.

```python
# i : 행의 좌표, len(arr)
# j : 열의 좌표, len(arr[0])
arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]] # 3*3 행렬
for i in range(3):
    for j in range(3):
        if i < j:
            arr[i][j], arr[j][i] = arr[j][i], arr[i][j]
```



## 2차원 배열 문제

### 5x5 2차원 배열

5x5인 모든 칸의 이웃에 대해 조사하는 경우 모든 칸의 범위는

```
for i : 0 -> 4
	for j : 0 -> 4
		if j+1 <= 4 # 오른쪽 칸이 존재하면
		if i+1 <= 4 # 아래쪽 칸이 존재하면
		if j-1 >= 0 # 왼쪽 칸이 존재하면
		if i-1 >= 0 # 위쪽 칸이 존재하면
```



​				3(i-1, j+0)

2(i+0, j-1)	(i, j)	0(i+0, j+1)

​				1(i+1, j+0)



```python
di = [0, 1, 0, -1]
dj = [1, 0, -1, 0]
i, j의 이웃 ni, nj는...
NxN에서
for k : 0 -> 3
	ni = i + di[k]
	nj = j + dj[k]
	if(ni >= 0 and ni < N and nj >= 0 and nj < N)
	# 존재하는 이웃인지 검사 
```



## 부분집합

[1, 2, 3]



[]

[1] [2] [3]

[1, 2] [1, 3] [2, 3]

[1, 2, 3]



1이 포함되는 경우

[1]

[1, 2]

[1, 3]

[1, 2, 3]



## 순차 검색

### 정렬 되어있지 않은 경우

7 2

4 9 11 23 2 19 7

```python
def f(n, v, arr):
    for idx in range(0, n):
        if arr[idx] == v: # 키 값을 찾으면
            return idx
    return -1 # 배열 안에서 키 값을 찾지 못하면


# 개수 N, 키 V
N, V = map(int, input().split())
arr = list(map(int, input().split()))
r = f(N, V, arr)
print(r)
```



### 정렬 되어있는 경우

비효율적일 수 있음



## 이진 검색

### 검색 과정

1. 자료의 중앙에 있는 원소를 고른다.
2. 중앙 원소의 값과 찾고자 하는 목표 값을 비교한다.
3. 목표 값이 중앙 원소의 값보다 작으면 자료의 왼쪽 반에 대해서 새로 검색을 수행하고, 크다면 자료의 오른쪽 반에 대해서 새로 검색을 수행한다.
4. 찾고자 하는 값을 찾을 때까지 위의 과정을 반복한다.



```python
# 개수 N
left = 0
right = N - 1
while left <= right:
# 탐색 구간에 1개의 원소가 남을 때까지 반복한다.
	center = (left + right) // 2
	if key == arr[center]:
		return 1
	elif key > arr[center]:
	# 작은 구간은 버림
		left = center + 1
	elif key < arr[center]:
	# 큰 구간은 버림
		right = center -1
# left > right, 1개만 남은 구간에서도 못찾으면
return -1
```



