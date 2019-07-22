

# Python 2주차 (2019.07.22)

## 근사값 구하기

```python
def bisection(x):
    left = 1
    right = x
    result = 1 # 'x**(1/2)': 구하고자 하는 값
    
    import math
    while not math.isclose(result**2, x): # isclose: 어느정도 근사치에 가까워졌을때
        result = (left + right)/2
        if result**2 < x:
            left = result
        else:
            right = result
    return result

bisection(2)
```



## Problem

### 문자열 탐색

```python
def start_end(words): # 리스트로 들어왔기 때문에 파라미터를 하나의 값으로 설정한다.
    count = 0
    for word in words:
        if (len(word) >= 2) and (word[0] == word[-1]): # 괄호로 명시적으로 구분한다.
            count += 1
    return count
```



### 최대공약수, 최소공배수 구하기

```python
def gcd(n, m):
    small, big = min(n, m), max(n, m)
    
    if big % small == 0:
        return small
    else:
        return gcd(big % small, small)
        
def gcdlcm(n, m):
    g = gcd(n, m)
    l = n * m / g
    return g, l
```



### Collatx 추측

```python
def collatz(num):
    for i in range(500):
        if num == 1:
            return i # i가 0일때는 for문을 그냥 통과한다.
        else:
            # 홀수
            if num % 2:
                num = num * 3 + 1
            # 짝수
            else:
                num /= 2
            # 이런 방법도 있다.
#             num = (num * 3 + 1) if num % 2 else (num / 2)
    return -1
```



### 이상한 덧셈

```python
def positive_sum(*numbers):
    sum = 0
    for number in numbers:
        if number > 0:
            sum += number
    return sum
```



## Project 01 풀이

> 딕셔너리는 중복된 key값에 대한 데이터를 가공할 때 좋다.
>
> 리스트에서도 중복 제거가 가능하긴 하지만 시간이 오래걸려 추천하지 않는다.



### 1. 하루 데이터 가져오기

```python
import requests

url = 'http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchWeeklyBoxOfficeList.json'
key = 'secret'
movie_url = f'{url}?key={key}&targetDt=20190713&weekGb=0'
print(movie_url)

res = requests.get(movie_url).json()
print(res)
```

요청 인터페이스의 'weekGb'를 '0'으로 설정해줘야 원하는 데이터를 불러올 수 있다. ('0': 주간, '1': 주말, '2': 주중)



### 2. 구조 확인하기

```python
print(res['boxOfficeResult']['weeklyBoxOfficeList'])
```



### 3. 내가 원하는 데이터 뽑아오기

```python
for i in range(10):
    movieCd = res['boxOfficeResult']['weeklyBoxOfficeList'][i]['movieCd']
    movieNm = res['boxOfficeResult']['weeklyBoxOfficeList'][i]['movieNm']
    audiAcc = res['boxOfficeResult']['weeklyBoxOfficeList'][i]['audiAcc']
    print(movieCd, movieNm, audiAcc)
```

위의 코드에 중복되는 코드가 많이 보이는데, 이는 코드를 단축할 수 있다는 것을 의미한다. 함수 등으로 단축시킬 수 있다.



### 4. 딕셔너리로 만들기

```python
movie_dict = {}
box_office_list = res['boxOfficeResult']['weeklyBoxOfficeList']
for movie in box_office_list:
    movie_dict[movie['movieCd']] = {
        'movieCd': movie['movieCd'],
        'movieNm': movie['movieNm'],
        'audiAcc': movie['audiAcc']
    }
print(movie_dict)
```



### 5. 날짜 늘리기

```python
import datetime
for i in range(5):
    targetDt = datetime.datetime(2019, 7, 13) - datetime.timedelta(weeks=i)
    targetDt = targetDt.strftime('%Y%m%d')
    url = 'http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchWeeklyBoxOfficeList.json'
    key = 'secret'
    movie_url = f'{url}?key={key}&targetDt={targetDt}&weekGb=0'
    print(movie_url)
```

`.timedelta`는  `datetime`의 연산을 도와준다.



### 6. csv파일 저장

```python
import csv
with open('boxoffice.csv', 'w', newline='', encoding='utf-8') as f:
    fieldnames = ('movieCd', 'movieNm', 'audiAcc')
    csv_writer = csv.DictWriter(f, fieldnames=fieldnames)
    csv_writer.writeheader()
    for movie in movie_dict.values():
        csv_writer.writerow(movie)
```



### 7. 영화 정보 가져오기

```python
import datetime
import requests

movie_dict = {}
for i in range(50):
    targetDt = datetime.datetime(2018, 8, 4) + datetime.timedelta(weeks=i)
    targetDt = targetDt.strftime('%Y%m%d')
    url = 'http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchWeeklyBoxOfficeList.json'
    key = 'secret'
    movie_url = f'{url}?key={key}&targetDt={targetDt}&weekGb=0'

    res = requests.get(movie_url).json()

    box_office_list = res['boxOfficeResult']['weeklyBoxOfficeList']
    for movie in box_office_list:
        movie_dict[movie['movieCd']] = {
            'movieCd': movie['movieCd'],
            'movieNm': movie['movieNm'],
            'audiAcc': movie['audiAcc']
        }
import csv
with open('boxoffice.csv', 'w', newline='', encoding='utf-8') as f:
    fieldnames = ('movieCd', 'movieNm', 'audiAcc')
    csv_writer = csv.DictWriter(f, fieldnames=fieldnames)
    csv_writer.writeheader()
    for movie in movie_dict.values():
        csv_writer.writerow(movie)
```



## List Comprehension

> 다른 사람이 짠 코드를 읽을 수 있으면 된다.



### 세제곱리스트

```python
cubic_list = [num**3 for num in numbers] # ':' 붙이지 않는다. 뒤에서부터 읽으면 편함
                                # numbers에서 num을 하나씩 세면서 num**3을 계산한다.
print(cubic_list)
```



### 짝수리스트

```python
even_list = [num for num in numbers if not num % 2]
# for문을 돌면서 if문의 조건에 맞다면 num을 list에 넣는다.
print(even_list)
```



### 곱집합

```python
pair = [(girl, boy) for girl in girls for boy in boys]
print(pair)
```



### 피타고라스 정리

```python
result = [(x, y, z) for x in range(1, 50) for y in range(x, 50) for z in range(y, 50) if x**2 + y**2 == z**2]
print(result)
```



### 모음 제거하기

```python
words = 'Life is too short, you need python!'
vowels = 'aeiou'

result = [word for word in words if word not in vowels]
print(''.join(result))
```



## 딕셔너리 메소드 활용

### .pop(key[ , default])

```python
my_dict = {'apple': '사과', 'banana': '바나나'}
my_dict.pop('apple')
print(my_dict)
# {'banana': '바나나'}

my_dict.pop('melon') # 오류 발생

my_dict.pop('melon', 0)
# 0
```



### .update()

```python
my_dict = {'apple': '사과', 'banana': '바나나', 'melon': '멜론'}
my_dict.update(apple='사과아') # key값이 없으면 추가적인 key를 생성한다.
print(my_dict)
# {'apple': '사과아', 'banana': '바나나', 'melon': '멜론'}
```



### .get(key[ , default])

```python
my_dict = {'apple': '사과', 'banana': '바나나', 'melon': '멜론'}
# my_dict['pineapple']
print(my_dict.get('pineapple', '없어요'))
# 없어요
```



### 세제곱딕셔너리

```python
cubic = {i: i**3 for i in range(1, 11)}
print(cubic)
```



### 미세먼지딕셔너리

```python
dusts = {'서울': 72, '대전': 82, '구미': 29, '광주': 45, '중국': 200}
result = {key: value for key, value in dusts.items() if value > 80}
print(result)
```

```python
result = {key: ('나쁨' if value > 80 else '보통') for key, value in dusts.items()}
print(result)
```

```python
result = {key: ('매우나쁨' if value > 150 else '나쁨' if value > 80 else '보통') for key, value in dusts.items()}
print(result)
```



## 세트 메소드 활용



### . add()

```python
fruits = {"사과", "바나나", "수박"}
fruits.add('포도') # set은 특별한 순서가 없다.
fruits.add('포도') # 중복 데이터는 하나만 저장
print(fruits)
# {'사과', '포도', '수박', '바나나'}
```



### . update(*others)

```python
fruits = {"사과", "바나나", "수박"}
fruits.update({'토마토', '토마토', '딸기'})
fruits.update('레몬') # '레', '몬' 처럼 출력
print(fruits)
# {'사과', '토마토', '레', '딸기', '바나나', '수박', '몬'}
```



### . remove()

```python
fruits = {"사과", "바나나", "수박"}
fruits.remove('사과')
fruits.remove('메론') # 오류 발생
print(fruits)
```



### .discard()

```python
fruits = {"사과", "바나나", "수박"}
fruits.discard('메론') # 오류 발생하지 않음
print(fruits)
# {'사과', '수박', '바나나'}
```



### .pop()

```python
a = {"사과", "바나나", "수박", "아보카도"}
a.pop() # 한번 pop 했을때 그 것을 들고있기 때문에 restart 해줘야 함
print(a)
# {'사과', '아보카도', '수박'}
```



## 모듈

### import

> fibo.py를 Text File에 작성한다.

```python
def fib(n):
    if n == 0 or n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)
    
    
def fib_loop(n):
    result = [1, 1,]
    for i in range(1, n):
        fib_sum = result[-1] + result[-2]
        result.append(fib_sum)
    return result[-1]
```

```python
import fibo
print(dir(fibo))
print(fibo.fib(5))
fibo.fib_loop(10)
# ['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'fib', 'fib_loop']
# 8
# 89

my_fib = fibo.fib
my_fib(10)
```



### 패키지

> Folder 'myPackage' 생성 후 폴더 안에 `__init__.py`를 생성한다.
>
> math, web 폴더 생성 후 폴더 안에 `__init__.py`를 생성한다.

`__init__.py` : 파이썬이 디렉터리를 패키지로 취급하게 하기 위해 필요한 파일

#### formula.py

```python
pi = 3.14

def my_max(a, b):
    if a > b:
        print(f'{a}가 {b}보다 큽니다.')
    else:
        print(f'{b}가 {a}보다 큽니다.')
```



#### url.py

````python
def my_url(itemPerPage=10, **kwargs):
    if itemPerPage not in range(1,11):
        return "1~10사이의 숫자를 입력하세요"
    if 'key' not in kwargs or 'targetDt' not in kwargs:
        return '필수값 누락'
        
    base_url = 'http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?'
    base_url += f'itemPerPage={itemPerPage}&'

    for key, value in kwargs.items():
        base_url += f'{key}={value}&'
        
    return base_url
````

```python
# from 모듈명 import 어트리뷰트
# 모듈 안의 어트리뷰트를 불러온다.
from myPackage import web
print(dir(web))
# ['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__']


# from 모듈명 import *
# 모듈 안의 모든 어트리뷰트를 불러온다.
# 필요없는 어트리뷰트도 같이 불러오기 때문에 추천하지 않는다.
from myPackage.math.formula import *
print(pi)
my_max(2, 3)
# 3.14
# 3가 2보다 큽니다.


# from 모듈명 import 어트리뷰트 as
# 모듈 안의 어트리뷰트 이름을 내가 지정하는 이름으로 바꿀 수 있다.
from myPackage.web.url import my_url as api_url # 이 안에서 만큼은 바꾼 이름으로 부름

# 파이썬 기본 모듈
from random import sample as s # 특별한 경우가 아니라면 비추천!
numbers = range(1, 46)
s(numbers, 6)
# [33, 40, 34, 41, 18, 7]
```



## 숫자 관련 함수

### 수학 관련 함수 (math)

```python
import math

# 원주율(pi)
math.pi

# 자연 로그의 밑 (상수 e)
math.e

# 올림
pi = 3.141592
math.ceil(pi) # 4

# 내림
math.floor(pi) # 3

# 버림
math.trunc(pi) # 3

# 내림과 버림은 음수에서 처리가 다르다.
print(math.floor(-pi)) # -4
print(math.trunc(-pi)) # -3 trunc는 소수점 아래부분을 버려준다.

# 프로그래밍에서 나눗셈은 음수로 하거나 양수로 하거나 두가지 상황이 있습니다. 
# % 는 정수를, fmod 는 float를
# 부호가 다른 경우 서로 다르게 출력합니다.
print(math.fmod(-5, 2)) # -1.0
print(-5 % 2) # 1

# 제곱
math.pow(2, 5) # 32.0

# 제곱근
math.sqrt(4) # 2.0

# e
math.exp(10) # 22026.465794806718
```



### 로그 계산

```python
# 로그
math.log(math.e) # 1.0

# 삼각함수를 사용해봅시다.
math.sin(0) # 0.0
math.cos(0) # 1.0
```



#### 난수 발생관련 함수 (random)

> 사람에게는 랜덤하게 보이지만 실제로는 컴퓨터가 처리하는 연산의 결과다.

```python
import random

# 난수 생성
random.random() # 마치 랜덤같이 보이지만 실제로는 연산된 값이다.

# 임의의 정수 반환
random.randint(1, 5) # 2

# seed
# 경우에 따라서(보통 디버깅 등을 위해 ) 동일한 순서로 난수를 발생시켜야 할 경우가 있다. 
# 난수 발생을 위해서는 적절한 시드(seed)를 난수발생기에 주어야 한다. 
# 만약 시드가 같다면 동일한 난수를 발생시키게 된다. 
# 시드 설정을 하지 않으면 현재 시간을 기반으로 만든다.
random.seed(1)

# 시드 설정 후에 첫번째 값을 확인해보자
random.random() # seed를 실행 후 실행해보면 첫번째 값은 무조건 0.13436424411240122

# 시퀀스 객체를 섞는다.
name = ['change', 'neo', 'jason', 'tak']
random.shuffle(name)
print(name)
```



## 날짜 관련 모듈

### datetime

> 날짜와 관련된 정보를 출력한다.

```python
# 1970년 1월 1일부터 1초씩 증가합니다.
# 오늘을 출력해봅시다.
from datetime import datetime
today = datetime.now()
print(today)

today = datetime.today()
print(today)

# UTC기준시도 출력가능합니다.
datetime.utcnow()

# 내가 원하는대로 예쁘게 출력해봅시다.
today.strftime('%Y-%m-%d') # '2019-07-22'

# 속성을 출력해봅시다.
today.year
today.hour

# 월요일을 시작으로 0~6
today.weekday() # 7월 22일 월요일 기준으로 0

# 크리스마스를 만들어봅시다.
christmas = datetime(2019, 12, 25)
print(christmas)

# 예쁘게 출력해봅시다.
christmas.strftime('%m/%d') + '는 크리스마스' # '12/25는 크리스마스'
```



### timedelta

> `datetime`의 연산을 할 수 있게 해준다.

```python
from datetime import timedelta

# 활용해봅시다.
ago = timedelta(days=-3)
print(ago) # -3 days, 0:00:00

# 비교 및 연산이 가능합니다.
today + ago

# 오늘부터 1일일 때, 100일 뒤는?
today + timedelta(days=100)

# 크리스마스까지 얼마나 남았는가?
diff = christmas - today
print(diff) # 155 days, 8:43:11.801063

# 초로 만들어봅시다.
diff_seconds = diff.total_seconds()

# 아래에 초를 예쁘게 출력하는 함수 print_time_delta() 를 만들어봅시다.
# 예시 => '10일 1시간 18분 51초 전'
def print_time_delta(seconds):
    sign = '전' if seconds < 0 else '후'
    seconds = abs(int(seconds))
    days, seconds = divmod(seconds, 86400)
    hours, seconds = divmod(seconds, 3600)
    minutes, seconds = divmod(seconds, 60)
    if days > 0:
        return f'{days}일 {hours}시간 {minutes}분 {seconds}초 {sign}'
    elif hours > 0:
        return f'{hours}시간 {minutes}분 {seconds}초 {sign}'
    elif minutes > 0:
        return f'{minutes}분 {seconds}초 {sign}'
    else:
        return f'{seconds}초 {sign}'

print(print_time_delta(diff_seconds)) # 155일 8시간 43분 11초 후
```

공식 문서를 읽는 습관을 들이는 것이 좋다.



## Errors and Exceptions (에러와 예외처리)



### 문법 에러 (Syntax Error)

`파일 이름`, `줄`, `^`을 통해 문제 발생 위치를 표현해준다. 하지만 지정된 위치가 아닐 수도 있으므로 전체를 확인해주어야 한다.



### 예외 (Exceptions)

실행 시 발생하는 에러다.



## 예외 처리

### try, except

> `if else`문과 비슷한 느낌이다.

에러가 발생하면 특별한 기능을 준다. 프로그래머가 기대하지 않는 입력을 사용자가 입력했을 때 발생한다.

```python
try:
    num = input('숫자를 입력하세요: ')
    print(int(num))
except ValueError:
    print('바보야 숫자를 입력하라고!')
```



### 복수의 예외 처리

> 튜플을 사용해 예외 처리를 할 수 있다.

```python
# 문자열일때와 0일때 모두 처리를 해봅시다.
try:
    num = input('100으로 나눌 값을 입력하세요: ')
    print(100/int(num))
except (ValueError, ZeroDivisionError):
    print('바보지?')
    
# 각각 다른 오류를 출력할 수 있습니다.
try:
    num = input('100으로 나눌 값을 입력하세요: ')
    print(100/int(num))
except ValueError:
    print('숫자를 넣으라고!!!')
except ZeroDivisionError:
    print('0으로 숫자를 나눌 수 없어')

# 에러는 순차적으로 수행된다.
try:
    num = input('100으로 나눌 값을 입력하세요: ')
    print(100/int(num))
except ValueError:
    print('숫자를 넣으라고!!!')
except ZeroDivisionError:
    print('0으로 숫자를 나눌 수 없어')
except Exception:
    print('뭔지 모르겠지만 잘못됨')
```



### 에러 문구 처리

```python
# 에러 메세지를 넘겨줄 수도 있습니다.
try:
    my_list = []
    print(my_list[100])
except IndexError as err:
    print(f'{err}, 오류가 발생')
    
# list index out of range, 오류가 발생
```



#### else

> 에러가 발생하지 않을 경우에 수행된다.

```python
try:
    numbers = [1, 2, 3]
    number = numbers[2]
except IndexError:
    print('에러발생!!!!!!!')
else:
    print(number**2) # IndexError가 발생하지 않으므로 else문이 실행된다.
```



#### finally

> 반드시 수행해야하는 문장에 사용한다.

```python
try:
    fruits = {'apple': '사과', 'peach': '복숭아'}
    fruits['pineapple']
except KeyError as err:
    print(f'{err}에러가 발생!!!')
finally:
    print('finally!!!')
    
# 'pineapple'에러가 발생!!!
# finally!!!
```



## gitlab  연결

```
git diff
git remote -v
git remote add gitlab 주소 # 주소는 clone의 HTTPS 복사
git push gitlab master
```

