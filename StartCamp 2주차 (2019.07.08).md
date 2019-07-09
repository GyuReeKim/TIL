# StartCamp 2주차 (2019.07.08)

## 프로그래밍 언어 3형식

1. 저장
2. 조건(if)
3. 반복(while)

## 프로그래밍 주의점

1. 대/소문자
2. 띄어쓰기(들여쓰기)
3. 스펠링

## 무엇을 저장하는가

1. 숫자
2. 글자
3. 참,거짓

## 저장 / 불러오기

1. 변수 : `dust = 40` / `dust`
2. 리스트 : `dust = [40, 50, 80]` / `dust[0]`
3. 딕셔너리 : `dust = {"영등포구" : 40, "강남구" : 50}` / `dust["영등포구"]`

## `print()` 함수

`print()`

`print("변수")`

##  `if()`

```python
if(True):
    print("조건문입니다.")
```

```python
if():
    print()
elif():
    print()
elif():
    print()
else:
    print()
```



##  `while()`

```python
n = 0
while n < 3:
    print("조건문입니다.")
    n = n + 1
```

## `for()`

```python
for i in range(3):
    print("조건문입니다.")
```

## API

서로 다른 응용체제 사이 약속.

서비스 간 대화방식.

1. 페이스북 로그인
2. 카카오톡 API
3. 네이버 지도
4. Riot API
5. 공공데이터 API

### 예제1

```python
greeting = 'Hello'

for i in range(15):
    print(i)
    print(greeting)
```

### 예제2

```python
import random

menu = ['한식', '중식', '일식', '양식']

choice = random.choice(menu) # .choice : 랜덤 중 하나 선택
print(f'오늘 점심은 {choice} 어떤가요?')
```

### 예제3

```python
import random

phonebook = {
    '가' : '010-1234-1234',
    '나' : '010-1234-5678',
    '다' : '010-5678-5678',
}
choice = random.choice(list(phonebook.keys()))
print(f'{choice} : {phonebook[choice]}')
```

###  예제4

```python
dust = 100
if dust > 150:
    print('매우나쁨')
elif dust > 80 and dust <= 150:
    print('나쁨')
elif 30 < dust <= 80:
    print('보통')
else:
    print('좋음')
```

### 예제5

```python
import random
numbers = list(range(1,46))

pick = random.sample(numbers,6)
print(sorted(pick))
```





