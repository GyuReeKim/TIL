# Django (2019.10.04)

## 수정된 부분만 commit 남기는 방법

````bash
$ git add todos/migrations/0001_initial.py
$ git add todos/models.py
$ git add db.sqlite3
$ git status # 상태를 보여준다.
$ git commit -m "commit 내용"
$ git log --oneline # git을 올린 현황을 알 수 있다.
````

초록색 부분을 commit 해준다.

빨간색 부분은 commit 하지 않는다.



gitignore에 django를 넣으면 db를 git에 올리지 않는다. 협업을 하기 위해서는 db를 올리지 않는 편이 좋다.

git add도 할 수 없다.



## board

### settings.py 디버깅 (오류 찾기)

```python
INSTALLED_APPS = [
    'todos' # 쉼표를 붙이지 않으면 오류가 발생한다.
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

```
ModuleNotFoundError: No module named 'todosdjango'
```



## todos

### urls.py

```python
from . import views # '.' 대신 'todos'를 써도 된다. '.' : 같은 폴더

urlpatterns = [
    path('', views.index),

    path('new/', views.new),
    path('create/', views.create),
]
```



### views.py

```python
def index(request):
    return render(request, 'index.html')

def new(request):
    return render(request, 'new.html')

def create(request):
    pass
```



### models.py

```python
class Todo(models.Model):
    title = models.CharField(max_lenght=50) # 짧은 형태의 글
    content = models.TextField() # 긴 형태의 글 # django에서 TextField는 form 클래스에서 크기조절이 가능한 큰 상자가 생긴다.
    due_date = models.DateField()
    author = models.CharField(max_length=50)
```

대부분의 프로젝트는 이후에 수정하기 어렵기 때문에 모델링을 먼저 한다. 따라서 모델링은 신중하게 해야한다.

불가피하게 바꿔야 한다면 DB를 모두 날리고 다시 만드는 편이 좋다.



model을 만들면 migrate 해줘야 한다.

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```



## Create (Post)

### new.html

```html
{% extends 'base.html' %}

{% block body %}
  <!-- form method의 기본값은 "get" -->
  <form action="/todos/create/" method="post">
    작성자: <input type="text">
    제목 : <input type="text">
    내용 : <input type="text">
    마감일 : <input type="date">
    <input type="submit" value="저장">
  </form>
{% endblock %}
```

실행해보면 forbidden이 뜬다.

get 어떤 값을 넣고 요청을 보내면, 사용자가 보낸 데이터를 처리한다.

데이터를 보내는 것을 처리하려면 post를 사용하는 것이 좋다.

```html
{% extends 'base.html' %}

{% block body %}
  <!-- form method의 기본값은 "get" -->
  <form action="/todos/create/" method="post">
    <!-- 'csrf_token'은 django 안에서 들어오는 input값을 hidden으로 처리해준다. -->
    {% csrf_token %}
    작성자: <input type="text">
    제목 : <input type="text">
    내용 : <input type="text">
    마감일 : <input type="date">
    <input type="submit" value="저장">
  </form>
{% endblock %}
```

csrf_token을 쓰고 실행하면 아래와 같이 뜬다.

```
ValueError at /todos/create/

The view todos.views.create didn't return an HttpResponse object. It returned None instead.
```

Request information의 Post에 Value값이 없는 것을 확인할 수 있다. 이는 input에 name을 작성해주면 해결된다.



### base.html

> name 추가

```html
{% extends 'base.html' %}

{% block body %}
  <!-- form method의 기본값은 "get" -->
  <form action="/todos/create/" method="post">
    <!-- 'csrf_token'은 django 안에서 들어오는 input값을 hidden으로 처리해준다. -->
    {% csrf_token %}
    작성자: <input type="text" name="author">
    제목 : <input type="text" name="title">
    내용 : <input type="text" name="content">
    마감일 : <input type="date" name="due-date">
    <input type="submit" value="저장">
  </form>
{% endblock %}
```



### views.py

```python
from django.shortcuts import render, redirect
from .models import Todo # '.models' 대신 'todos.models'를 사용해도 된다.

# Create your views here.
def index(request):
    todos = Todo.objects.all()
    context = {
        'todos': todos,
    }
    return render(request, 'index.html')

def create(request):
    author = request.Post.get('author')
    title = request.Post.get('title')
    content = request.Post.get('content')
    due_date = request.Post.get('due-date')

    # todo = Todo()
    # todo.author = author
    # todo.title = title
    # todo.content = content
    # todo.due_date = due_date
    # todo.save() # 바로 저장하지 않고 쓰일때 사용
    
    # create는 안에 인스턴스를 만들고 생성까지 해준다.
    todo = Todo.objects.create(
        author=author,
        title=title,
        content=content,
        due_date=due_date,
    )
    return redirect('/todos/')
```



## Read

### index.html

```html
{% extends 'base.html' %}

{% block body %}
  <h1>index</h1>
  {% for todo in todos %}
    <h5>{{todo.author}}/{{todo.title}} - {{todo.due_date}}</h5>
  {% endfor %}
{% endblock %}
```



## Delete

### index.html

```html
{% extends 'base.html' %}

{% block body %}
  <h1>index</h1>
  {% for todo in todos %}
    <h5>{{todo.author}}/{{todo.title}} - {{todo.due_date}}</h5>
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

    path('<int:id>/delete/', views.delete)
]
```



### views.py

```python
def delete(request, id):
    todo = Todo.objects.get(id=id)
    todo.delete()
    return redirect('/todos/')
```



## 변수로 경로 사용하기

### base.html

```html
<body>
  <a href="http://127.0.0.1:8000/todos/new/">할일추가</a>
  {% block body %}
  {% endblock %}
</body>
```

'/todos/new/'처럼 쓸 수 있다. '/' 앞은 현재 폴더를 말한다.

하지만 경로 설정하는 것은 어렵다.



### urls.py

```python
# app을 여러개 생성해 줄 수 있기 때문에 app_name으로 묶어준 후 변수를 설정해주는 편이 좋다.
app_name = 'todos'

urlpatterns = [
    path('', views.index),

    path('new/', views.new, name="new"), # new/라는 경로를 new라는 변수에 저장해서 사용한다.
    path('create/', views.create),

    path('<int:id>/delete/', views.delete)
]
```



### base.html

```html
<body>
  <a href="{% url 'todos:new' %}">할일추가</a>
  {% block body %}
  {% endblock %}
</body>
```



위와 같은 방법으로 해보면

### urls.py

> name 작성

```python
app_name = 'todos'

urlpatterns = [
    path('', views.index, name="index"),

    path('new/', views.new, name="new"), # new/라는 경로를 new라는 변수에 저장해서 사용한다.
    path('create/', views.create),

    path('<int:id>/delete/', views.delete)
]
```



### base.html

> url 변경

```html
<body>
  <a href="{% url 'todos:index' %}">전체글보기</a>
  <a href="{% url 'todos:new' %}">할일추가</a>
  {% block body %}
  {% endblock %}
</body>
```



### views.py

> url 변경

```python
def delete(request, id):
    todo = Todo.objects.get(id=id)
    todo.delete()
    return redirect('todos:index') # '/todos/' 대신 사용한다.
```



delete를 같은 방식으로 바꿔주게 되면 아래와 같은 오류가 발생한다.

### index.html

```html
{% extends 'base.html' %}

{% block body %}
  <h1>index</h1>
  {% for todo in todos %}
    <h5>{{todo.author}}/{{todo.title}} - {{todo.due_date}}</h5>
    <a href="{% url 'todos:delete' %}">삭제</a>
  {% endfor %}
{% endblock %}
```



```
NoReverseMatch at /todos/
Reverse for 'delete' with no arguments not found. 1 pattern(s) tried: ['todos/(?P<id>[0-9]+)/delete/$']
```

render에서 오류가 발생한다.



### index.html

> {% url 'todos:delete' todo.id %}와 같이 써줘야 한다.

```html
{% extends 'base.html' %}

{% block body %}
  <h1>index</h1>
  {% for todo in todos %}
    <h5>{{todo.author}}/{{todo.title}} - {{todo.due_date}}</h5>
    <a href="{% url 'todos:delete' todo.id %}">삭제</a>
  {% endfor %}
{% endblock %}
```



## New + Create

> url을 하나로 합친다.

### urls.py

> add 경로를 만들어준다.

```python
urlpatterns = [
    path('', views.index, name="index"),
    
    # new와 create를 합친다.
    path('add/', views.add, name="add"),

    path('<int:id>/delete/', views.delete, name="delete")
]
```



### views.py

```python
def add(request):
    if request.method == "POST":
        author = request.POST.get('author')
        title = request.POST.get('title')
        content = request.POST.get('content')
        due_date = request.POST.get('due-date')
        
        todo = Todo.objects.create(
            author=author,
            title=title,
            content=content,
            due_date=due_date,
        )
        return redirect('todos:index')
    else:
        return render(request, 'add.html')
```



### add.html

```html
{% extends 'base.html' %}

{% block body %}
  <!-- action이 빈칸일 경우 제자리에 있게 된다. -->
  <form action="" method="post">
    <!-- 'csrf_token'은 django 안에서 들어오는 input값을 hidden으로 처리해준다. -->
    {% csrf_token %}
    작성자: <input type="text" name="author">
    제목 : <input type="text" name="title">
    내용 : <input type="text" name="content">
    마감일 : <input type="date" name="due-date">
    <input type="submit" value="저장">
  </form>
{% endblock %}
```



## Restful

resource 생성, 삭제는 POST방식, 수정은 PUT, 목록과 내용을 표시하는 것은 GET방식이다.

하나의 url로 여러 기능을 할 수 있다.



## Update

### urls.py

```python
urlpatterns = [
    # Update
    path('<int:id>/update/', views.update, name="update"),
]
```



### index.html

```html
{% extends 'base.html' %}

{% block body %}
  <h1>index</h1>
  {% for todo in todos %}
    <h5>{{todo.author}}/{{todo.title}} - {{todo.due_date}}</h5>
    <a href="{% url 'todos:delete' todo.id %}">삭제</a>
    <a href="{% url 'todos:update' todo.id %}">수정</a>
  {% endfor %}
{% endblock %}
```



### views.py

```python
def update(request, id):
    todo = Todo.objects.get(id=id)
    if request.method == "POST":
        author = request.POST.get('author')
        title = request.POST.get('title')
        content = request.POST.get('content')
        due_date = request.POST.get('due-date')

        # todo = Todo(author=author, title=title, content=content, due_date=due_date)
        todo.author = author
        todo.title = title
        todo.content = content
        todo.due_date = due_date
        todo.save()
        return redirect('todos:index')
    else:
        context = {
            'todo': todo,
        }
        return render(request, 'update.html', context)
```



### update.html

```html
{% extends 'base.html' %}

{% block body %}
  <form action="" method="post">
    <!-- 'csrf_token'은 django 안에서 들어오는 input값을 hidden으로 처리해준다. -->
    {% csrf_token %}
    작성자: <input type="text" name="author" value="{{todo.author}}">
    제목 : <input type="text" name="title" value="{{todo.title}}">
    내용 : <input type="text" name="content" value="{{todo.content}}">
    마감일 : <input type="date" name="due-date">
    <input type="submit" value="저장">
  </form>
{% endblock %}
```

