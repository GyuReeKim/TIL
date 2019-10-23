# Django (2019.10.22)

post는 user가 있어야 하므로 accounts를 먼저 만든다.

## Model에 user 가져오기

> 몇가지 방법이 있다.

### 1번

```python
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    user = models.ForeignKey(User)
```



### 2번

```python
class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    user = models.ForeignKey('auth.User') # auth라는 앱의 User를 불러온다.
```



### 3번

```python
class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE) # 변수에 데이터를 넣어서 사용한다.
```



## Doctor & Patient (N:M)

> 1:N은 상하관계가 뚜렷하다.

### model 생성

```python
class Doctor(models.Model):
    name = models.CharField(max_length=100)

class Patient(models.Model):
    name = models.CharField(max_length=100)

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)

```



### django extensions

> django_extensions를 settings에 넣어준 후 shell을 실행해준다.

```bash
$ python manage.py shell_plus

In [1]: Doctor
Out[1]: manytomany.models.Doctor

In [2]: d1 = Doctor.objects.create(name="john")

In [3]: d1
Out[3]: <Doctor: Doctor object (1)>

In [4]: p1 = Patient.objects.create(name="change")

In [5]: p1
Out[5]: <Patient: Patient object (1)>

In [6]: exit()
```



### model에 함수 추가

```python
class Doctor(models.Model):
    name = models.CharField(max_length=100)
    def __str__(self):
        return self.name

class Patient(models.Model):
    name = models.CharField(max_length=100)
    def __str__(self):
        return self.name

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    def __str__(self):
        return f"{self.doctor.name}의사 : {self.patient.name}환자"
```

```bash
$ python manage.py shell_plus

In [1]: d1 = Doctor.objects.get(id=1)

In [2]: d1
Out[2]: <Doctor: john>

In [3]: p1 = Patient.objects.get(id=1)

In [4]: p1
Out[4]: <Patient: change>

In [5]: Reservation
Out[5]: manytomany.models.Reservation

In [6]: r1 = Reservation.objects.create(doctor=d1, patient=p1)

In [7]: r1
Out[7]: <Reservation: john의사 : change환자>

In [8]: doctor1 = Doctor.objects.create(name="tak")
   ...: doctor2 = Doctor.objects.create(name="zzulu")
   ...: doctor3 = Doctor.objects.create(name="neo")
   ...: patient1 = Patient.objects.create(name="junho")
   ...: patient2 = Patient.objects.create(name="jason")
   ...: patient3 = Patient.objects.create(name="justin")

In [9]: reservation1 = Reservation.objects.create(doctor=doctor1, patient=patient1)
   ...: reservation2 = Reservation.objects.create(doctor=doctor1, patient=patient2)
   ...: reservation3 = Reservation.objects.create(doctor=doctor2, patient=patient3)
   ...: reservation4 = Reservation.objects.create(doctor=doctor2, patient=patient1)
   ...: reservation5 = Reservation.objects.create(doctor=doctor3, patient=patient3)

In [10]: doctor1
Out[10]: <Doctor: tak>

In [11]: patient1
Out[11]: <Patient: junho>

In [12]: doctor1.reservation_set.all()
Out[12]: <QuerySet [<Reservation: tak의사 : junho환자>, <Reservation: tak의사 : jason환자>]>

In [13]: for reservation in doctor1.reservation_set.all():
    ...:     print(reservation.patient.name)
    ...: 
junho
jason

In [14]: for reservation in patient1.reservation_set.all():
    ...:     print(reservation.doctor.name)
    ...: 
tak
zzulu
```



### model의 Patient 클래스에 변수 추가

```python
class Doctor(models.Model):
    name = models.CharField(max_length=100)
    def __str__(self):
        return self.name

class Patient(models.Model):
    name = models.CharField(max_length=100)
    doctors = models.ManyToManyField(Doctor, through='Reservation')
    def __str__(self):
        return self.name

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    def __str__(self):
        return f"{self.doctor.name}의사 : {self.patient.name}환자"
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
$ python manage.py shell_plus

In [1]: doctor1 = Doctor.objects.create(name="tak")
   ...: doctor2 = Doctor.objects.create(name="zzulu")
   ...: doctor3 = Doctor.objects.create(name="neo")
   ...: patient1 = Patient.objects.create(name="junho")
   ...: patient2 = Patient.objects.create(name="jason")
   ...: patient3 = Patient.objects.create(name="justin")

In [2]: reservation1 = Reservation.objects.create(doctor=doctor1, patient=patient1)
   ...: reservation2 = Reservation.objects.create(doctor=doctor1, patient=patient2)
   ...: reservation3 = Reservation.objects.create(doctor=doctor2, patient=patient3)
   ...: reservation4 = Reservation.objects.create(doctor=doctor2, patient=patient1)
   ...: reservation5 = Reservation.objects.create(doctor=doctor3, patient=patient3)

In [3]: patient1
Out[3]: <Patient: junho>

In [4]: dir(patient1)
Out[4]: 

In [5]: patient1.doctors
Out[5]: <django.db.models.fields.related_descriptors.create_forward_many_to_many_manager.<locals>.ManyRelatedManager at 0x171ac29e4c8>

In [6]: patient1.doctors.all()
Out[6]: <QuerySet [<Doctor: tak>, <Doctor: zzulu>]>

In [7]: doctor1
Out[7]: <Doctor: tak>

In [8]: doctor1.patient_set.all()
Out[8]: <QuerySet [<Patient: junho>, <Patient: jason>]>

In [9]: doctor1.patients.all()
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-9-4196dcffa1fe> in <module>
----> 1 doctor1.patients.all()

AttributeError: 'Doctor' object has no attribute 'patients'


```



### model에 related_name 추가

```python
class Doctor(models.Model):
    name = models.CharField(max_length=100)
    def __str__(self):
        return self.name

class Patient(models.Model):
    name = models.CharField(max_length=100)
    # through는 Patient클래스와 Doctor클래스를 연결해주는 클래스를 적어준다.
    # related_name은 Doctor클래스에서도 patients라는 변수를 통해 접근할 수 있도록 한다.
    doctors = models.ManyToManyField(Doctor, through='Reservation', related_name="patients")
    def __str__(self):
        return self.name

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    def __str__(self):
        return f"{self.doctor.name}의사 : {self.patient.name}환자"
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
$ python manage.py shell_plus

In [1]: doctor1 = Doctor.objects.create(name="tak")
   ...: doctor2 = Doctor.objects.create(name="zzulu")
   ...: doctor3 = Doctor.objects.create(name="neo")
   ...: patient1 = Patient.objects.create(name="junho")
   ...: patient2 = Patient.objects.create(name="jason")
   ...: patient3 = Patient.objects.create(name="justin")

In [2]: reservation1 = Reservation.objects.create(doctor=doctor1, patient=patient1)
   ...: reservation2 = Reservation.objects.create(doctor=doctor1, patient=patient2)
   ...: reservation3 = Reservation.objects.create(doctor=doctor2, patient=patient3)
   ...: reservation4 = Reservation.objects.create(doctor=doctor2, patient=patient1)
   ...: reservation5 = Reservation.objects.create(doctor=doctor3, patient=patient3)

In [3]: doctor1.patients.all()
Out[3]: <QuerySet [<Patient: junho>, <Patient: jason>]>

In [4]: patient1.doctors.all()
Out[4]: <QuerySet [<Doctor: tak>, <Doctor: zzulu>]>

```



## Model 변경시 makemigrations의 오류

### 1번: default값 추가

```bash
$ python manage.py makemigrations
You are trying to add a non-nullable field 'time' to reservation without a default; we can't do that (the database needs something to populate existing rows).
Please select a fix:
 1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
 2) Quit, and let me add a default in models.py
Select an option: 1
Please enter the default value now, as valid Python
The datetime and django.utils.timezone modules are available, so you can do e.g. timezone.now
Type 'exit' to exit this prompt
>>> "4pm"
Migrations for 'manytomany':
  manytomany\migrations\0004_reservation_time.py
    - Add field time to reservation
```



### 2번: migration 종료

```bash
$ python manage.py makemigrations
You are trying to add a non-nullable field 'location' to reservation without a default; we can't do
that (the database needs something to populate existing rows).
Please select a fix:
 1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
 2) Quit, and let me add a default in models.py
Select an option: 2
```

```python
class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    time = models.CharField(max_length=100)
    location = models.CharField(max_length=100, default="3층") # default값 추가
    def __str__(self):
        return f"{self.doctor.name}의사 : {self.patient.name}환자"
```



### model 내용 삭제

> 모델 일부가 삭제되면 삭제된 column의 내용 전부 삭제된다.

```bash
$ python manage.py makemigrations
Migrations for 'manytomany':
  manytomany\migrations\0006_remove_reservation_location.py
    - Remove field location from reservation
```



### shell_plus 실행

```bash
$ python manage.py shell_plus

In [1]: d1 = Doctor.objects.get(id=1)

In [2]: p1 = Patient.objects.get(id=1)

In [3]: d1
Out[3]: <Doctor: john>

In [4]: p1
Out[4]: <Patient: change>

In [5]: r1 = Reservation.objects.create(doctor=d1, patient=p1, time="1시", location="2층")

In [6]: r1
Out[6]: <Reservation: john의사 : change환자>

In [7]: r1.time
Out[7]: '1시'

In [8]: r1.location
Out[8]: '2층'

```



## Reservation 삭제

> db날리고 migrations 다시 해준다.

```python
class Doctor(models.Model):
    name = models.CharField(max_length=100)
    def __str__(self):
        return self.name

class Patient(models.Model):
    name = models.CharField(max_length=100)
    # through는 Patient클래스와 Doctor클래스를 연결해주는 클래스를 적어준다.
    # related_name은 Doctor클래스에서도 patients라는 변수를 통해 접근할 수 있도록 한다.
    doctors = models.ManyToManyField(Doctor, related_name="patients") # through='Reservation'
    def __str__(self):
        return self.name

# class Reservation(models.Model):
#     doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
#     patient = models.ForeignKey(Patient, on_delete=models.CASCADE) # Not Null = True 값도 줄 수 있다. 빈 칸 허용한다.
#     time = models.CharField(max_length=100)
#     location = models.CharField(max_length=100, default="3층") # default 값 추가
#     def __str__(self):
#         return f"{self.doctor.name}의사 : {self.patient.name}환자"
```



```bash
$ python manage.py shell_plus

In [1]: d1 = Doctor.objects.create(name="john")

In [2]: p1 = Patient.objects.create(name="change")

In [3]: d1
Out[3]: <Doctor: john>

In [4]: p1
Out[4]: <Patient: change>

In [5]: dir(d1)
Out[5]: 

In [6]: d1
Out[6]: <Doctor: john>

In [7]: p1
Out[7]: <Patient: change>

In [8]: d1.patients.add(p1)

In [9]: d1.patients.all()
Out[9]: <QuerySet [<Patient: change>]>

In [10]: p1.doctors.all()
Out[10]: <QuerySet [<Doctor: john>]>

```

Open database에서 table을 확인하자



## Post

### Post 모델 변경

```python
from django.db import models
from django.conf import settings

# Create your models here.
class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    users = models.ManyToManyField(settings.AUTH_USER_MODEL)
```

오류가 발생한다.

```bash
$ python manage.py makemigrations
SystemCheckError: System check identified some issues:

ERRORS:
posts.Post.user: (fields.E304) Reverse accessor for 'Post.user' clashes with reverse accessor for 'Post.users'.
        HINT: Add or change a related_name argument to the definition for 'Post.user' or 'Post.users'.
posts.Post.users: (fields.E304) Reverse accessor for 'Post.users' clashes with reverse accessor for
'Post.user'.
        HINT: Add or change a related_name argument to the definition for 'Post.users' or 'Post.user'.
```



### Post 모델에 related_name 추가

```python
from django.db import models
from django.conf import settings

# Create your models here.
class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    # related_name을 사용하는 이유는 user.post_set이 user과 users에서 중복되기 때문이다.
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name="like_posts")
```



### index에 좋아요 버튼 생성

```html
{% extends 'base.html' %}
{% block body %}
  <h1>인덱스</h1>
  {% for post in posts %}
    <p>{{post.user.username}}</p>
    <p>{{post.title}}</p>
    {% if user in post.like_users.all %}
      <a href="{% url 'posts:like' post.id %}">좋아요취소</a>
    {% else %}
      <a href="{% url 'posts:like' post.id %}">좋아요</a>
    {% endif %}
    <hr>
  {% endfor %}
{% endblock %}
```



### 좋아요 함수 생성

```python
def like(request, id):
    post = get_object_or_404(Post, id=id)
    user = request.user
    
    if user in post.like_users.all(): # like_users에 user가 포함되어있을 경우 좋아요 취소
        post.like_users.remove(user)
    else:
        post.like_users.add(user)

    return redirect('posts:index')
```

