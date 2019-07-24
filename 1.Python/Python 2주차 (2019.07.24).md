# Python 2주차 (2019.07.24)

## class Circle

```python
# class Circle 정의
class Circle:
    pi = 3.14
    r = 0
    x = 0
    y = 0
    
    def __init__(self, r, x, y):
        self.r = r
        self.x = x
        self.y = y
        
    def area(self):
        return self.pi * self.r ** 2
    
    def circumference(self):
        return 2 * self.pi * self.r
    
    def center(self):
        return (self.x, self.y)
    
    def move(self, x, y):
        return (x, y)

c1 = Circle(3, 2, 4)
print(c1.area())
print(c1.circumference())
print(c1.center())
print(c1.move(0, 0))
```



```python
print(c1.area)
```

만약 위의 코드처럼 method를 괄호 없이 실행한다면, 이 코드는 area 함수를 수행하지 않고, 이 함수가 무엇인지 물어보게 된다. 그러므로 meathod 뒤에는 괄호를 항상 붙여줘야 한다.



## OOP advanced

### 클래스 변수/ 인스턴스 변수

클래스는 `Class.`로 접근한다.

인스턴스 변수는 `self.`로 접근한다.



### 인스턴스 메서드, 클래스 메서드, 스태틱(정적) 메서드

> 인스턴스 메서드를 주로 사용할 예정이다.

```python
# 인스턴스 메서드
class MyClass:
      def instance_method_name(self, arg1, arg2, ...):

# 클래스 메서드 # 잘 쓰지 않는다.
class MyClass:
      @classmethod # 적어줘야 한다. # 첫 번째 인자로 cls를 받으며, 클래스 객체가 cls가 된다.
      def class_method_name(cls, arg1, arg2, ...): # 'class_'를 앞에 적어준다.

# 스태틱 메서드 # 잘 쓰지 않는다. # 인자가 자동으로 넘어가지 않는다.
class MyClass:
      @staticmethod
      def static_method_name(arg1, arg2, ...):
```



#### 스태틱 메서드 예제

````python
class Calculator:
        
    @staticmethod # 명시적으로 스태틱 메소드라고 알려주는 표시
    def add(a, b):
        return a + b
    
    @staticmethod
    def sub(a, b):
        return a - b
    
    @staticmethod
    def mul(a, b):
        return a * b
    
    @staticmethod
    def div(a, b):
        try: # try문으로 0으로 나눌 경우 다른 리턴을 주도록 설정한다.
            a / b
            return a / b
        except:
            return '0으로 나눌 수 없음'
````



## 실습문제

```python
class Stack:
    
    def __init__(self): # data의 공간을 따로 만들어준다. Class 변수에는 data가 존재하지 않는다.
        self.data = []
    
    def empty(self):
        if self.data: # data가 있으면
            return False
        else:
            return True
    
    def top(self):
        if self.data:
            self.data[-1]
#         else: # 함수는 리턴이 없으면 자연스럽게 None을 리턴해준다.
#             return None
        
    def pop(self):
        if not self.empty():
#             return self.data.pop() # pop 리턴
            last = self.data[-1]
            self.data = self.data[0:-1]
            return last
        
#         if self.data:
#             num = 0
#             for sl in self.data: # for문을 위의 리스트를 활용하여 간단하게 표현할 수 있다.
#                 num += 1
#             self.data.pop(num - 1)
#             return sl
        
    def push(self, item):
        self.data.append(item)
```



## 연산자 오버라이딩(중복 정의, 덮어 쓰기)

```python
+  __add__
-  __sub__
*  __mul__
<  __lt__
<= __le__
== __eq__
!= __ne__
>= __ge__
>  __gt__
```



```python
class Person:
    population = 0
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
        Person.population += 1
        
    def __gt__(self, other):
        if self.age > other.age:
            return True
        else:
            return False
        
p1 = Person('노인', 100)
p2 = Person('아기', 5)
p1 > p2 # 앞의 값과 뒤의 값을 비교해준다.
```



## 상속

### 기본

```python
class Person:
    population = 0
    def __init__(self, name='사람'):
        self.name = name
        Person.population += 1
        
    def greeting(self):
        print(f'안녕하세요 {self.name}입니다.')
        
p1 = Person('change')
p1.greeting() # 안녕하세요 change입니다.

class Student(Person): # Person의 데이터가 포함된다.
    def __init__(self, student_id, name='사람'):
        self.student_id = student_id
        self.name = name
        
s = Student(123456, '김싸피')
s.greeting() # 안녕하세요 김싸피입니다.
```



### super()

```python
class Person:
    
    brain = True
    
    def __init__(self, name, age, number, email):
        self.name = name
        self.age = age
        self.number = number
        self.email = email 
        
    def greeting(self):
        print(f'안녕, {self.name}')
        
    def walk(self):
        print('뚜벅뚜벅')
        
class Student(Person):
    
    def __init__(self, name, age, number, email, student_id):
        super().__init__(name, age, number, email)
        # super는 자신이 상속받는 부모를 말함 # person이 가진 init 함수를 불러옴
        self.student_id = student_id
        
p1 = Person('홍길동', 200, '0101231234', 'hong@gildong')
s1 = Student('학생', 20, '12312312', 'student@naver.com', '190000')

p1.greeting() # 안녕, 홍길동
s1.greeting() # 안녕, 학생
```



### 메서드 오버라이딩

```python
class Soldier(Person):
    def __init__(self, name, age, number, email, army_id):
        super().__init__(name, age, number, email)
        self.army_id = army_id
    
    def greeting(self): # 부모와 같지만 위에 덮어씌워졌다.
        print(f'충성! 이병 {self.name}')
        
    def walk(self):
        print('성큼성큼')
        
s1 = Soldier('굳건이', 20, 123123, 'email@email.com', 123456)
s1.greeting() # 충성! 이병 굳건이
s1.walk() # 성큼성큼
s1.brain # True
```



### 다중 상속

```python
# Person 클래스를 정의합니다.
class Person:
    def __init__(self, name):
        self.name = name
        
    def breath(self):
        print('후하후하')
        
    def greeting(self):
        print(f'{self.name}입니다.')
        
# Mom 클래스를 정의합니다.
class Mom(Person):
    gene = 'XX'
    
    def swim(self):
        print('어푸어푸')
        
# Dad 클래스를 정의합니다.
class Dad(Person):
    gene = 'XY'
    
    def climbing(self):
        print('산이 있으니 올라간다.')
        
# Child 클래스를 정의합니다.
class Child(Dad, Mom): # 첫번째로 들어간 것이 상속의 우선순위다.
    def swim(self):
        print('첨벙첨벙')
        
    def cry(self):
        print('응애응애')        
```

```python
# 상속순서 'Mom > Dad'
c = Child('아기')
c.cry() # 응애응애
c.swim() # 첨벙첨벙 # Mom의 Swim()이 덮어씌워졌다.
c.climbing() # 산이 있으니 올라간다. # Child에 climbing이 없어 덮어씌워지지 않았다.
c.gene # 'XX' # Mom의 Gene을 참조했다.

# 상속순서 'Dad > Mom'
sc = Child('둘째')
sc.cry() # 응애응애
sc.swim() # 첨벙첨벙
sc.climbing() # 산이 있으니 올라간다.
sc.gene # 'XY' # Dad의 Gene을 참조했다.
```



## 포켓몬 구현하기

### Poketmon

```python
class Poketmon:
    
    def __init__(self, name):
        self.name = name
        self.level = 5
        self.hp = self.level * 20
        self.exp = 0
        
    def now_level(self):
        print(f'{self.name}의 레벨은 {self.level}입니다.')
        
    def bark(self):
        print(f'{self.name}은 말했다. 피카피카')
        
        
    def level_up(self):
        if self.exp >= self.level * 100:
            self.level += 1
            self.exp = 0
            print(f'{self.name}의 레벨이 {self.level}으로 올랐습니다.')
        else:
            print(f'{self.name}의 경험치가 {self.exp}입니다. {self.level+1}레벨까지 {self.level * 100 - self.exp}만큼 남았습니다.')
    
    def exp_get(self, enemy):
        if enemy.hp <= 0:
            print(f'{enemy.name}은 죽었습니다. {enemy.name}이 부활합니다.')
            enemy.hp = enemy.level * 20
            self.exp += enemy.level * 15
            self.level_up()
    
    def body_attack(self, enemy):
        enemy.hp -= self.level * 5
        print(f'{self.name}의 공격으로 {enemy.name}의 hp는 {enemy.hp}로 줄었다.')
        self.exp_get(enemy)
        
        
    def thousand_volt(self, enemy):
        enemy.hp -= self.level * 7
        print(f'{self.name}의 공격으로 {enemy.name}의 hp는 {enemy.hp}로 줄었다.')
        self.exp_get(enemy)
        
p1 = Poketmon('피카피카')
p2 = Poketmon('야생')        
```



### WaterPoketmon

```python
class WaterPoketmon(Poketmon):
    def __init__(self, name):
        super().__init__(name)
        self.attribute = '물'
        self.attribute_power = self.level * 3
        
    def att(self):
        print(f'{self.name}의 속성은 {self.attribute}입니다.')
        
    def attack(self, enemy):
        enemy.hp -= self.attribute_power
        print(f'{self.name}의 공격으로 {enemy.name}의 hp는 {enemy.hp}로 줄었다.')
        self.exp_get(enemy)
        
w = WaterPoketmon('물폭탄')        
```



### SalmonKing

```python

class SalmonKing(WaterPoketmon):
    def __init__(self, name):
        super().__init__(name)
        self.skill_name = '살몬살몬'
        self.water_skill = self.level * 10
        
    def salmon_attack(self, enemy):
        enemy.hp -= self.water_skill
        print(f'{self.name}의 {self.skill_name}공격으로 {enemy.name}에게 엄청난 피해를 입혔다.')
        print(f'{enemy.name}의 hp는 {enemy.hp}로 줄었다.')
        self.exp_get(enemy)
        
s = SalmonKing('초대형연어')        
```



### 실행 코드

```python
p1.level_up() # 다음 레벨까지의 경험치 출력
p2.level_up()
p1.body_attack(p2) # 기본 공격 출력
p2.bark() # bark() 출력
p1.bark()
p2.thousand_volt(p1) # 강한 공격 출력
p1.now_level() # 현재 레벨 출력
p2.now_level()
```

```python
w.body_attack(p2)
w.thousand_volt(p2)
w.attack(p2) # 속성 공격 출력
w.now_level()
```

```python
s.body_attack(p2)
s.thousand_volt(p2)
s.attack(p2)
s.salmon_attack(p2) # 스킬 공격 출력
s.now_level()
```

