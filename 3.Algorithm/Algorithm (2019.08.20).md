# Algorithm (2019.08.20)

## 백만장자 프로젝트

```
7 2 4 6 3 2 5
        3 2 5 : 6의 오른쪽 숫자들. 3과 5 중 큰 수(3과 3의 오른쪽 숫자 중 가장 큰 수)
          2 5 : 3의 오른쪽 숫자들. 가장 큰 수 5
          
Ai : A[i+1]~A[N-1] 중 가장 큰 수 == A[i+1]과 A[i+2]~A[N-1]의 최대값 중 큰 수
```



## 반복문자 지우기

```python
T = int(input())
for tc in range(1, T+1):
    txt = input()
    s = list()
    s.append(txt[0])
    for i in range(1, len(txt)):
        # 스택이 비어있거나, 스택의 맨 위 글자와 다르면 push(txt[i], 같으면 pop()
        if len(s) != 0 and s[-1] != txt[i]:
            s.append(txt[i])
        else:
            s.pop()
    print('#{} {}'.format(tc, len(s)))
```



## 후위 표기법



## 백트래킹

문제를 풀 때 그림을 그려가며 풀어야 한다. 필요없는 것을 버려내는 작업이 백트래킹이다.



알고리즘에서는 줄이는 것이 중요하다.



## Forth

```python
def find():
    s = []
    for i in range(len(code)):
        if code[i] == '+' or code[i] == '-' or code[i] == '*' or code[i] == '/':
            if len(s) >= 2:
                op2 = int(s.pop())
                op1 = int(s.pop())
                if code[i] == '+':
                    ...
```



## 미로 찾기

```python
# 미로찾기

def f(i, j): # 이차원 이웃 참고
    di = [0, 1, 0, -1]
    dj = [1, 0, -1, 0]
    global maze
    global N
    # if maze[i][j] == '1': # 벽이면 방문 안함
    #     return 0
    if maze[i][j] == '3': # 목적지면
        return 1
    else:
        maze[i][j] = '1' # 방문 표시, 벽으로 바꿈
        # 이동할 좌표 생성
        for k in range(4): # 벽에 둘러쌓이지 않았기 때문에 유효범위 검사를 해야한다. 네 방향을 전부 가본다.
            ni = i + di[k]
            nj = j + dj[k]
            if ni >= 0 and ni < N and nj >= 0 and nj < N:
                if maze[ni][nj] != '1': # 벽이 아니면 방문. 만약 통로(0)이면 방문이라고 설정한다면 도착지에 도착하지 못한다.
                    if f(ni, nj) == 1: # 진행방향에서 목적지를 찾은 경우
                        return 1
        return 0 # 현재 위치에서 갈 수 있는 방향에서 목적지를 찾지 못함. 이전의 다른 방향 탐색.

T = int(input())
for tc in range(1, T+1):
    N = int(input())
    maze = [list(input()) for _ in range(N)]
    startI = 0
    startJ = 0
    for i in range(N):
        for j in range(N):
            if maze[i][j] == '2':
                startI = i
                startJ = j
    # (i, j)부터 탐색을 시작한다.
    print('#{} {}'.format(tc, f(startI, startJ)))
```


```python
for i in range(N):
    if 2 in maze[i]:
        sRow = i
        sCol ...
```

```python
def find():
    dRow = [0, 1, 0, -1]
    dCol = [1, 0, -1, 0]
    s = []
    s.append([sRow, sCol]) # 입구로 이동
    maze[sRow][sCol] = 1 # 방문 표시
    while len(s) != 0:
        n = s.pop() # 이동할 칸 좌표를 꺼내고
        for i in range(4): # 주변 좌표 계산
            nRow = n[0] + dRow[i]
            nCol = n[1] + dCol[i]
            if nRow >= 0 and nRow < N and nCol >= 0 and nCol < N: # 미로 내부인지 확인
                if maze[nRow][nCol] == 3: # 목적지인 경우 1 반환
                    return 1
                elif maze[nRow][nCol] == 0: # 갈 수 있는 곳 저장
                    s.append([nRow, nCol])
                    maze[n[0]][n[1]] = 1
    return 0 # 출구에 가지 못하고 모든 칸 방문
```



### 파스칼의 삼각형

```python
for i: 0 -> N-1 # 행 번호
    for j: 0 -> i # 열 번호
        if j == 0 or i == j # 가장자리
        	arr[i][j] = 1
        else
        	arr[i][j] = arr[i-1][j-1] + arr[i-1][j]
            
```

