# Django (2019.10.24)

## 회원 탈퇴

### 회원탈퇴 url 작성

> 회원 탈퇴를 위해 팔로우의 else에 작성한다.

```html
{% else %}
  <a href="{% url 'accounts:delete' user_info.id %}">회원탈퇴</a>
  <form action="" method="post">
    {% csrf_token %}
    <input type="submit" value="탈퇴">
  </form>
{% endif %}
```

```python
urlpatterns = [
    ...
    path('<int:id>/delete/', views.delete, name="delete"),
    ...
]
```

```python
def delete(request, id):
    user_info = get_object_or_404(User, id=id)
    user = request.user

    if user == user_info:
        user.delete()
    return redirect('posts:index')
```

user(로그인 되어있는 user)와 user_info(현재 페이지의 user)가 같다면 회원탈퇴를 해준다.



## 정보수정

### 정보 수정 url 작성

> 정보 수정을 위해 팔로우의 else에 작성한다.

```html
{% else %}
  <a href="{% url 'accounts:update' %}">정보수정</a>
{% endif %}
```

```python
urlpatterns = [
    ...
    path('update/', views.update, name="update"),
    ...
]
```

```python
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm, UserChangeForm
from .forms import CustomUserCreationForm, CustomUserChangeForm

def update(request):
    if request.method == "POST":
        form = CustomUserChangeForm(request.POST, instance=request.user)
        if form.is_valid():
            form.save()
            return redirect('posts:index')
    else:
        form = CustomUserChangeForm(instance=request.user)
    context = {
        'form': form,
    }
    return render(request, 'accounts/form.html', context)
```



### 비밀번호 변경 url 작성

> 비밀번호 변경을 위해 팔로우의 else에 작성한다.

```html
{% else %}
  <a href="{% url 'accounts:password' %}">비밀번호변경</a>
{% endif %}
```

```python
urlpatterns = [
    ...
    path('password/', views.password, name="password"),
    ...
]
```

```python
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm, UserChangeForm, PasswordChangeForm
from django.contrib.auth import update_session_auth_hash

def password(request):
    if request.method == "POST":
        form = PasswordChangeForm(request.user, request.POST)
        if form.is_valid():
            form.save() # save만 하면 비밀번호 변경 후 로그인이 풀린다.
            update_session_auth_hash(request, form.user)
            return redirect('posts:index')
    else:
        form = PasswordChangeForm(request.user)
    context = {
        'form': form,
    }
    return render(request, 'accounts/form.html', context)
```

비밀번호 변경 후 바로 로그인 해주기 위해 함수를 추가해야 한다.



## 소셜로그인 기능 추가

### django allauth

> ` https://django-allauth.readthedocs.io/en/latest/installation.html `settings.py와 비교 후 변경해준다.

```bash
pip install django-allauth
```

```python
INSTALLED_APPS = [
    'allauth', # 필수
    'allauth.account', # 필수
    'allauth.socialaccount', # 필수
    'accounts',
    'posts',
    'imagekit',
    'bootstrap4',
    'django.contrib.admin',
    'django.contrib.auth', # 필수
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages', # 필수
    'django.contrib.staticfiles',
    'django.contrib.sites', # 추가
    'allauth.socialaccount.providers.kakao', # 카카오 추가
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'insta', 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                ...
                'django.template.context_processors.request', # allauth를 사용하기 위해 꼭 필요하다.
                ...
            ],
        },
    },
]

AUTHENTICATION_BACKENDS = (
    # Needed to login by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
)

SITE_ID = 1
```

````python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('posts/', include('posts.urls')),
    path('accounts/', include('accounts.urls')),
    path('accounts/', include('allauth.urls')),
]
````

urlpatterns는 순서가 중요하다. 직접 만든 accounts가 우선이 되게 하기 위해서는 위쪽에 두어야 한다.



### 카카오 개발자 센터

> ` https://developers.kakao.com/ ` ` https://developers.kakao.com/buttons `

- 로그인

- 앱 등록

- 설정 - 일반 - 플랫폼 추가(웹)
  - `http://127.0.0.1` `https://127.0.0.1`

- 사용자관리 - on
  - 로그인 동의항목에 email 추가
  - 로그인 Redirect URI `http://127.0.0.1:8000/accounts/kakao/login/callback` 추가

- 일반 - REST API 키 복사

- admin페이지에 있는 `Social applications`의 `Client id`에 REST API 키 복사

- 고급설정 - 코드 발급 - 상태 on - 적용

- admin페이지에 있는 `Social applications`의 `Secret key`에  발급받은 키 저장



### accounts의 form.html에 socialaccount 연결

> 현재 상태에서는 form을 사용할 때마다 카카오로그인이 보인다.

```html
{% extends 'base.html' %}
{% load socialaccount %}
{% load bootstrap4 %}
{% block body %}
  <form action="" method="post">
    {% csrf_token %}
    {% bootstrap_form form %}
    <input type="submit">
  </form>

  <a href="{% provider_login_url 'kakao' %}">카카오로그인</a>
{% endblock %}
```

page not found(404)에 ` http://127.0.0.1:8000/accounts/profile/ `이 뜨면 성공한 것이다.



### 로그인 하면 index로 가도록 설정

> settings.py에 작성한다.

```python
LOGIN_REDIRECT_URL = 'posts:index'
```



### 로그인 하면 마이페이지로 가도록 설정 (profile 경로 생성)

> settings.py를 주석처리한다.

```python
path('profile/', views.profile, name="profile"),
```

profile 함수는 user_page 함수와 비슷하게 작성한다.

```python
def profile(request):
    user_info = request.user # 로그인 되어있는 user 정보를 가져온다.
    context = {
        'user_info': user_info,
    }
    return render(request, 'accounts/user_page.html', context)
```



## 페이지네이션

> settings.py의 INSTALLED_APPS에 `'bootstrap4',`를 추가해준다.

### django의 pagination

> ` https://docs.djangoproject.com/en/2.2/topics/pagination/ `

```python
from django.core.paginator import Paginator

def all(request):
    posts = Post.objects.all()
    paginator = Paginator(posts, 5)
    page = request.GET.get('page')
    contacts = paginator.get_page(page)
    context = {
        'posts': posts,
        'contacts': contacts,
    }
    return render(request, 'posts/pagination.html', context)
```

```python
<div class="pagination">
    <span class="step-links">
        {% if contacts.has_previous %}
            <a href="?page=1">&laquo; first</a>
            <a href="?page={{ contacts.previous_page_number }}">previous</a>
        {% endif %}

        <span class="current">
            Page {{ contacts.number }} of {{ contacts.paginator.num_pages }}.
        </span>

        {% if contacts.has_next %}
            <a href="?page={{ contacts.next_page_number }}">next</a>
            <a href="?page={{ contacts.paginator.num_pages }}">last &raquo;</a>
        {% endif %}
    </span>
</div>

```



### load 이용

>` https://pypi.org/project/django-bootstrap-pagination/ `

```python
from django.core.paginator import Paginator

def all(request):
    posts = Post.objects.all()
    paginator = Paginator(posts, 5)
    page = request.GET.get('page')
    posts = paginator.get_page(page)
    context = {
        'posts': posts,
    }
    return render(request, 'posts/pagination.html', context)
```

```html
{% extends 'base.html' %}
{% load bootstrap_pagination %}
{% block body %}
  <div class="container m-3">
    <div class="row d-flex justify-content-center">
      {% for post in posts %}
        {% include 'posts/_post.html' %}
      {% endfor %}
    </div>
    <div class="d-flex justify-content-center">
      {% bootstrap_paginate posts %}
    </div>
  </div>
{% endblock %}
```



## Heroku

> 기본적으로 무료, 한달에 750시간 돌릴 수 있다. 너무 많은 사용자가 들어오면 문제가 발생할 수 있다.

- 회원가입 후 앱을 생성한다.

- git commit 해준다.

- ```bash
  $ pip install django-heroku
  ```

- settings.py에 아래와 같이 추가해준다.

  ````python
  import django_heroku
  django_heroku.settings(locals())
  ````

- Procfile 파일을 root에 생성해준다.

  ```
  web: gunicorn insta.wsgi --log-file -
  ```

- ```bash
  $ pip install gunicorn
  ```

- runtime.txt

  ```
  python-3.7.4
  ```

- ```bash
  $ pip freeze > requirements.txt
  ```

  - 어떤 버전의 프로그램을 쓰는지 정리해준다.

- git commit과 push 해준다.

- heroku페이지에서 GitHub연동해준 후  github 폴더를 찾아 connect 해준다.

- Authmatic deploys의 `Enable Automatic Deploys`  버튼을 클릭한다.

- Manual deploy의 `Deploy Branch`버튼을 클릭한다.

- 오른쪽 위 more - Run console

  - python manage.py migrate

- python manage.py createsuperuser

- Open app

- settings.py

  ```python
  DEBUG = False
  ```

- 카카오 개발자 - 내 애플리케이션

  - 일반 - 앱 - `https://soulpeace-insta.herokuapp.com`

  - 사용자관리 - 로그인 Redirect URI

    `https://soulpeace-insta.herokuapp.com/accounts/kakao/login/callback/`

- admin페이지에 있는 `Social applications`의 `Secret key`에  발급받은 키 저장

