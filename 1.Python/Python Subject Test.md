# Python Subject Test

### 1번

```
not built-in function?
sqrt()
```



### 2번

```
sequence 특징?
자료형 타입
연속적 나열
in 연산자
정렬되어있지 않음
```



### 3번

```python
names = ['John', 'Ron', 'James', 'Betty']
print(names[-2][-2]) # e
```



### 4번

```python
class Person:
    population = 0
    # 클래스 변수이지만 인스턴스 참조 가능하다.
    # Person이 상위 class이기 때문에 population 값을 가져온다.
    
    def __init__(self, name):
        self.name = name
        Person.population += 1
        
p1 = Person('hong')
p2 = Person('kim')
p3 = Person('kang')
p3.population # 3
```



### 5번

```
오류 발생하는 것?
.real()
```

```python
my_list = [1, 2, 3]
my_list.pop() # 3
```



### 6번

```python
def func(c, b, a):
    return a*b+c

print(func(2, 5, 4)) # 22
```



### 7번

```python
print(type({'a': 'apple'})) # <class 'dict'>
```



### 8번

```
함수 설명 아닌것?
return 없으면 none값을 반환한다.
오류가 나지 않는다.
```



### 9번

```python
numbers = [0, 0, 0, 0, 0] # [0, 0, 0, 0, 0]

numbers = [0] * 5 # [0, 0, 0, 0, 0]

numbers = []
numbers.append(0*5) # [0]

numbers = [0 for i in range(5)] # [0, 0, 0, 0, 0]
print(numbers)b
```



### 10번

```python
result = 4 + True + False + 5
print(result) # 10
```



### 11번

```python
s = 'hello my name is ssafy'
for i in s:
    if i == 'm':
        print(s)
'''
hello my name is ssafy
hello my name is ssafy
'''
```



### 12번

```python
d1 = {'d': dict()}
d2 = dict(d={})
print(d1 == d2) # True
print(id(d1) == id(d2)) # False
len(d1) # 1
len(d2) # 1
print(d1) # {'d': {}}
print(d2) # {'d': {}}
```



### 13번

```python
fruits = {'apple': '사과', 'banana': '바나나'}
a = fruits.get('apple')
b = fruits.get('cherry')
c = fruits.get('melon', True)
d = {a: b}
if c:
    print(d) # {'사과': None}
```



### 14번

```python
def func(c='5', *args):
    a, c, b = args
    return a + b + c
print(func('3', '4', '1', '2')) # 421

# 위치 인자가 없기에 기본 인자가 나올 수 있다.
'''
def func('3', ('4', '1', '2')):
    c = '3'
    a, c, b = '4', '1', '2'
'''
```



### 15번

```python
name = 'hong'

class Person:
    name = 'choi' # class로 접근 불가 Person.name으로 접근가능
    def greeting(self):
        print(name) # self가 없어 접근 불가
        print(self.name) # 'kim'
        print(Person.name) # 'choi'
        
p1 = Person()
p1.name = 'kim' # 인스턴스 내부에 저장
p1.greeting() # hong
```



### 16번

```python
my_int = 3
# isinstance(int, my_int) # 오류
# my_int == 3 # True, int형인지 확인 불가
# isinstance(my_int, 3) # 오류
type(my_int) == int # True

isinstance(my_int, int)
```



### 17번

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        
# p1 = Person(p1, 'hong', 100)
# p2 = Person('kim')
p3 = Person(age=3, name='kang')
# p4 = Person()
```



### 18번

```python
word = 'python'
# indexing = word[3:8]
indexing = word[3:]
print(indexing) # hon
```



### 19번

```python
def my_sum(a, b):
    c = a + b
    print(c) # 13
    
result = my_sum(5, 8)
print(result) # None
```



### 20번

```python
string = 'I am hungry'
result = ''
for idx, value in enumerate(string):
    if idx % 2:
        result += value.upper()
    else:
        result += value
print(result) # I aM HuNgRy
```



### 21번

```python
d = {'a': 1, 'b': 2}
a1 = d.update(c=3)
a2 = a1

print(a1) # None

# len(a1) # 오류
# 딕셔너리 x
# 에러 발생 x
# 보기 중에 답이 없다
```



### 22번

```python
def func(a, b=1, c=2, *args, **kwargs):
    d = sum([n*2 for n in args if n > 2]) # 6 + 14 = 20
    e = sum([v*v for k, v in kwargs.items()]) # 9 + 36 = 45
    return a + b + c + d + e

print(func(9, 4, 2, 3, 1, 7, d = 3, e = 6)) # 80

'''
def func(9, 4, 2, (3, 1, 7), {d: 3, e: 6}):
'''
```



### 23번

```python
def fib(n):
    if n == 0 or n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)
    
print(fib(4)) # fib(0) + fib(1) + fib(0) + fib(1) + fib(1)
```



### 24번

```python
a = 1
def func_1():
    a = 5 # func_1 안에 있는 5
    func_2() # 함수를 호출했을 뿐...
    
def func_2():
    print(a, end='')
    
func_1()
print(a) # 11
```

```python
a = 1
def func_1():
    a = 5
    func_2(a) # 함수에서 쓰려면 인자를 넣어줘야 한다.
    
def func_2(asdf):
    print(asdf, end='')
    
func_1()
print(a) # 51
```



### 25번

```python
import copy

list1 = [3, 'a', 'b']
list2 = [1, 2, list1]

list3 = list1[:]
list4 = copy.copy(list2)
list5 = copy.deepcopy(list2)

print(list1) # [3, 'a', 'b']
print(list2) # [1, 2, [3, 'a', 'b']]
print(list3) # [3, 'a', 'b']
print(list4) # [1, 2, [3, 'a', 'b']]
print(list5) # [1, 2, [3, 'a', 'b']]

print(list1 == list3) # True

list4[2][0] = 4
print(list2[2][0]) # 4

list4[2] = 5
print(list2[2]) # [3, 'a', 'b']

list5[2][1] = 3
print(list2[2][1]) # a
```





