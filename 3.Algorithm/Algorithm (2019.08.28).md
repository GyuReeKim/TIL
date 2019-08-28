# Algorithm (2019.08.28)

## 권장하는 풀이법 (함수로 만들기)

효율적인 방법이다.

```python
def f():
    for i in range(N):
        for j in range(N):
            if arr[i][j] == 1:
                return 1
```



## 함수를 만들지 않았을 경우

IM형은 논리적으로 돌아가기만 하면 된다.

```python
result = 0
for i in range(N):
    if arr[i] == 1:
        result = 1
        break
```



### 이중 for문

```python
result = 0
for i in range(N):
    for j in range(N):
        if arr[i][j] == 1:
            result = 1
if result:
```



## 스위치 켜고 끄기

```python

```



## 원재의 메모리 복구하기 코드 샘플

```python
T = int(input())
for tc in range(1, T+1):
    bit = [int(num) for num in (input())]
    # 이 부분에 코드 작성
    
    # 여기까지
    print('#{} {}'.format(tc, cnt))
```





## IM 기출1 풀이

어떤 방에 대한 조작으로 왼쪽 방은 조작할 수 없다.

꺼진 상태에서 켜는 대신, 최종 상태에서 모두 꺼진 상태를 만들어본다.

왼쪽부터 오른쪽으로 가면서 켜져있는 스위치만 찾아서 끔

```python
cnt = 0
for i in range(1, N+1): # 스위치가 켜져있는 지 확인
    if room[i] == 1: # i번 방이 켜져있으면
        j = 1
        cnt += 1 # i번 방의 스위치를 끈 횟수
        while i*j <= N: # i의 배수가 존재하는 방 번호일 때
            room[i] = (room[i]+1) % 2 # 스위치 상태 반전
            j+1
print(cnt)
```



- 7510 상원이의 연속합은 오른쪽에서 왼쪽으로 (왼쪽에서 오른쪽으로 가도 상관은 없음)



## 정곤이의 단조 증가하는 수

```python
for i in range(0, N-1):
    for j in range(i+1, N):
        A[i]*A[j]
```



