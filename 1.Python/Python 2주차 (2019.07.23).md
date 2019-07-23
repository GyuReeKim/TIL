# Python 2주차 (2019.07.23)

##  원격저장소 이름 변경

```
git remote rename origin github
# 원격저장소: remote
#이름을 바꾸기: rename
# 원래이름을: origin
#바꿀 이름으로: git

origin 이름을 가진 폴더 모두 각각 적용해주어야 한다.
```



## map(), zip(), filter()

### map(function, iterable)

> map을 사용하면 메모리도 줄이고 실행 시간을 단축시킬 수 있어 많이 쓴다. 적어도 읽을 줄 알아야 한다. 전부 적용하는 경우에 사용한다.

```python
numbers = [1, 2, 3]
# 위의 코드를 문자열 '123'으로 만들어봅시다.
''.join(list(map(str, numbers)))
# list comprehension
''.join([str(number) for number in numbers])

chars = ['1', '2', '3']
# 위의 코드를 [1, 2, 3]으로 만들어봅시다.
result = list(map(int, chars))
# list comprehension
result = [int(char) for char in chars]

# 세제곱의 결과를 나타내는 함수
def cube(n):
    return n**3
numbers = [1, 2, 3, 4, 5]
list(map(cube, numbers))
```



### zip(*iterables)

> 거의 사용하지 않는다.

```python
# for문으로 한 명씩 순서대로 매칭시켜봅시다.
{girl: boy for girl in girls for boy in boys}

# 첫번째 쌍, 두번째 쌍, 세번째 쌍을 구분하기 위해 zip을 사용한다.
girls = ['jane', 'iu', 'mary']
boys = ['justin', 'david', 'kim']
print(list(zip(girls, boys)))

# 길이가 같을 때
a = '123'
b = '567'
for digit_a, digit_b in zip(a, b):
    print(digit_a, digit_b)
```



### filter(function, iterable)

> 조건에 맞는 경우 반환해 걸러준다.

```python
# 짝수인지 판단하는 함수를 작성해봅시다.
def even(n):
    return not n%2

numbers = [1, 2, 3, 4, 5]
list(filter(even, numbers))

# 다음의 list comprehension과 동일하다.
result = [number for number in numbers if number % 2 == 0]
result = [number for number in numbers if not number % 2]
print(result)
```



## OOP with python - 패러다임

> Object-Oriented Programming은 컴퓨터 프로그래밍의 패러다임이라고 할 수 있다.
>
> 객체라는 용어는 생소하니 Object라는 단어를 사용한다.



Class: 정의. 속성과 행위, 메소드를 가질 수 있다.

Instance: 예시, 만들어졌다. 생성한다. 실존하도록 만들어 준 것이다.

method: instance 가 수행하는 기능



괄호가 없으면 attribute로 접근하고 괄호가 있으면 method로 접근한다.

```python
my_list = [5, 4, 3, 2, 1]
print(type(my_list))

my_list.sort() # my_list 원본을 정렬해주므로 원본이 바뀐다.
print(my_list)
```



```python
print(dir([1, 2, 3]))

# ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']

method : 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort'
# list가 공통적으로 가지는 능력이다.
```



##  클래스 및 인스턴스

```python
# Class를 만들어봅시다. # 클래스 선언
class TestClass: # 단어의 시작은 대문자로 적어주어야 한다. # CamelCase # '_'를 사용하는 SnakeCase도 변수 작성할 때 많이 쓸 예정
    """
    이것은 테스트 클래스입니다.
    """
    name = 'TestClass'
```



### 인스턴스 생성하기

```python
# Phone 클래스를 만들어봅시다.
class Phone: # 괄호는 없어도 된다.
    power = False
    number = ''
    book = {}
    model = 's10'
    
    def on(self):
        if not self.power: # false 이면?
            self.power = True
            print('============')
            print(f'{self.model}')
            print('============')
            
    def off(self):
        if self.power:
            self.power = False
            print('꺼졌습니다.')
```



### Phone 복사본 my_phone 만들기

```python
# 클래스 Phone 인스턴스 'my_phone' 를 만들어봅시다.
my_phone = Phone() # 소괄호를 만든 순간 새로운 인스턴스가 생성된다. 규칙은 Phone
print(my_phone.model)
```



### 변수 확인하기

```python
# 켜져 있나 확인해 봅시다.
my_phone.power
```



### Method 확인하기

```python
# 새 폰은 꺼져있으니 켜 봅시다.
my_phone.on()
```



### 변수 바꾸기

```python
# 모델을 바꿔 봅시다.
my_phone.model = 'iPhone XR'
```



### 객체 출력

```python
# python 출력의 비밀 __repr__ 과 __str__
# 특정 객체를 print() 할 때 보이는 값과
# 그냥 객체 자체가 보여주는 값도 사실 모두 우리가 바꿀 수 있습니다.
class Phone:
    power = False
    number = ''
    book = {}
    model = 's10'
    
    def on(self):
        if not self.power: # false 이면?
            self.power = True
            print('============')
            print(f'{self.model}')
            print('============')
            
    def off(self):
        if self.power:
            self.power = False
            print('꺼졌습니다.')
            
    def __str__(self):
        return 'print문에 넣으면 얘가 실행됩니다.'
    
    def __repr__(self):
        return '그냥 객체만 놔두면 얘가 실행됩니다.'
```



### self : 인스턴스 객체 자기자신

> method에서 self를 첫번째 인자로 설정한다.

```python
# 클래스 선언코드에서 메서드를 정의할 때, 왜 self 를 꼭 써준걸까요?
# self를 쓰지 않으면 def greeting 밖의 name을 불러올 수 없기 때문에 self를 써줘야 한다.
class Person:
    name = 'unknown'
    
    def greeting(self):
        return f'my name is {name}'
    
p1 = Person()
p1.greeting()
# 동작하지 않습니다. 생성된 객체는 method scope 안에서 name 이 뭔지 모릅니다.
```

아래처럼 해결할 수 있다.

```python
class Person:
    name = 'unknown'
    
    def greeting(self):
        return f'my name is {self.name}'  # 이번에는 *내*이름 을 찾아봅니다.
```

인스턴스화 되었다는 것은 개인 공간이라고 생각할 수 있다.



### 생성자 / 소멸자

```python
# 생성자와 소멸자를 만들어봅시다.
class Person:
    def __init__(self):
        print('응애')
    def __del__(self):
        print('RIP')

# 생성해 봅시다.
p1 = Person() # 괄호를 쓰면서 '__init__'가 자동 실행되었다.

# 소멸시켜 봅시다.
del p1

# 생성자 역시 메서드(함수)기 때문에 추가인자를 받을 수 있습니다.
class Person:
    def __init__(self, name):
        self.name = name # 너 자신이 가지고 있는 name에 사용자가 입력한 name을 넣어줘라.
        print(f'응애! 나는 {self.name}')
    def __del__(self):
        print(f'{self.name}은 떠난다...')
```



## 포켓몬 구현하기

### 출력 오류

```python
# p1 = Person() # 앞에서 저장했던 값이다.
p1 = Pikachu()
# 이름은 떠난다... # Person(p1)이 출력된다.
```



### 코드 초기값

```python
# 내가 수정보완한 코드
class Pikachu:
    def __init__(self, name):
        self.name = name
        self.level = 5
        self.hp = self.level * 20
        self.exp = 0
        
    def bark(self):
        print('pikapika')
        
    def body_attack(self, enemy): # p1의 데이터를 얻기 위해서 p2의 데이터가 필요하다.
        if type(enemy) == Pikachu:
            print(f'공격전 {enemy.name}의 hp는 {enemy.hp}입니다.')
            enemy.hp = enemy.hp - self.level * 5
            print(f'공격후 {enemy.name}의 hp는 {enemy.hp}입니다.')
            
            # 적의 체력이 0보다 작아질 때
            if enemy.hp < 0:
                print(f'{enemy.name}는 죽었습니다.')
                enemy.hp = enemy.level * 20
                self.exp += enemy.level * 15
                
                # 경험치가 다 찼을 때
                if self.exp >= self.level * 10:
                    self.level += 1
                    self.exp = 0
                    print(f'{self.name}의 레벨이 {self.level}으로 올랐습니다.')
                    
                # 경험치가 덜 찼을 때
                else:
                    print(f'{self.name}의 경험치는 {self.exp}입니다.')
            
            # 내 체력이 0보다 작아질 때
            elif self.hp < 0:
                print(f'{self.name}는 죽었습니다.')
                self.hp = self.level * 20
        else:
            print('피카츄를 넣어주세요')
            
    def thousond_volt(self, enemy): # p1의 데이터를 얻기 위해서 p2의 데이터가 필요하다.
        if type(enemy) == Pikachu:
            print(f'공격전 {enemy.name}의 hp는 {enemy.hp}입니다.')
            enemy.hp = enemy.hp - self.level * 7
            print(f'공격후 {enemy.name}의 hp는 {enemy.hp}입니다.')
            if enemy.hp < 0:
                print(f'{enemy.name}는 죽었습니다.')
                enemy.hp = enemy.level * 20
                self.exp += enemy.level * 15
                if self.exp >= self.level * 10:
                    self.level += 1
                    self.exp = 0
                    print(f'{self.name}의 레벨이 {self.level}으로 올랐습니다.')
                else:
                    print(f'{self.name}의 경험치는 {self.exp}입니다.')
            elif self.hp < 0:
                print(f'{self.name}는 죽었습니다.')
                self.hp = self.level * 20
        else:
            print('피카츄를 넣어주세요')
        
p1 = Pikachu('지우의피카츄')
p2 = Pikachu('야생의피카츄')

print(p1.name)
print(p2.name)
p1.body_attack(p2)
p2.thousond_volt(p1)
```



### 이후에 반복해서 실행

> 피카츄의 체력이 변하고, 죽고, 레벨이 오르는 코드가 실행된다.

```python
p1.body_attack(p2)
p2.thousond_volt(p1)
```

