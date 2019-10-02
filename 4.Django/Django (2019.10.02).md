# Django (2019.10.02)

## Intro

```bash
$ mkdir django/
$ cd django/
$ mkdir reCRUD/
$ cd reCRUD/
$ django-admin startproject recrud .
$ ls
manage.py*  recrud/
```



reCRUD 폴더 안에서 마우스 우클릭 후, vs code 실행

```bash
~/django/reCRUD
$ python -V
$ python manage.py runserver
$ django-admin startapp todos
$ touch .gitignore
$ git init
$ git add .
$ git commit -m "내용"
$ git log # git commit 내역 확인
```



서버를 실행

```bash
$ python manage.py migrate # admin 나오지 않을 경우
$ python manage.py runserver
```



## recrud (project)

### Settings.py

INSTALLED_APPS에 생성된 app을 등록한다.

```python
INSTALLED_APPS = [
    'todos',
]
```



### urls.py

```python
from django.urls import path, include

urlpatterns = [
    path('todos/', include('todos.urls')),
]
```



## todos (app)

### url.py 생성

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index),
]
```



### views.py

```python
def index(request): # request는 인자의 이름
    return render(request, 'index.html') # render는 python에서 html을 읽을 수 있도록 한다.
```



### template를 todos 안에 생성

#### base.html

```html
<body>
  <h1>투두앱</h1>
  {% block body %}
  {% endblock %}
</body>
```



#### index.html

```html
{% extends 'base.html' %}

{% block body %}
  <h2>여기는 인덱스입니다.</h2>
{% endblock %}
```



### models.py

```python
class Todo(models.Model): # models 안의 Model을 상속받는다.
    title = models.CharField(max_length=50)
    due_date = models.DateField()
```



DB 생성

```bash
$ python manage.py makemigrations # SQL에 저장하기 위해 한번 변환해주는 과정
$ python manage.py migrate # DB의 틀을 생성한다.
```



## Create

### urls.py

```python
urlpatterns = [
    path('', views.index),
    path('new/', views.new),
    path('create/', views.create),
]
```



### views.py

```python
def new(request):
    return render(request, 'new.html')
    
def create(request):
    title = request.GET.get('title')
    due_date = request.GET.get('duedate')

    todo = Todo()
    todo.title = title
    todo.due_date = due_date
    todo.save()

    return redirect('/todos/')
```



### new.html

```html
<!-- /todos/create/에 가서 저장 -->
<form action="/todos/create/">
  <input type="text" name="title">
  <input type="date" name="duedate">
  <input type="submit">
</form>
```



### admin.py

```python
from .models import Todo # 현재 폴더의 models에서 Todo를 가져온다.

admin.site.register(Todo)
```



관리자 권한 생성

```bash
$ python manage.py createsuperuser
```



## Read

### index.html

```html
{% extends 'base.html' %}

{% block body %}
  <h2>여기는 인덱스입니다.</h2>
  {% for todo in todos %}
    <h5>{{todo.due_date}} : {{todo.title}}</h5>
  {% endfor %}
{% endblock %}
```



### urls.py

```python
urlpatterns = [
    path('', views.index),

    path('new/', views.new),
    path('create/', views.create),

    path('<int:id>/detail/', views.detail),
]
```



### views.py

```python
def detail(request, id):
    todo = Todo.objects.get(id=id)
    context = {
        'todo': todo,
    }

    return render(request, 'detail.html', context)
```



### detail.html

```html
{% extends 'base.html' %}

{% block body %}
  <h5>{{todo.title}}</h5>
  <h5>{{todo.due_date}}</h5>
{% endblock %}
```



## Delete

### index.html

```html
{% extends 'base.html' %}

{% block body %}
  <h2>여기는 인덱스입니다.</h2>
  {% for todo in todos %}
    <h5>{{todo.due_date}} : {{todo.title}}</h5>
    <a href="/todos/{{todo.id}}/delete/">삭제</a>
  {% endfor %}
{% endblock %}
```



### urls.py

```python
urlpatterns = [
    path('', views.index),

    path('new/', views.new),
    path('create/', views.create),

    path('<int:id>/delete/', views.delete),
]
```



### views.py

```python
def delete(request, id):
    todo = Todo.objects.get(id=id)
    todo.delete()
    return redirect('/todos/')
```



## Update

### index.html

```html
{% extends 'base.html' %}

{% block body %}
  <h2>여기는 인덱스입니다.</h2>
  {% for todo in todos %}
    <h5>{{todo.due_date}} : {{todo.title}}</h5>
    <a href="/todos/{{todo.id}}/delete/">삭제</a>
	<!-- 수정하기 전 단계 -->
    <a href="/todos/{{todo.id}}/edit/">수정</a>
  {% endfor %}
{% endblock %}
```



### urls.py

```python
urlpatterns = [
    path('', views.index),

    path('new/', views.new),
    path('create/', views.create),

    path('<int:id>/delete/', views.delete),

    path('<int:id>/edit/', views.edit)
]
```



### views.py

```python
def edit(request, id):
    todo = Todo.objects.get(id=id)
    context = {
        'todo': todo,
    }

    return render(request, 'edit.html', context)
```



### edit.html

```html
{% extends 'base.html' %}

{% block body %}
  <form action="/todos/{{todo.id}}/update/">
    <input type="text" name="title" value="{{todo.title}}">
    <input type="date" name="duedate" value="{{todo.due_date|date:'Y-m-d'}}">
    <input type="submit">
  </form>
{% endblock %}
```



### urls.py

```python
urlpatterns = [
    path('', views.index),

    path('new/', views.new),
    path('create/', views.create),

    path('<int:id>/delete/', views.delete),

    path('<int:id>/edit/', views.edit),
    path('<int:id>/update/', views.update),
]
```



### views.py

```python
def update(request, id):
    todo = Todo.objects.get(id=id) # 데이터베이스의 원본 자료

    title = request.GET.get('title')
    due_date = request.GET.get('duedate')

    todo.title = title
    todo.due_date = due_date
    
    todo.save()

    return redirect('/todos/')
```

