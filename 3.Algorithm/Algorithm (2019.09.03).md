# Algorithm (2019.09.03)

## 재귀 함수

```python
f(i, j, s)
	if 도착지면
    	if minV > s+arr[i][j]
        	minV = s+arr[i][j]
    else # 도착지가 아니면
    	# 아래로 이동
        if i+1 < N
        	f(i+1, j, s+arr[i][j])
        # 오른쪽으로 이동
        if j+1 < N
        	f(i, j+1, s+arr[i][j])
```

