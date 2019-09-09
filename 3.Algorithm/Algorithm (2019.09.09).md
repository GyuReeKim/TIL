# Algorithm (2019.09.09)

## 이진트리

### 이진트리 정보

5 4 # 노드의 개수 V, 간선의 개수 E

2 1 2 4 4 3 4 5



### 부모를 인덱스로 자식번호를 저장

```python
for i: 1 -> N
    read p, c;
    if ch1[p] == 0:
    	ch1[p] = c
    else:
        ch2[p] = c
```



### 자식 번호를 인덱스로 부모를 저장

```python
for i: 1 -> N
    read p, c;
    pa[c] = p
```



첫번째는 루트를 찾는 것으로 부터 시작한다.



### 조상 노드 찾기

```python
# 자식 c: 0 1 2 3 4 5
# 부모 a: 0 0 1 1 3 3

c = 5;
while a[c] != 0: # 루트인지 확인
	c = a[c]
	print(c)
```



### 이진 트리 순회1

> 검사를 하고 진입

```python
DLR(1)
	Visit();
    if Left:
        DLR(2)
    else:
        DLR(3)
```

```python
DLR(2)
	Visit();
    if Left:
        DLR()
    else:
        DLR()
```

```python
DLR(3)
	Visit();
    if Left:
        DLR(4)
    else:
        DLR(5)
```

```python
DLR(4)
	Visit();
    if Left:
        DLR()
    else:
        DLR()
```

```python
DLR(5)
	Visit();
    if Left:
        DLR()
    else:
        DLR()
```



### 이진 트리 순회2

> 진입 후 검사

```python
f(1):
	if(1):
		V(1)
		f(2)
		f(3)
		
f(2):
	if(2):
		V(2)
		f(0)
		f(0)
		
f(0):
	if(0):
	
f(0):
	if(0):
	
f(3):
	if(3):
		V(3)
		f(4)
		f(5)
		
f(4):
	if(4):
		V(4)
		f(0)
		f(0)
		
f(0):
	if(0):
	
f(0):
	if(0):
	
f(5):
	if(5):
		V(5)
		f(0)
		f(0)
		
f(0):
	if(0):

f(0):
	if(0):
```



### 깊이

```python
f(1):
	if(1):
		cnt +1
		f(2)
		f(3)
```



### 순회

```python
# tree

# 13
# 1 2 1 3 2 4 3 5 3 6 4 7 5 8 5 9 6 10 6 11 7 12 11 13

# 전위순회
def preorder(n):
    if n > 0:
        print(n, end=' ')
        preorder(ch1[n])
        preorder(ch2[n])

# 중위순회
def inorder(n):
    if n > 0:
        inorder(ch1[n])
        print(n, end=' ')
        inorder(ch2[n])

# 후위 순회
def postorder(n):
    if n > 0:
        postorder(ch1[n])
        postorder(ch2[n])
        print(n, end=' ')

# n의 조상 출력하기
def f(n):
    while par[n] != 0: # n의 부모가 있으면
        print(par[n], end=' ')
        n = par[n] # 부모를 새로운 자식으로 해서 부모의 부모를 찾으러 감


V = int(input()) # 간선 수 = V - 1
E = V - 1
t = list(map(int, input().split()))

ch1 = [0] * (V+1) # 부모를 인덱스로 자식 저장
ch2 = [0] * (V+1)
par = [0] * (V+1) # 자식을 인덱스로 부모 저장

for i in range(V-1):
    p = t[2*i]
    c = t[2*i+1]
    if ch1[p] == 0: # 아직 ch1 자식이 없으면
        ch1[p] = c
    else:
        ch2[p] = c
    par[c] = p

preorder(1)
print()
inorder(1)
print()
postorder(1)
print()
f(5)
print()
```







## 인접행렬을 이용한 그래프 저장

### 무향 그래프

```
read V, E
for i: 1 -> E:
	read n1, n2
	M[n1][n2] = 1
	M[n2][n1] = 1
```



### 유향 그래프

```
read n1, n2
M[n1][n2] = 1
```



### 가중치 그래프

```
read n1, n2, w
M[n1][n2] = w
```



## 마을 문제 (A형)

```python
# 그래프에 속한 모든 노드를 탐색...
adj = [][] # 인접행렬
visited = []
A = [1, 2, 4] # 탐색 전 생성..
dfs(n)
	visit(n) # 방문한 노드에 대한 처리
	visited[n] = 1 # 방문 표시
	for i: 1 -> N # 인접하고 방문하지 않은 노드로 이동
		if i in A and adj[n][i] == 1 and visited[i] == 0:
            dfs(i)
# A에 속한 모든 노드를 방문했는지 확인
```

