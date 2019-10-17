# Django (2019.10.17)

## accounts를 우선 생성

> base 파일은 공통되므로 project 폴더 안에 생성할 수 있다.

settings에서 template의` 'APP_DIRS': True`

`BASE_DIR`은 현재 프로젝트 폴더를 가리킨다.

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        # 운영체제별로 경로설정 방법이 다르기때문에 함수를 사용한다.
        'DIRS': [os.path.join(BASE_DIR, 'wunderlist', 'templates')], # 폴더 경로를 지정해준다.
        'APP_DIRS': True, # 프로젝트 폴더는 검색하지 않는다.
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```



### base

> settings.py의 INSTALLED_APPS 에 `'bootstrap4',`를 추가해준다.

```python
{% load bootstrap4 %}
<head>
  {% bootstrap_css %}
</head>
<body>
  <ul class="nav justify-content-center">
    <li class="nav-item">
      <a class="nav-link active" href="{% url 'accounts:signup' %}">회원가입</a>
    </li>
  </ul>
  <div class="container">
    {% block body %}
    {% endblock %}
  </div>
  {% bootstrap_javascript jquery='full' %}
</body>
</html>
```





## 회원가입

```python
from django.shortcuts import render, redirect
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth import login as auth_login

# Create your views here.
def signup(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            # form을 user에 저장한다.
            user = form.save()
            # 회원가입 후 바로 로그인을 할 수 있도록 설정
            auth_login(request, user)
            return redirect('todos:index')
    else:
        form = UserCreationForm()
    context = {
        'form':form
    }
    return render(request, 'accounts/signup.html', context)
```

```bash
$ python manage.py migrate
```



## todos 앱 생성

> 쿠키는 사용자가 관리하고 세션은 서버 관리자가 관리한다.



### embed 확인

```shell
In [1]: request
Out[1]: <WSGIRequest: GET '/todos/'>

In [2]: request.user
Out[2]: <SimpleLazyObject: <User: test2>>

In [3]: request.session
Out[3]: <django.contrib.sessions.backends.db.SessionStore at 0x1aa4eb24188>

In [4]: dir(request.session)
Out[4]:
['TEST_COOKIE_NAME',
 'TEST_COOKIE_VALUE',
 '_SessionBase__not_given',
 '_SessionBase__session_key',
 '__class__',
 '__contains__',
 '__delattr__',
 '__delitem__',
 '__dict__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getitem__',
 '__gt__',
 '__hash__',
 '__init__',
 '__init_subclass__',
 '__le__',
 '__lt__',
 '__module__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__setitem__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 '__weakref__',
 '_get_new_session_key',
 '_get_or_create_session_key',
 '_get_session',
 '_get_session_from_db',
 '_get_session_key',
 '_hash',
 '_session',
 '_session_cache',
 '_session_key',
 '_set_session_key',
 '_validate_session_key',
 'accessed',
 'clear',
 'clear_expired',
 'create',
 'create_model_instance',
 'cycle_key',
 'decode',
 'delete',
 'delete_test_cookie',
 'encode',
 'exists',
 'flush',
 'get',
 'get_expire_at_browser_close',
 'get_expiry_age',
 'get_expiry_date',
 'get_model_class',
 'has_key',
 'is_empty',
 'items',
 'keys',
 'load',
 'model',
 'modified',
 'pop',
 'save',
 'serializer',
 'session_key',
 'set_expiry',
 'set_test_cookie',
 'setdefault',
 'test_cookie_worked',
 'update',
 'values']

In [5]: request.session._session
Out[5]:
{'_auth_user_id': '3',
 '_auth_user_backend': 'django.contrib.auth.backends.ModelBackend',
 '_auth_user_hash': 'fcc39b4a26a38c530605ee508776c7678219e8ef'}

In [6]: exit
```



### 몇번 방문했는지 count

> 사용하지는 않음

```
def index(request):
    # embed()
    visit_num = request.session.get('visit_num', 0)
    # 방문 할 때마다 visit이 1씩 늘어남
    request.session['visit_num'] = visit_num + 1
    
    request.session.modified = True

    context = {
        'visit_num': visit_num,
    }

    return render(request, 'todos/index.html', context)
```



## login

> Authentication : 인증, Authorization: 권한분리



### 이미 로그인 했을 경우 login창 보여주지 않기

> 하지만 url 경로를 알고있을 경우에는 소용이 없다.

```html
<body>
  <ul class="nav justify-content-center">
    {% if user.is_authenticated %}
    <li class="nav-item">
      <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">{{user.username}}</a>
    </li>
    {% else %}
    <li class="nav-item">
      <a class="nav-link active" href="{% url 'accounts:signup' %}">회원가입</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="{% url 'accounts:login' %}">로그인</a>
    </li>
    {% endif %}
    <li class="nav-item">
      <a class="nav-link" href="#">로그아웃</a>
    </li>
  </ul>
</body>
```



### accounts의 views.py

> url로 접근할 경우 사용자가 로그인한 상태일 경우 index로 보내주도록 설정한다.

```python
def signup(request):
    if request.user.is_authenticated: # 추가해준다.
        return redirect('todos:index')
        
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            # form을 user에 저장한다.
            user = form.save()
            # 회원가입 후 바로 로그인을 할 수 있도록 설정
            auth_login(request, user)
            return redirect('todos:index')
    else:
        form = UserCreationForm()
    context = {
        'form':form
    }
    return render(request, 'accounts/form.html', context)

def login(request):
    if request.user.is_authenticated: # 추가해준다.
        return redirect('todos:index')

    if request.method == "POST":
        form = AuthenticationForm(request, request.POST) # request가 필요하다.
        if form.is_valid():
            auth_login(request, form.get_user())
            return redirect('todos:index')
    else:
        form = AuthenticationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/form.html', context)
```



## logout

```python
def logout(request):
    auth_logout(request)
    return redirect('todos:index')
```

```html
<body>
  <ul class="nav justify-content-center">
    {% if user.is_authenticated %}
      <li class="nav-item">
        <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">{{user.username}}</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="{% url 'accounts:logout' %}">로그아웃</a>
      </li>
    {% else %}
      <li class="nav-item">
        <a class="nav-link active" href="{% url 'accounts:signup' %}">회원가입</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="{% url 'accounts:login' %}">로그인</a>
      </li>
    {% endif %}
  </ul>
  <div class="container">
    {% block body %}
    {% endblock %}
  </div>
  {% bootstrap_javascript jquery='full' %}
</body>
</html>
```



## todo 모델 생성

### settings.py의 언어, 시간 변경

```python
LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/seoul'
```



### todos의 models.py

```python
from django.db import models
from django.conf import settings

# Create your models here.
class Todo(models.Model):
    content = models.CharField(max_length=100)
    due_date = models.DateField()
    created_at = models.DateField(auto_now_add=True) # 현재시간 저장
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
```



## Form 생성

> `created_at`은 자동으로 생성된다.

```python
from django import forms
from .models import Todo

class TodoForm(forms.ModelForm):
    due_date = forms.DateField(
                    widget=forms.DateInput(
                        attrs={
                            'type': 'date'
                        }
                    )
                )
    class Meta:
        model = Todo
        fields = ('content', 'due_date',)
```



## 글쓰기 (todos의 create)

```python
def create(request):
    if request.method == "POST":
        form = TodoForm(request.POST)
        if form.is_valid():
            # 비어있는 column이 있기 때문에, 완전히 저장하지 않고 잠시 기다린다.
            todo = form.save(commit=False)
            todo.user = request.user
            todo.save()
            return redirect('todos:index')
    else:
        form = TodoForm()
    context = {
        'form': form
    }
    return render(request, 'todos/form.html', context)
```

