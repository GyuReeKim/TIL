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
AUTH_USER_MODEL = 'accounts.User'
```



### admin 생성

> 기존의 User가 사라졌기 때문에 비슷한 형태로 만들어준다.

```
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import User

# Register your models here.
admin.site.register(User, UserAdmin)
```

