# Algorithm (2019.08.19)

## 스택 (stack)

> 메모리를 쌓아 올린 후, 가장 마지막에 삽입한 자료를 가장 먼저 꺼내는 방식이다. 후입선출방식(LIFO, Last-In-First-Out)



push, pop 연산을 사용한다.



### Push

```python
def push(item):
    s.append(item)
```



### Pop

```python
def pop():
    if len(s) == 0:
        # underflow
        return
    else:
        return s.pop(-1)
```



## 괄호 검사 (스택의 응용1)

```python
# 2
# ( )( )((( )))
# ((( )((((( )( )((( )( ))((( ))))))

# 괄호의 짝이 맞으면 1 아니면 0 리턴
def f(txt):
    s = list() # 스택생성
    for i in range(len(txt)):
        if txt[i] == '(': # 여는 괄호면 push()
            s.append(txt[i])
        elif txt[i] == ')': # 닫는 괄호면 pop()해서 비교
            if len(s) == 0: # 스택이 비어있으면 오류
                return 0
            # 스택이 비어있지 않으면 여는 괄호 하나 꺼냄
            else:
                s.pop()

    if len(s) != 0: # 스택에 여는 괄호가 남아있으면
        return 0
    else:
        return 1


T = int(input())
for tc in range(1, T+1):
    txt = input()
    print(f(txt))
```



## function call (스택의 응용2)

### 재귀 호출

> factorial과 피보나치 수열 등이 있다. 실무에 쓰지는 않지만 쓸모가 없지는 않다. 수식 검증을 한다.



#### 특징

- 자기 자신을 호출하지만 사용하는 메모리 영역이 구분되므로 다른 함수를 호출하는 것과 같음
- 정해진 횟수만큼, 혹은 조건을 만족할 때 까지 호출을 반복함.



#### 정해진 횟수만큼 호출하기

- 호출 횟수에 대한 정보는 인자로 전달.
- 정해진 횟수에 다다르면 호출 중단.





함수를 호출할 때 이름이 겹치면 내부에 있는 함수가 우선시 된다.

서로의 영역을 침범하지 않는다.



다음 함수를 호출하면 기존 함수의 메모리가 제거된다.



### factorial

```python
# factorial
def fact(n):
    if n < 2:
        return 1
    else:
        return n * fact(n-1)

N = 4
print(fact(N))
```



### fibonacci

```python
# fibonacci
# 1번
def fibo1(n):
    global memo
    if n >= 2 and len(memo) <= n:
        memo.append(fibo1(n-1) + fibo1(n-2))
    return memo[n]

memo = [0, 1]

N = 4
print(fibo1(N))

# 2번
def fibo(n):
    global memo
    if n >= 2 and memo[n] == 0: # 아직 fibo(n)이 계산되지 않은 경우
        memo[n] = fibo(n-1) + fibo(n-2)
    return memo[n] # fibo(n)이 계산되어 있으면 리턴

N = 7
memo = [0]*(N+1)
memo[0] = 0
memo[1] = 1
print(fibo(N))
print()
```



## DP (Dynamic Programming)

> 최적화와 관련된다. 유형 20가지 정도를 보면 된다. 코드를 보고 원리를 찾아내기 힘들다.





## 문제 풀이

### 종이붙이기

```python
# 1번
f(n) = f(n-1) + 2*f(n-2)

f = [1] # N = 0
f.append(1) # N = 1
f.append(3) # N = 2
for i in range(3, N+1):
    f.append(f[i-1] + 2*f[i-2])
    
# 2번
f = [0]*(N+1)
f[0] = 1 # 상황에 따라 사용
f[1] = 1
f[2] = 3
for i in range(3, N+1):
    f[i] = f[i-1] + 2*f[i-2]
    
# 3번
def f(n):
	if n == 1:
        return 1
    elif n == 2:
        return 3
   	else:
        return f(n-1) + 2*f(n-2)
```



### 괄호검사

모든 글자에 대해

​	(1) 여는 괄호 '{' 또는 '(' 인 경우 push()

​	(2)닫는 괄호인 경우

​	(2-1) '}'인 경우

​		pop() 결과 짝이 맞으면('{'인 경우) 계속 아니면 중지(비정상)

​	(2-2) ')' 인 경우

​		pop() 결과 짝이 맞으면('('인 경우) 계속 아니면 중지(비정상)

모든 글자에 대한 검토 후

​	스택에 남은 괄호가 있으면 비정상, 남은 괄호가 없으면 정상



### 반복문자 지우기

모든 글자에 대해

​	스택이 비어있지 않으면

​		현재 글자를 마지막으로 저장된 글자와 비교

​		일치하면 pop()하고 버림

​		일치하지 않으면 push()

​	스택이 비어있으면

​		push()



CAAABBA



C         (C, stack_is_empty)

CA       (A)

C~~AA~~    (A)

CA       (A)

CAB    (B)

CA~~BB~~  (B)

C~~AA~~     (A)

