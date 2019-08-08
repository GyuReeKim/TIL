# Web Service 2주차 (2019.08.08)

## 05_qna

```bash
$ python -m venv venv
$ pip install django
$ django=admin startproject qna .
$ django-admin startapp articles
```



### settings.py (qna)

```python
INSTALLED_APPS = [
    'articles',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```



### commit 남기기

```bash
$ git init
$ git add .
$ git commit -m "startproject & startapp"
```



## models선언 & migration

### models.py (articles)

```python
from django.db import models

# Create your models here.
class Question(models.Model):
    title = models.CharField(max_length=50)
    content = models.CharField(max_length=100)
    user = models.CharField(max_length=20)
    created_at = models.DateTimeField(auto_now_add=True)
```



### Migration

```bash
$ python manage.py makemigrations
$ python manage.py migrate # 처음에는 많은 양의 migrate가 된다.
```



## admin 설정

### admin.py

```python
from django.contrib import admin
from .models import Question
# Register your models here.
admin.site.register(Question)
```



### super user 생성

```bash
$ python manage.py createsuperuser
```



## new & create 완성

### urls.py (qna)

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('questions/', include('articles.urls')),
]
```



### urls.py (articles)

```python
from django.urls import path
from . import views

urlpatterns = [
    # Create
    path('new/', views.new),
    path('create/', views.create),
]
```



### views.py

```python
from django.shortcuts import render

# Create your views here.
def new(request):
    return render(request, 'new.html')

def create(request):
    user = request.GET.get('user')
    title = request.GET.get('title')
    content = request.GET.get('content')

    question = Question()
    question.user = user
    question.title = title
    question.content = content
    question.save()

    return redirect('/questions/')
```



### base.html

```html
<body>
  {% block body %}
  {% endblock %}
</body>
```



### new.html

```html
{% extends 'base.html' %}
{% block body %}
  <form action="/questions/create/">
    <input type="text" name="user">
    <input type="text" name="title">
    <input type="text" name="content">
    <input type="submit">
  </form>
{% endblock %}
```



## index 완성

### urls.py (articles)

```python
from django.urls import path
from . import views

urlpatterns = [
    # Create
    path('new/', views.new),
    path('create/', views.create),
    # Read
    path('', views.index),
]
```



### views.py

```python
def index(request):
    questions = Question.objects.all()
    context = {
        'questions': questions,
    }
    return render(request, 'index.html', context)
```



### index.html

> test 해보기

```html
{% extends 'base.html' %}
{% block body %}
  {{questions}}
{% endblock %}
```



### base.html

> 바로가기 편하도록 `<a>`설정

```html
<body>
  <a href="/questions/">전체보기</a>
  <a href="/questions/new/">질문하기</a>
  {% block body %}
  {% endblock %}
</body>
```



## index 수정

### index.html

```html
{% extends 'base.html' %}
{% block body %}
  {% for question in questions %}
    <h1>{{question.title}}</h1>
    <p>{{question.user}}</p>
    <p>{{question.content}}</p>
    <hr>
  {% endfor %}
{% endblock %}
```



## 제목

### models.py

```python
class Answer(models.Model):
    content = models.CharField(max_length=100)
    # CASCADE: 폭포같은 느낌, question이 사라지면 그에 따른 값들이 다 지워진다.
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
```



### Migration

```bash
$ python manage.py makemigrations
$ python manage.py migrate # 처음에는 많은 양의 migrate가 된다.
```



### index.html

```html
{% extends 'base.html' %}
{% block body %}
  {% for question in questions %}
    <h1>{{question.title}}</h1>
    <p>{{question.user}}</p>
    <p>{{question.content}}</p>
    <form action="/questions/{{question.id}}/answers/create/">
      <input type="text" name="content">
      <input type="submit">
    </form>
      {% for answer in question.answer_set.all %}
        <p>{{answer.content}}</p>
      {% endfor %}
    <hr>
  {% endfor %}
{% endblock %}
```



### urls.py (articles)

```python
from django.urls import path
from . import views

urlpatterns = [
    # Create
    path('new/', views.new),
    path('create/', views.create),
    # Read
    path('', views.index),

    path('<int:question_id>/answers/create/', views.answer_create),
]
```



### views.py

```python
def index(request):
    questions = Question.objects.all()
    answers = Answer.objects.all()
    context = {
        'questions': questions,
        'answers': answers,
    }
    return render(request, 'index.html', context)

def answer_create(request, question_id):
    content = request.GET.get('content')

    # Question 데이터의 본질을 저장
    question = Question.objects.get(id=question_id)

    answer = Answer()
    answer.content = content
    answer.question = question
    answer.save()

    return redirect('/questions/')
```



### admin.py

```python
from django.contrib import admin
from .models import Question, Answer
# Register your models here.
admin.site.register(Question)
admin.site.register(Answer)
```

