# Algorithm (2019.08.22)

## 파리 퇴치

### X자 모양의 파리채

(1) 파리채가 X자 모양이라면(크기는 K)

부분 배열의 왼쪽 위 모서리 좌표가 i, j일 때,

```python
s = 0
for m in range(K):
    s += fly[i+m][j+m] # 오른쪽 아래 방향
    s += fly[i+m][j+K-1-m] # 왼쪽 아래 방향
# K가 홀수인 경우 가운데 원소가 두번 더해지므로
if K % 2 == 1: # 홀수일 경우
    s -= fly[i+K//2][j+K//2] # 중복된 가운데 한개를 빼줌
```



### ㄱ, ㄴ, ㄷ, ㅁ 모양의 파리채

(2) 파리채가 ㄱ, ㄴ, ㄷ, ㅁ 모양인 경우



### 모자이크 모양의 파리채

(3) 파리채의 영역이 i+m, j+n일 때, (0 <= m , n < K)

m 짝수, n 홀수

m 홀수, n 짝수

나머지는 구멍이 나서 파리가 죽지 않는다.



## 숫자 배열 회전

원본 행렬 A와 같은 NxN크기의 빈 배열 B를 준비한다.

(1) 90도 회전

A의 첫(0) 행을 B의 마지막 (N-1)열에 복사

A의 둘째(1) 행을 B의 (N-2)열에 복사

...

A의 마지막(N-1) 행을 B의 첫(0)열에 복사

(2) 180도 회전: 90도 회전의 결과를 다시 (1)에 입력

(3) 270도 회전: (2)의 결과를 (1)에 다시 입력
