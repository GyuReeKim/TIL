# Django Subject Test

### 1번

DATABASE

````
설정을 변경하면 MySQL등 사용 가능하다
````



### 2번

Project urls.py

```
path('home/', include('home.urls')),
```



### 3번

```
from django.views.decorator.http import required_POST
```



### 4번

```
ModelForm
```



### 5번

```
{% extends 'base.html' %}
```



### 6번

```
article = article_form.save(commit=False)
```



### 7번

```
get_object_or_404
```



### 8번

```
Asia/Seoul
```



### 9번

likes = models.ManyToManyField()

```
form django.conf import settings

User.article_set.all()
article.likes.all()
```

```
likes_id는 다른 table에 생성된다.
```



### 10번

settings.py TEMPLATES

```
OPTIONS 설정이 아닌 DIRS 안에 넣어줘야 한다.
```



### 11번

User(AbstractUser)

````
UserCreationForm -> UserCustomCreationForm
AUTH_USER_MODEL='accounts.User' # app이름으로 설정
get_user_model()로 함수 호출 가능
````



### 12번

Migration

```
makemigrations 명령어를 사용하면 등록된 app에 변경 사항이 있는 경우 migration 파일 생성
showmigrations migration 정보, 상태
sqlmigrate sql문 확인 가능
models.py의 모든 수정 사항이 migration을 추가 생성하지 않음
```



### 13번

```
request.user None이 아닌 익명의 user 정보가 들어있음
이미지 -> request.FILES
```



### 14번

```
path('articles/<int:id>/')

article = Article.objects.get(id=id)
```



### 15번

form의 이미지 파일 받는 인코딩 타입

```
enctype="multipart/form-data"
```



### 16번

로그인

```
user = form.get_user()
```



### 17번

Built-in validators

````
MinValueValidator
EmailValidator
URLValidator
````

```
PhoneNumberValidator
```



### 18번

```
STATIC_URL
INSTALLED_APP에 설정
STATICFILES_DIRS 설정
```

```
{% load static %} 재선언 필요
```



### 19번

````
@require_POST -> 405 상태코드 반환
from django.contrib.auth.decorators import login_required
@require_GET, @require_http_methods
````

```
@login_required -> 로그인 경로로 redirect
```



### 20번

```
{% if True %}
<h1>Header</h1>
{% endif %}
```



### 21번

startapp

```
admin.py
apps.py
migrations/
```

```
forms.py # 항상 새로 생성
```



### 22번

django query method

```
get_or_create
filter
get
```

```
offset
```



### 23번

Author / Book

```
isbn unique=True -> id는 자동생성
author table에 author_id column 생성
Author 오브젝트 삭제 시 Book 오브젝트 삭제
auth_now_add=True 자동생성
```



### 24번

True/False

```
is_authenticated
```



### 25번

```
GET
POST
PUT
DELETE
```

```
UPDATE
```

