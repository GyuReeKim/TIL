# Algorithm (2019.10.01)

## Computational Thinking

### log n이란

> 시험 출제 가능성 높음

(1) 2의 몇 승이 n이 되느냐의 답

(2) n을 표현하는 데 몇 비트가 필요한가의 답

(3) 1로 시작해서 계속 두배를 할 때 몇 번 하면 n이 되느냐의 답

(4) n을 2로 계속 나눌 때 몇 번 나누면 거의 1이 되느냐에 대한 답



### 공식

#### 상수 법칙

$$
log_a1 = 0
$$

$$
log_aa = 1
$$

#### 덧셈 법칙

$$
log_axy = log_ax + log_ay
$$

#### 뺄셈 법칙

$$
log_a\frac{x}{y} = log_ax - log_ay
$$

#### 지수 법칙

$$
log_ax^b = blog_ax
$$

#### 밑 변환 법칙

$$
log_bx = \frac{log_kx}{log_kb}(단, k>0, k ≠ 1)
$$

#### 역수 법칙

$$
log_bx = \frac{1}{log_xb}(단, b ≠ 1)
$$



## 집합과 조합론

A = {x|x = 2k+1, k는 0 이상의 정수}, B = {x|x = 4k+1 혹은 x = 4k+3, k는 0 이상의 정수}



## 기초 수식

> 중요! 시험에 나올 가능성 큼!

(1) T(n) = T(n-1) + 1 => O(n)

(2) T(n) = T(n-1) + n => O(n**2)

(3) T(n) = T(n-1) + logn => O(nlogn)

(4) T(n) = T(n/2)  + 1 => O(logn)

(5) T(n) = T(n/2) + n  => O(n)

(6) T(n) =2T(n/2) + n => O(nlogn)

(7) T(n) = 3T(n/2) + n => O(n**log3)

(8) T(n) = T(n-1) + 1/n O(logn)

(9) O(n)

(10) O(nlog(logn))