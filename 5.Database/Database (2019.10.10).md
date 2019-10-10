# SQL (2019.10.10)

## Django를 활용함

````bash
$ django-admin startproject sqlorm .
$ django-admin startapp users
$ python manage.py makemigrations
$ python manage.py migrate
$ python manage.py sqlmigrate users 0001
# sql문으로 번역 {app_name} {migration_name}
# migrations 폴더의 0001_initial.py에서 확인 가능
$ python manage.py shell_plus
$ sqlite3 db.sqlite3
````



Settings.py의 INSTALLED_APPS에 'django_extensions'를 넣어준다.



```sql
sqlite> .tables
auth_group                  django_admin_log
auth_group_permissions      django_content_type
auth_permission             django_migrations
auth_user                   django_session
auth_user_groups            users_user
auth_user_user_permissions

sqlite> .mode csv

sqlite> .import users.csv users_user
users.csv:1: INSERT failed: datatype mismatch

sqlite> SELECT COUNT(*) FROM users_user;
100
```



## sql

> 터미널을 분할해 사용한다.

```bash
$ sqlite3 db.sqlite3
```



#### 기본 CRUD 로직

```sql
-- 모든 user 레코드 조회
sqlite> SELECT * FROM users_user;

-- user 레코드 생성
sqlite> INSERT INTO users_user
   ...> VALUES(101,'길동','홍',30,'제주도','010-1234-1234',10000);

-- 해당 user 레코드 조회
sqlite> SELECT * FROM users_user
   ...> WHERE id=102;
102,"길동","홍",100,"제주",010-1234-1234,123123

-- 해당 user 레코드 수정
sqlite> UPDATE users_user
   ...> SET last_name="김"
   ...> WHERE id=102;

-- 해당 user 레코드 삭제
sqlite> DELETE FROM users_user
   ...> WHERE id=102;

```



#### 조건에 따른 쿼리문

```sql
-- 전체 인원 수
sqlite> SELECT COUNT(*) FROM users_user;
100

-- 나이가 30인 사람의 이름
sqlite> SELECT first_name FROM users_user
   ...> WHERE age=30;

-- 나이가 30살 이상인 사람의 인원 수
sqlite> SELECT COUNT(*) FROM users_user
   ...> WHERE age >= 30;
43

-- 나이가 30이면서 성이 김씨인 사람의 인원 수
sqlite> SELECT COUNT(*) FROM users_user
   ...> WHERE age=30 AND last_name='김';
1

-- 지역번호가 02인 사람의 인원 수
sqlite> SELECT COUNT(*) FROM users_user
   ...> WHERE phone LIKE '02-%';
24

-- 거주 지역이 강원도이면서 성이 황씨인 사람의 이름
sqlite> SELECT first_name FROM users_user
   ...> WHERE country='강원도' AND last_name='황';
"은정"
```



#### 정렬 및 LIMIT, OFFSET

```sql
-- 나이가 많은 사람 10명
sqlite> SELECT * FROM users_user
   ...> ORDER BY age DESC LIMIT 10;

-- 잔액이 적은 사람 10명
sqlite> SELECT * FROM users_user
   ...> ORDER BY balance LIMIT 10;
   
-- 성, 이름 내림차순 순으로 5번째 있는 사람
sqlite> SELECT * FROM users_user
   ...> ORDER BY last_name DESC, first_name DESC
   ...> LIMIT 1 OFFSET 4;
67,"보람","허",28,"충청북도",016-4392-9432,82000
```



#### 표현식

```sql
-- 전체 평균 나이
sqlite> SELECT AVG(age) FROM users_user;
28.23

-- 김씨의 평균 나이
sqlite> SELECT AVG(age) FROM users_user
   ...> WHERE last_name='김';
28.7826086956522

-- 계좌 잔액 중 가장 높은 값
sqlite> SELECT MAX(balance) FROM users_user;
1000000

-- 계좌 잔액 총액
sqlite> SELECT SUM(balance) FROM users_user;
14425040
```



## orm

> 터미널을 분할해 사용한다.

```bash
$ python manage.py shell_plus
```



### 기본 CRUD 로직

```shell
In [1]: User
Out[1]: users.models.User

# 모든 user 레코드 조회
In [2]: User.objects.all()

# user 레코드 생성
In [6]: User.objects.create(last_name="홍", first_name="길동", age=100, country="제주", phone="010-1234-1234", balance=123123)
Out[6]: <User: User object (102)>

# 해당 user 레코드 조회
In [7]: User.objects.get(id=102)
Out[7]: <User: User object (102)>

# 해당 user 레코드 수정
In [8]: user = User.objects.get(id=102)

In [9]: user.age = 10

In [10]: user.save()

In [11]: user.age
Out[11]: 10

In [12]: User.objects.get(id=102)
Out[12]: <User: User object (102)>

In [13]: user = User.objects.get(id=102)

In [15]: user.last_name
Out[15]: '김'

# 해당 user 레코드 삭제
In [16]: user.delete()
Out[16]: (1, {'users.User': 1})

In [17]: user = User.objects.get(id=101)

In [18]: user.delete()
Out[18]: (1, {'users.User': 1})

In [19]: User.objects.get(id=102).delete()
```



### 조건에 따른 쿼리문

```shell
# 전체 인원 수
In [20]: User.objects.all().count()
Out[20]: 100

In [21]: len(User.objects.all())
Out[21]: 100

In [22]: User.objects.count()
Out[22]: 100

# 나이가 30인 사람의 이름
In [25]: User.objects.filter(age=30).values('first_name')
Out[25]: <QuerySet [{'first_name': '영환'}, {'first_name': '보람'}, {'first_name': '은영'}]>

# 나이가 30살 이상인 사람의 인원 수
In [28]: User.objects.filter(age__gte=30).count()
Out[28]: 43

# 나이가 30이면서 성이 김씨인 사람의 인원 수
In [33]: User.objects.filter(age=30).filter(last_name='김').count()
Out[33]: 1

In [34]: User.objects.filter(age=30, last_name='김').count()
Out[34]: 1

# 지역번호가 02인 사람의 인원 수
In [37]: User.objects.filter(phone__startswith='02-').count()
Out[37]: 24

# 거주 지역이 강원도이면서 성이 황씨인 사람의 이름
In [41]: User.objects.filter(country='강원도',
    ...: last_name='황')
Out[41]: <QuerySet [<User: User object (22)>]>

In [42]: User.objects.filter(country='강원도',
    ...: last_name='황').values('first_name')
Out[42]: <QuerySet [{'first_name': '은정'}]>

In [45]: User.objects.filter(country='강원도', last_name='황').values('first_name')[0]
Out[45]: {'first_name': '은정'}

In [46]: User.objects.filter(country='강원도', last_name='황')[0].first_name
Out[46]: '은정'
```



### 정렬 및 LIMIT, OFFSET

```shell
# 나이가 많은 사람 10명
In [49]: User.objects.order_by('-age')[:10]
Out[49]: <QuerySet [<User: User object (1)>, <User: User object (4)>, <User: User object (28)>,
<User: User object (53)>, <User: User object (65)>, <User: User object (26)>, <User: User object (55)>, <User: User object (58)>, <User: User object (74)>, <User: User object (82)>]>

# 잔액이 적은 사람 10명
In [50]: User.objects.order_by('balance')[:10]
Out[50]: <QuerySet [<User: User object (99)>, <User: User object (48)>, <User: User object (100)>, <User: User object (5)>, <User: User object (24)>, <User: User object (61)>, <User: User object (92)>, <User: User object (46)>, <User: User
object (38)>, <User: User object (60)>]>

# 성, 이름 내림차순 순으로 5번째 있는 사람
In [51]: User.objects.order_by('-last_name', '-first_name')[4]
Out[51]: <User: User object (67)>
```



### 표현식

```shell
In [52]: from django.db.models import Avg, Min, Max, Sum

# 전체 평균 나이
In [54]: User.objects.aggregate(Avg('age'))
Out[54]: {'age__avg': 28.23}

# 김씨의 평균 나이
In [58]: User.objects.filter(last_name='김').ag
    ...: gregate(Avg('age'))
Out[58]: {'age__avg': 28.782608695652176}

# 계좌 잔액 중 가장 높은 값
In [59]: User.objects.aggregate(Max('balance'))
Out[59]: {'balance__max': 1000000}

# 계좌 잔액 총액
In [60]: User.objects.aggregate(Sum('balance'))
Out[60]: {'balance__sum': 14425040}
```





### 둘다 배우는 이유?

sql은 sql문법을 이용하고 orm은 python문법을 이용한다.

database(RDBMS)는 sql문법으로만 조작 가능하다.

orm은 python문법을 sql로 바꿔주는 역할을 한다.



## django orm lookup

[QuerySet API reference | Django documentation | Django]

https://docs.djangoproject.com/en/2.2/ref/models/querysets/

Lookups를 참고하여 orm을 작성할 수 있다.

