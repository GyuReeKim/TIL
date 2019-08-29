# Algorithm (2019.08.29)

## 활주로 건설

왼쪽 -> 오른쪽: 내리막 설치

오른쪽 -> 왼쪽: 내리막 설치



## 농장물 수확하기

```python
ct = N//2
s = 0
for i in range(N):
    if i <= ct:
        for j in range(N):
            if j >= ct-i and j <= ct+i:
                s += arr[i][j]
    else:
        for j in range(N):
            if j >= ct-(N-1-i) and j <= ct+i:
                s += arr[i][j]
```



## 붕어빵

```python
sell = [0]*11112 # 시간별 판매량
for i in t:
    sell[i] += 1
fish = 0
for i in range(11112):
    if i > 0 and i % M == 0: # i시간 재고 계산
        fish += K
    fish -= sell[i] # 시간별로 판매
    if fish < 0:
        break
        
if fish < 0:
    print('#{} Impossible'.format(tc))
else:
    print('#{} Possible'.format(tc))
```

