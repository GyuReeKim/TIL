# Python 1주차 (2019.07.18)

데이터 타입을 확인하는 습관을 기르자!

key의 값에 접근할 때는 대괄호 안에 key값을 적어준다.

```python
ssafy['location']
```



## Practice

```python
print(len(ssafy['location']))
print(ssafy.get('location')) # .get()을 활용하는 경우가 많을 예정
```

### 1. 

```python
"""
난이도* 1. 지역(location)은 몇개 있나요? : list length
출력예시)
4
"""
# for key, val in ssafy.items():
#     if key == 'location':
#         print(len(val))

print(len(ssafy['location']))
print(ssafy.get('location'))
print(ssafy.get('locatiooooon')) # none은 if문 안에서 False를 반환한다.
```



### 2.

```python
"""
난이도** 2. python standard library에 'requests'가 있나요? : 접근 및 list in
출력예시)
False
"""
# for key, val in ssafy.items():
#     if key == 'language':
#         for lan, sub in val.items():
#             if lan == 'web':
#                 break
#             for subj, lib in sub.items():
#                 if subj == 'python standard library':
#                     if 'requests' in lib:
#                         print('True')
#                     else:
#                         print('False')

# lan = ssafy['language']['python']['python standard library']
# if lan == 'requests':
#     print('True')
# else:
#     print('False')
    
if ssafy.get('language').get('python').get('python standard library') == 'requests':
    print('True')
else:
    print('False')
```



### 3.

```python
"""
난이도** 3. gj반의 반장의 이름을 출력하세요. : depth 있는 접근
출력예시)
박선용
"""
# for key, val in ssafy.items():
#     if key == 'classes':
#         for gwang, cont in val.items():
#             for posi, pers in cont.items():
#                 if posi == 'class president':
#                     print(pers)

# stud = ssafy['classes']['gj']['class president']
# print(stud)

print(ssafy.get('classes').get('gj').get('class president'))
```



### 4. 

```python
"""
난이도*** 4. ssafy에서 배우는 언어들을 출력하세요. : dictionary.keys() 반복
출력 예시)
python
web
"""
# for key, val in ssafy.items():
#     if key == 'language':
#         for lang in val.keys():
#             print(lang)

# lang = ssafy['language']
# for key in lang:
#     print(key)

language = ssafy.get('language')
for l in language.keys():
    print(l)
```



### 5.

```python
"""
난이도*** 5 ssafy gm반의 강사와 매니저의 이름을 출력하세요. dictionary.values() 반복
출력 예시)
justin
pro-gm
"""
# name = ssafy['classes']['gm']
# for val in name.values():
#     print(val)

gm = ssafy.get('classes').get('gm')
for i in gm.values():
    print(i)
```



### 6. 

```python
"""
난이도***** 6. framework들의 이름과 설명을 다음과 같이 출력하세요. : dictionary 반복 및 string interpolation
출력 예시)
flask는 micro이다.
django는 full-functioning이다.
"""
# frame = ssafy['language']['python']['frameworks']
# for fr, fram in frame.items():
#     print(f'{fr}는 {fram}이다.')

frameworks = ssafy.get('language').get('python').get('frameworks')
for key, val in frameworks.items():
    print(f'{key}는 {val}이다')
```



### 7.

```python
"""
난이도***** 7. 오늘 당번을 뽑기 위해 groups의 E 그룹에서 한명을 랜덤으로 뽑아주세요. : depth 있는 접근 + list 가지고 와서 random.
출력예시)
오늘의 당번은 정은지
"""
# import random
# person = ssafy['classes']['gj']['groups']['E']
# per = random.choice(person)
# print(per)

e = ssafy.get('classes').get('gj').get('groups').get('E')
print(e)
import random
today = random.choice(e)
print(f'오늘 당번은 {today}')
```



## Scope

구글에 '유지 보수를 어렵게 하는 코딩'을 검색하면 pdf 파일이 나온다. 읽어보면 좋다.



## 재귀 함수

점화식에는 초기값이 항상 존재한다.

반복되는 시점을 항상 표기해줘야 한다.

### 팩토리얼

```python
# 반복문
def fact(n):
    result = 1
    while n > 1:
        result *= n
        n -= 1
    return result
fact(5)

# 재귀
def factorial(n):
    if n <= 1:
        return n
    else:
        return factorial(n-1) * n # 함수 내부에서 자기 자신을 호출할 수 있다.
factorial(5)
```



### 피보나치 수열

```python
# 반복문 1
def fib_loop(n):
    result = [1, 1, ]
    for i in range(1, n):
        fib_sum = result[i] + result[i-1]
        result.append(fib_sum)
    return result[-1]

# 반복문 2
def fib_loop(n):
    result = [1, 1, ]
    for i in range(1, n):
        fib_sum = result[-1] + result[-2] # 끝 값과, 끝 값의 바로 앞 값이다.
        result.append(fib_sum)
    return result[-1]

# 재귀
def fib(n):
    if n == 0 or n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)
fib(4)
```



### Random

```python
# for문 사용
import random
secret = random.randint(1, 20)
print('추측해보세요')

for count in range(6):
    guess = int(input('입력: '))
    
    if secret < guess:
        print('down')
    elif secret > guess:
        print('up')
    else:
        break
        
if guess == secret:
    print('정답')
else:
    print('끝')

# 함수 사용
import random

def generate_num():
    return random.randint(1, 20)

def compare_num(guess, secret):
    if guess < secret:
        return 'up'
    elif guess > secret:
        return 'down'
    else:
        return 'ok'
    
def guess_num():
    print('숫자를 맞춰보세요')
    secret = generate_num()
    
    for count in range(6):
        guess = int(input('input : '))
        status = compare_num(guess, secret)
        
        if status == 'ok':
            return '정답'
        else:
            print(status)
            
guess_num()
```



## problem

### 버거지수

```python
def burger(king, mc, kfc, ria):
    return (king + mc + kfc) / ria

print(burger(**locationA))
```



### 종합소득세 계산하기

```python
def tax(money):
    if money <= 1200:
        result = money*0.06
    elif money <= 4600:
        result = 1200*0.06 + (money - 1200)*0.15
    else:
        result = 1200*0.06 + (4600-1200)*0.15 + (money - 4600)*0.35
    return result

print(tax(1200))
```



### 텔레그램 챗봇

```python
def tele(token, method):
    base_url = 'https://api.telegram.org/bot'
    
    if len(token) != 41:
        return 403
    
    return f'{base_url}{token}/{method}'
    
tele('123123:afjio;wef', 'getMe')
tele('123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11', 'getMe')
```



### 솔로 천국

```python
# 1
def lonely(couple):
    solo = [] # 빈 리스트 생성
    for item in couple:
        if not solo: # solo가 비어있으면 item을 넣어준다.
            solo.append(item)
        if solo[-1] != item: # solo의 마지막 원소가 couple의 item과 다를경우 solo에 값을 추가해준다.
            solo.append(item)
    return solo

# 2
def lonely(couple):
    solo = []
    for item in couple:
        if not solo:
            solo.append(item)
        if solo[-1] == item:
            pass
        else:
            solo.append(item)
    return solo
```



## 문자열 메소드 활용

> 원본의 값들을 바꿀 수 없다.

### 변형

#### .capitalize() .title() .upper()

```python
a = "hI! Everyone, I'm kim"

a.capitalize() # 앞글자를 대문자로 만들어 반환
# a라고 하는 변수에 문자열이 들어있다. capitalize라는 기능이 있다.
a.title() # ' 와 공백 이후를 대문자로 만들어 반환
a.upper() # 모두 대문자로 반환
```

위의 값들은 원본 데이터를 건드리지 않는다.



#### .lower() .swapcase()

```python
a = "hI! Everyone, I'm kim"

a.lower() # 모두 소문자로 반환
a.swapcase() # 대-소문자 변경후 반환
```



#### .join(iterable)

```python
'!'.join('배고파') # join 안의 글자 사이사이에 '!'를 넣어준다.
'-'.join(['1', '2', '3', '4', '5'])
```



#### .replace(old, new, count)

```python
word = 'yay!'
word.replace('a', '_') # 바꿀 대상을 새로운 글자로 바꿔서 반환

word2 = 'wooooooow'
print(word2.replace('o', 'a', 3))
```



#### .strip(chars)

```
'            hello'.strip()
'    hello       '.lstrip()
'hihihihi123ihihi'.rstrip('hi')
```



### 탐색 및 검증

#### .find(x)

```python
'apple'.find('a')
'apple'.find('z') # -1 반환
```



#### .index(x)

```python
'apple'.index('a')
'apple'.index('z') # 오류 발생
```



#### .split(x)

```python
'apple peach grape'.split()
'apple!peach!grape'.split('!')
```



## 리스트 메소드 활용

> 원본의 값들을 바꿀 수 있다.

### 값 추가 및 삭제

#### .append(x)

```python
caffe = ['starbucks', 'tomntoms', 'hollys']
caffe.append('coffeebene')
caffe[len(caffe):] = ['ediya']
```



#### .extend(iterable)

````python
caffe.extend(['droptop', '흑화당'])
caffe += ['빽다방', 'megacoffee']
caffe.append(['청자다방'])
caffe.extend('커피빈')
````



#### .insert(i, x)

```python
caffe.insert(0, 'hi')
caffe.insert(-1, 'bye')
caffe.insert(len(caffe), 'bye')
caffe.insert(999, 'hihi')
```



#### .remove(x)

```python
numbers = [1, 2, 3, 1, 2]
numbers.remove(1)
numbers.remove(1)
numbers.remove(1)
```



#### .pop(i)

```python
numbers = [1, 2, 3, 4, 5, 6]
print(numbers.pop(0)) # 원본을 바꿀 수 있다.
value = numbers.pop(4)
print(f'{value}가 빠져나온 결과는 {numbers}입니다.')
```



### 탐색 및 정렬

#### .index(x)

```python
numbers = [1, 2, 3, 4, 5]
numbers.index(1)
numbers.index(10)
```



#### .count(x)

```python
numbers = [1, 2, 5, 1, 5, 1]
numbers.count(1)
num = 1
for number in range(numbers.count(num)):
    numbers.remove(num) # count를 통해 검증을 했기에 오류발생 x
num is numbers
```



#### .sort()

````python
import random
lotto = random.sample(range(1, 46), 6)
print(lotto.sort(reverse=True)) # 'reverse=True': 역순 출력
````



#### .reverse()

```python
classroom = ['Tom', 'David', 'Justin', 'change']
classroom.reverse()
```



### 복사

```python
original_list = [1, 2, 3]
copy_list = original_list
print(original_list)
print(copy_list)

original_list = [123, 2, 3]
copy_list = original_list
print(original_list)
print(copy_list)

print(id(original_list))
print(id(copy_list))

a = 123456
b = a
b = 654321
print(a)

lunch = {'차이나': '고추참치덮밥', '스냅스낵': '라면'}
dinner = lunch
dinner['차이나'] = '쫄면'
print(lunch)
```

#### Copy

copy 단계는 python tutor을 활용하는 것이 좋다.

````python
a = [1, 2, 3]
b = a[:] # ':' 처음부터 끝까지 slicing을 한다.
b[0] = 123

a = [1, 2, 3]
b = list(a)
b[0] = 123

a = [1, 2, [9, 10]]
b = list(a)
b[2][0] = 9999

import copy
a = [1, 2, [9, 10]]
b = copy.deepcopy(a) # b에 있는 것을 바꿔도 a가 바뀌지 않는다.
b[2][0] = 99999
print(a)
````



#### .clear()

```python
caffe.clear() # 데이터를 모두 지운다.
print(caffe)
```
