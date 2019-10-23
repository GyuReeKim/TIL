# Django (2019.10.23)

## accounts의 User 변경

### model 추가

```python
from django.db import models
from django.contrib.auth.models import AbstractUser
from django.conf import settings

# Create your models here.
class User(AbstractUser): # AbstractUser를 상속받았다.
    followers = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name="followings")
```



### settings에 User 작성

> 맨 밑에 AUTH_USER_MODEL을 추가해준다.

```python
# global 변수 설정
AUTH_USER_MODEL = 'accounts.User' # 'auth.User'
```



### admin 생성

> 기존의 User가 사라졌기 때문에 비슷한 형태로 만들어준다.

```python
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin # 기존의 User과 비슷하게 만들어준다.
from .models import User

# Register your models here.
admin.site.register(User, UserAdmin)
```



### form 생성

```python
from django.contrib.auth.forms import UserCreationForm
from .models import User

class CustomUserCreationForm(UserCreationForm):
    class Meta(UserCreationForm.Meta): # 상속이 필요하다.
        model = User
```



### views.py의 signup 수정

> UserCreationForm을 CustomUserCreationForm으로 변경해준다.

```python
from .forms import CustomUserCreationForm

def signup(request):
    if request.method == "POST":
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('accounts:login')
    else:
        form = CustomUserCreationForm()
    context = {
        'form': form
    }
    return render(request, 'accounts/form.html', context)
```

AuthenticationForm은 ModelForm이 아닌 Form을 상속하므로 Custom이 필요없다.



### user_page 생성

> user_info 대신 user를 사용하면 django의 user와 충돌이 일어나 user가 제대로 표현되지 않는다.

```python
def user_page(request, id):
    # 기본적으로 django에서 제공하는 user와의 충돌을 피하기 위해서 user_info로 변경하였다.
    user_info = get_object_or_404(User, id=id)
    context = {
        'user_info': user_info,
    }
    return render(request, 'accounts/user_page.html', context)
```

````html
{% extends 'base.html' %}
{% block body %}
<!-- 기본적으로 django에서 제공하는 user와의 충돌을 피하기 위해서 user_info로 변경하였다. -->
  <h1>{{user_info.username}}님의 유저페이지입니다.</h1>

  {% if user.is_authenticated and user != user_info %}
    <a href="{% url 'accounts:follow' user_info.id %}">팔로우</a>
  {% endif %}
{% endblock %}
````



### follow기능 생성

> followers와 followings를 잘 구분해줘야 한다.

```python
def follow(request, id):
    you = get_object_or_404(User, id=id)
    me = request.user

    if you != me:
        if me in you.followers.all():
            me.followings.remove(you)
            # you.followers.remove(me)
        else:
            me.followings.add(you)
            # you.followers.add(me)

        # if you in me.followings.all():
        #     me.followings.remove(you)
        #     you.followers.remove(me)
        # else:
        #     me.followings.add(you)
        #     you.followers.add(me)

    return redirect('accounts:user_page', id)
```

N:M 관계이기 때문에 순서가 중요하지 않다.

```html
{% extends 'base.html' %}
{% block body %}
<!-- 기본적으로 django에서 제공하는 user와의 충돌을 피하기 위해서 user_info로 변경하였다. -->
  <h1>{{user_info.username}}님의 유저페이지입니다.</h1>
  <h5>{{user_info.username}}를 팔로우하는 사람 {{user_info.followers.all}}</h5>
  <h5>팔로워 : {{user_info.followers.all | length}}</h5>
  <h5>{{user_info.username}}가 팔로우하는 사람 {{user_info.followings.all}}</h5>
  <h5>팔로잉 : {{user_info.followings.all | length}}</h5>

  {% if user.is_authenticated and user != user_info %}
    <a href="{% url 'accounts:follow' user_info.id %}">팔로우</a>
  {% endif %}
{% endblock %}
```



## Posts

### model 생성

```python
from django.db import models
from django.conf import settings
from imagekit.models import ProcessedImageField
from imagekit.processors import ResizeToFill

# Create your models here.
class HashTag(models.Model):
    content = models.CharField(max_length=100)

class Post(models.Model):
    content = models.TextField()
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    image = ProcessedImageField(
                processors= [ResizeToFill(300,300)],
                format= 'JPEG',
                options= {'quality': 90},
                upload_to= 'media'
            )
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name="like_posts")
    created_at = models.DateTimeField(auto_now_add=True)
    hashtags = models.ManyToManyField(HashTag, related_name="taged_post")
```

`ProcessedImageField`를 사용하기 위해 `settings.py`의 `INSTALLED_APPS`에 `imagekit`를 추가해준다.



### create 로직 생성

```python
from django.shortcuts import render, redirect
from .forms import PostForm

def create(request):
    if request.method == "POST":
        # content, image, hashtag 모두 포함되어야한다.
        form = PostForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('posts:index')
    else:
        form = PostForm()
    context = {
        'form': form,
    }
    return render(request, 'posts/form.html', context)
```

이미지를 업로드 하려고 하면  `This field is required. `가 뜬다. `is_valid`에서 오류가 발생한다.

`request.FILES`를 추가해주고, form에서 부족한 내용을 채워주기 위해 임시저장도 해준다.

```python
from django.shortcuts import render, redirect
from .forms import PostForm

# Create your views here.
def index(request):
    return render(request, 'posts/index.html')

def create(request):
    if request.method == "POST":
        # content, image, hashtag 모두 포함되어야한다.
        form = PostForm(request.POST, request.FILES)
        if form.is_valid():
            post = form.save(commit=False) # save를 잠시 멈춰둔다.
            post.user = request.user
            post.save()
            return redirect('posts:index')
    else:
        form = PostForm()
    context = {
        'form': form,
    }
    return render(request, 'posts/form.html', context)
```



### HashTag 구분해주기

> admin에서 확인하면 사용하는 hashtag는 회색으로 나타난다.

```python
from .models import HashTag, Post

def create(request):
    if request.method == "POST":
        # content, image, hashtag 모두 포함되어야한다.
        form = PostForm(request.POST, request.FILES)
        if form.is_valid():
            post = form.save(commit=False) # save를 잠시 멈춰둔다.
            post.user = request.user
            # post object (None)
            post.save()
            # post object (1) # id를 부여
            for word in post.content.split():
                if word.startswith('#'):
                    # hashtag 추가
                    # hashtag, created = Hashtag.objects.get_or_create(content=word) # (obj, True or False)
                    hashtag = HashTag.objects.get_or_create(content=word)[0]
                    post.hashtags.add(hashtag)
            return redirect('posts:index')
    else:
        form = PostForm()
    context = {
        'form': form,
    }
    return render(request, 'posts/form.html', context)
```



### index에 나타내주기

```html
{% extends 'base.html' %}
{% block body %}
  {% for post in posts %}
    {% include 'posts/_post.html' %}
  {% endfor %}
{% endblock %}
```

```html
<div class="card col-6">
  <img src="{{post.image.url}}" class="card-img-top" alt="...">
  <div class="card-body">
    <h5 class="card-title">Card title</h5>
    <p class="card-text">{{post.content}}</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
</div>
```

이미지에 접근하려면 project 폴더에 MEDIA와 url을 추가해줘야 한다.

```python
from django.conf.urls.static import static
from django.conf import settings

# image url에 접근하기 위해 필요하다.
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

```python
# urls.py와 같은 역할
MEDIA_URL = '/media/'

# 내부적으로 파일을 찾는 설정
MEDIA_ROOT = os.path.join(BASE_DIR)
```



## HashTag를 a태그로 변경

### hashtags 함수 생성

```python
def hashtags(request, id):
    hashtag = get_object_or_404(HashTag, id=id)

    posts = hashtag.taged_post.all()
    context = {
        'posts': posts,
    }
    return render(request, 'posts/index.html', context)
```



### templatetags 폴더 생성

> `__init__.py`와 `make_link.py`를 templatetags 폴더 안에 추가해준다.

```python
from django import template

register = template.Library()

@register.filter
def hashtag_link(post):
    content = post.content # #고양이 #야옹 #강아지 #멍멍 => <a>#고양이</a>
    hashtags = post.hashtags.all() # QuerySet [HashTag object (1), HashTag object (2)]

    for hashtag in hashtags:
        content = content.replace(
            f'{hashtag.content}',
            f'<a href="/posts/hashtags/{hashtag.id}/">{hashtag.content}</a>'
        )
    return content
```

`__init__.py`는 함수 실행을 위해 필요하다.

```html
{% load make_link %}
<div class="card col-6">
  <img src="{{post.image.url}}" class="card-img-top" alt="...">
  <div class="card-body">
    <h5 class="card-title"></h5>
    <!-- hashtag_link(post) -->
    <p class="card-text">{{post|hashtag_link|safe}}</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
</div>
```

함수는 `load`가 필요하며 img url이 필요하다.