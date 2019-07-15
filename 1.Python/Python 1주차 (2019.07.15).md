# Python 1주차 (2019.07.15)

## Python

1. 쉽다! 영어를 직독직해 하는 것 처럼! 하지만 쉽지 않다...
2. 강력하다! pypi.org (업로드 관리 사이트 : 파이썬 패키지 올려서 공유하는 사이트, bs4, tensorflow등 검색)
3. 빠르다! 개발 속도가 빠르다. Life is too short, you need Python



## Jupyter

> 앞으로 사용하게 된다.
>
> 코드를 셀 단위로 실행할 수 있다.

```
pip install jupyter
jupyter notebook
ctrl+c
mkdir python
cd python
jupyter notebook
new, Python3
```

```
ctrl+enter
shift+enter
alt+enter
esc로 나간 후 화살표로 위치 지정
enter 누르면 input모드
셀을 만든 후 위쪽의 Code를 Markdown으로 바꾸면 Typora처럼 바뀐다.
Code와 Markdown을 주로 작성
Markdown은 더블클릭하면 수정할 수 있으며, 추가 내용을 작성한 후 ctrl+enter을 누르면 저장할 수 있다.
파일을 지우고 싶다면 home으로 가서 체크박스 클릭 후 휴지통 버튼 누르면 된다.
```

github의 gjpy2로 들어가서 python teacher clone을 `cd ~`로 나가서 복사한다.

teacher문서는 손대지 않고 python파일로 끌어와서 옮긴다. `git pull`로 끌어와서 옮길수도 있다.



노트는 메모를 남길 수 있고, 책처럼 구성할 예정이며 코드를 작성하며 만들어나갈 수 있다.

PEP-8은 규칙이라고 할 수 있다. 정답이라고는 할 수 없지만 지키는 것이 편의를 위해 좋다.

PEP 가이드를 참고해서 수업을 진행한다.



d2 font를 검색하여 폰트를 다운로드한다.

Chrome 설정에서 글꼴 맞춤 설정에 들어가 고정폭 글꼴을 D2 Coding으로 바꿔준다.



## 식별자

```python
str = "hi"
str(5)
```

위의 예시처럼 작성하게 되면 오류가 발생하게 된다. 기존 str에 접근하려 했지만 덮어씌워졌기 때문에 불가능하다.

```python
del str
```

위의 `del str`을 여러번 해주면 python의 `str`도 삭제되기 때문에 꼭 한번만 지워야 한다. 하지만 지웠다고 해서 완전히 지워지는 것은 아니고, 다시 실행해주면 돌아오게 된다.



## 주석

```python
def mysum():
    """ # 여러줄을 쓰는 데 유용하다.
    설명
    """
    print("더했다!")
mysum.__doc__
```

`mysum.__doc__`는 이 문서를 읽어주라는 명령어다. 대부분은 작성할 필요가 없지만, 패키지 혹은 모듈을 만들 경우에는 docstring을 만들어야 한다.



## 코드 라인

에러코드가 나오면 어디에서 잘못되었고, 어떤 것이 잘못되었는지 확인하는 습관을 가지는 것이 좋다.

invalid syntax의 경우 문법적인 오류를 말한다.



```python
lunch = ["짜장면", "탕수육",
        "소시지야끼카레", "라면",
        "피자"
]
```

코드의 길이가 길어지지 않는편이 좋기 때문에 코드를 작성할 때 줄 바꿈을 활용해야 한다.



## 변수(variable) 및 자료형

변수를 사용하는 이유는 데이터의 의미 때문도 있지만, 보통 재사용을 위해서다.



### type()

`str`은 `string`의 약자로 `type()`를 사용해서 알 수 있다.

앞으로 `type`를 매우 많이 사용하게 될 것이다.



### id()

컴퓨터의 RAM에 저장되는데 저장되는 위치를 나타낸다.



```python
print(x, y)
x, y = y, x
print(x, y)
```

이렇게 표현이 가능한 것은 튜플이기에 가능하다.



## 수치형(Numbers)

### int(정수)

python에서 `int()`의 경우 매우 긴 숫자도 쓸 수 있다.

Overflow가 발생할 것 같지만 arbitray-precision arithmetic에 의해 가능하다.



### float(부동소수점, 실수)

`round(5.1-3.11, 2)`를 해주면 2번째 자리까지 반올림을 해준다.



```python
5.1 - 3.11 == 1.99
```

위를 실행하면 `False`가 나오기 때문에 이를 해결하기 위한 몇가지 방법들이 있다.

`sys.float_info.epsilon`에서 `epsilon`은 매우 작은 숫자를 말한다. 기억하지 않아도 된다.

math 모듈인 `math.isclose()`을 활용하는 방법도 있다.



```python
print(a.imag) # imaginary: 허수부
print(a.real) # real: 실수부
print(a.conjugate()) # conjugate: 켤레복소수
```



## Bool

`True`(참)와 `False`(거짓)는 대문자로 쓴다.

Bool은 if문에 사용 가능하다.



### None

다른 프로그래밍 언어에서는 Null을 쓴다.

```
a = None
print(a)
```

`None`이 출력되는데, 여기서 `None`은 문자열이 아니고 없는 데이터이다.



## 문자형(String)

jupyter에서 input()을 하게 되면 ln[*]가 뜬다. 여기서 input은 어떤 값을 입력할 때까지 기다린다.

혹시 오류가 발생한다면 'Kernel' 선택 후 'Restart'하면 재시작한다.



아래의 예시처럼 입력하게 되면 앞의 큰 따옴표 세트와 뒤의 큰 따옴표 세트를 큰 따옴표 내용으로 취급하여 오류가 발생하게 된다.

```python
print("   "안녕하세요"   ")
```



오류를 해결하기 위해서 아래의 예시처럼 `\"`를 쓰거나, `' "hi" '`처럼 쓰면 된다.

```python
print("   \"안녕하세요\"   ")
print('   "안녕하세요"   ')
```



```python
print("""
첫번째 문장
두번째 문장
끝나는 문장
""")
```

위의 예시처럼 줄을 바꾸는 경우는 크롤링할 때 많이 사용하게 될 것이다.



### 이스케이프 문자열

캐리지리턴`\r`은 `\r`뒤의 문장을 `\r`앞의 문장에 덮어씌워준다.



### String interpolation

`%`는 데이터의 포맷을 같이 적어준다.

```python
"반갑습니다. {}{}".format(name, name)
```

대괄호, 중괄호, 소괄호를 잘 구분해서 작성해야 한다.



## 연산자

### 산술 연산자

`divmod`는 괄호 안에 몫과 나머지를 보여준다.



### 비교 연산자

```python
3.0 == 3
True
```

위의 두 숫자는 문자형은 달라도 같은 숫자로 취급한다.



### 논리 연산자

Python에서는 and, or, not을 사용한다. Python에서 `&`와 `|`는 비트 연산자다.



### 복합 연산자

복합 연산자는 사칙연산 모두가 해당된다.



### 기타 연산자

기타 연산자는 python에서 사용하는 특수 연산자다.

Containment Test는 `in`연산자를 사용하며, 안에 있는지를 확인한다.



Identity는 `is`연산자를 사용하며 본질적으로 같은지를 확인한다.

```python
a = 5
b = 5
print(a is b)
True

a = 1000
b = 1000
print(a is b)
False
```

`is`는 메모리의 주소를 확인하는데, `a = 1000`과 `b = 1000`의 id가 다르기 때문에 `False`값이 나온다.

5의 경우는 기존의 값들이 만들어진 값들이기 때문에 id가 같다.



### 연산자 우선순위

연산자에는 우선순위가 있다는 것을 알아두자.



## 기초 형변환(Type conversion)

예전에는 RAM의 크기가 작았기 때문에, 자료형의 크기가 중요했다. 메모리를 효율적으로 사용하기 위해 '형'을 구분하였다.

외부 데이터를 가져올 때 형변환이 필요하다.



## 시퀀스(sequence) 자료형...(중요...!!!)

리스트, 문자열을 제일 많이 사용한다. 순서가 있다.

### list(mutable)

원소 10개 이상이면 줄바꿈을 해주는 것이 좋다.



### tuple(immutable)

튜플은 수정할 수 없는 구조다.



### range()

`range(n, m, s)`에서 `s`는 step이다. `s`만큼 증가한다.

range는 list로 바꾸는 것이 가능하다.

```python
list(range(0,-9, )) # 기본적으로 더하는 것이기 때문에 []같이 빈 리스트가 나온다.
list(range(0,-9,-1)) # -1을 해주어야 한다.
```



## Set, Dictionary

Dictionary를 많이 사용한다. 순서가 없다.



### Set

중괄호로 이루어졌으며, 중복된 값을 포함한다.

### Dictionary(mutable)

Key와 Value로 이루어져있다.



## Practice

### 상승장? 하락장?

```python
import requests

url = "https://api.bithumb.com/public/ticker/btc"
data = requests.get(url).json()['data']
print(data) # 딕셔너리임을 알 수 있다.

start_price = int(data['opening_price'])
maximum = int(data['max_price'])
minimum = int(data['min_price'])

coin_range = maximum - minimum

if maximum < start_price + coin_range:
    print('상승장')
else:
    print('하락장')
```



### 모음 제거하기

```python
my_str = "Life is too short, you need python"
# a, e, i, o, u 제거
result = ""

vowels = ['a', 'e', 'i', 'o', 'u']

# for char in my_str:
#     if char in vowels: # char이 vowels 안에 있는 글자인가?
#         pass
#     else:
#         result += char
        
for char in my_str:
    if char not in vowels: # 조금 더 가벼워짐
        result += char
        
print(result)
```



### 개인정보보호

```python
phone = input()
if len(phone) == 11 and phone[0:3] == '010':
    print('*'*7 + phone[-4:])
#     print('%s\r*******' %phone)
else:
    print('핸드폰번호를 입력하세요')
```



### 정중앙

```python
text = input()

num = len(text) // 2 # 몫을 구한다.

if len(text) % 2 == 1:
    middle = text[num]
else:
    middle = text[num-1:num+1]
    
print(middle)
```

