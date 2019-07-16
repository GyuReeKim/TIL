# Python 1주차 (2019.07.16)

## Test 파일 github에 올라가지 않도록 설정

### .gitignore 파일 생성

```
touch .gitignore
```

test파일을 올리고 싶지 않다면 gitignore 파일 맨 처음에 `/test`를 입력하면 된다. 'gitignore.io'에서 'Python'을 추가해준다.



#### test 파일을 올려버렸을 경우 해결방법

```
git status # 현재 git 파일을 확인
git reset test/ # test 파일을 리셋
git status
git add .gitignore # git ignore을 추가
```



## 파일 내려받기

```
rm -rf python_teacher
git clone 주소
cd python_teacher/
git pull origin master
```



## 제어문

조건문이 없다면 위에서부터 순차적으로 실행된다.



## 조건문

`if:`뒤의 문장은 자동으로 형 변환이 된다.

조건문을 쓸 때, `:`을 작성하는 습관을 들여야 한다.

`else:`는 `if:`가 있어야 쓸 수 있다.



```python
num = int(input("숫자를 입력하세요 : "))

if num % 2: # if num % 2 == 1: # '== 1'은 같은 의미로 생략 가능
    print('홀수입니다.')
else:
    print('짝수입니다.')
```



### Python Tutor

크롬창에 python tutor 검색해서 들어가면 'http://pythontutor.com/visualize.html#mode=edit'사이트 나옴

코드 복사해서 Visualize Execution을 누르면 코드가 실행되고 forward를 누르면 한줄 한줄 실행해 볼 수 있다.



### 조건 표현식

읽고 이해를 할 수 있으면 된다.



## 반복문

### while

조건식 이후에 `:`입력을 꼭 해야한다. 자주 사용하지는 않는다.



### for

```python
vowels = 'aeiou'
for vowel in vowels: # 앞에는 단수, 뒤에는 복수 형태로 적어주는 편이 좋다.
    print(vowel)
```

변수 이름을 명시적으로 작성하는 것이 좋다.

```python
for i in range(1,31):
    if not i % 3:
        print(i)
```



#### enumerate()

>  python의 기본 내장함수다.

```python
# enumerate()를 활용해서 출력해봅시다.
lunch = ['짜장면', '초밥']
for idx, menu in enumerate(lunch):
    print(idx, menu)
```

enumerate()를 사용하면, 두가지의 값을 동시에 return 해준다.



#### dictionary

dictionary는 순서가 없기 때문에 key값으로 접근해야 한다.

```
students = {'student1': '김승연', 'student2': '최동호', 'student3': '김규리'}
for student in students:
    print(student) # 각각의 key값들을 제공한다.
```



#### dictionary for문

```python
# 여기에 코드를 작성하세요.
blood_type = {"A": 4, "B": 2, "AB": 3, "O":1}

# 0 # 문자열을 이용하여 옆으로 나열하는 방식으로 출력가능하다.
a = '혈액형의 종류는 =>'
for blood in blood_type:
    a += blood + ' ' # 문자열이기 때문에 '+' 기호로 연결할 수 있다.
print(a)

# 1 # 위와 동일한 결과를 출력한다.
print(blood_type.keys())
b = '혈액형의 종류는 =>'
for blood in blood_type.keys():
    b += blood + ' '
print(b)

# 2
total = 0
for num in blood_type.values(): # num은 blood_type의 value값이다.
    total = total + num # value 값들을 더해준다.
print(f'총 인원은 {total}명입니다.')

# 3
for key, value in blood_type.items():
    if key == "A" or key == "B":
        print(f'{key}형은 {value}명입니다.', end = ' ')
```



#### break

특정 조건에서 for문을 탈출하기 위해 사용한다.



#### continue

특정 조건의 요소들을 건너뛸 때 사용한다. 특정 조건에 걸리면 for문으로 바로 접근한다.



#### else

끝까지 반복문을 시행한 이후에 실행된다.

```python
numbers = [1, 5, 10]

for num in numbers:
    if num == 3:
        print(True)
        break
else:
    print(False)
```

만약 if문 안의 else라면 num을 반복할 때 마다 False가 출력될 것이다.

Python Tutor을 활용해 차이점을 확인할 수 있다.



## 함수 (function)

### 함수의 선언과 호출

`def`로 시작하고 `:`로 끝난다. 

매개변수는 함수에 사용자가 넣어주는 변수를 말한다. Output에서는 return이 하나만 있어야 한다. return을 만나면 함수의 할 일이 다 끝난 것이다.

```python
def rectangle(width, height):
    area = width * height
    perimeter = (width + height) * 2
    print(f'둘레: {perimeter}, 넓이: {area}')
    
rectangle() # 오류가 발생한다. 괄호 안에 값들을 넣어줘야 한다.
rectangle(20, 50)
```



### 함수의 return

return과 print는 비슷해 보일 수 있으나 다르다.

print는 콘솔 창에 print의 값을 보여준다. return은 함수의 결과로, return을 만나면 함수가 종료가 된다.

print는 재사용하지 않지만, return은 재사용이 가능하다.



### 함수의 인수

위치기반으로 인자를 탐색한다.



## Practice

### 모든코인 상승장? 하락장? (다음시간에)

````python
del data["date"]

for key, data_price in data.items():
    for d, price in data_price.items():
        if d == "opening_price":
            open_price = price
        elif d == "min_price":
            minimum_price = price
        elif d == "max_price":
            maximum_price = price
    
    start_price = float(open_price)
    maximum = float(maximum_price)
    minimum = float(minimum_price)
    coin_range = maximum - minimum

    if maximum < start_price + coin_range:
        print("상승장")
    else:
        print("하락장")
# key의 값을 출력하지 못했다.
````



### 평균점수

```python
student = {'python':80, 'algorithm':78, 'django':95, 'flask':80}

result = 0
for score in student.values():
    result += score 
print(result/len(student))
```



### 혈액형

```python
blood = ['A','A','B','O','A','B','A','AB','AB','O','A','O','AB','O']

blood_dict = {}

for b in blood:
    if b in blood_dict:
        blood_dict[b] += 1 # 'A'가 'blood_dict'에 있다면, 'A: 1'의 value값에 1을 더해준다.
    else:
        blood_dict[b] = 1 # 'A'가 'blood_dict'에 없다면 dictionary에 'A: 1'을 생성한다.
print(blood_dict)

```



### UBD

````python
movies = {
    "7번방의선물":12811206,
    "괴물":13019740,
    "국제시장":14257115,
    "극한직업":16261018,
    "도둑들":12983330,
    "명량":17613682,
    "베테랑":13414009,
    "신과함께-죄와벌":14410754,
    "아바타":13624328,
    "어벤져스:엔드게임":13901423,
}

UBD = 172212

for name, count in movies.items(): # key와 value가 둘다 필요한 경우 items()를 사용
    if count < UBD * 80:
        print(name)
````

