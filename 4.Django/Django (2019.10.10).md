# Django (2019.10.10)

## 사진 업로드

### Static폴더 생성

> feeds 앱 안에 static 폴더를 만들어준다.

favicon의 주소를 복사한다. (ctrl+f:favicon)

`https://www.instagram.com/static/images/ico/favicon-192.png/68d99ba29cc8.png`



### base.html

```html
{% load static %}
<!-- static을 불러오려면 load static을 사용해야 한다. -->
<head>
  <link rel="icon" href="{% static 'insta.png' %}">
</head>
```



### memory cache

> 메모리를 임시저장한다.

캐시를 지울 때 강력 새로고침을 사용한다.

ctrl+shift+R



### navbar 만들기

> a 태그 안에 font awesome에서 가져온 'insta' 이미지의 주소를 넣어준다.

```html
<head>
  <script src="font awesome 로그인하면 나오는 주소 복사하기" crossorigin="anonymous"></script>
</head>
<body>
  <nav class="navbar navbar-light bg-light">
    <a class="navbar-brand"><i class="fab fa-instagram"></i> Instagram</a>
    <form class="form-inline">
      <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
      <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
    </form>
  </nav>
</body>
```



### model 만들기

```python
class Feed(models.Model):
    content = models.CharField(max_length=150)
    created_at = models.DateTimeField(auto_now_add=True)
    image = models.ImageField()
```

```bash
$ pip install Pillow # 이미지를 조작(기본적으로 이미지를 넣을 수 없기 때문)
```

db 날리고 다시 migrations 해줘야 한다.

migrations폴더의 initial 파일을 날려주면 된다.



### form 만들기

#### form.html

> enctype를 multipart/form-data로 준다.

```html
{% extends 'base.html' %}

{% block body %}
  <form action="" method="post" enctype="multipart/form-data">
    {% csrf_token %}
    <input class="form-control" type="text" name="content">
    <input class="form-control" type="file" name="image" accept="image/*">
    <input class="btn btn-primary" type="submit">
  </form>
{% endblock %}
```

` accept="image/*"`를 사용하면 표면적으로는 이미지만 올릴 수 있도록 변경된다.



#### views.py

```python
from django.shortcuts import render, redirect
from .models import Feed

def create(request):
    if request.method == "POST":
        content = request.POST.get('content')
        image = request.FILES.get('image')

        Feed.objects.create(content=content, image=image)
        
        return redirect('feeds:index')
    else:
        return render(request, 'form.html')
```



이미지는 INSTAGRAM 폴더 안에 저장된다. 중복된 이미지는 랜덤한 문자열이 추가된 상태로 저장된다.



### IPython

> jupyter notebook과 같은 shell 기능을 가진다.

#### views.py

```python
from IPython import embed

def create(request):
    if request.method == "POST":
        content = request.POST.get('content')
        image = request.FILES.get('image')

        feed = Feed.objects.create(content=content, image=image)
        embed()

        return redirect('feeds:index')
    else:
        return render(request, 'form.html')
```



어떤 데이터가 있는지 확인한다.

```
In [1]: image
Out[1]: <InMemoryUploadedFile: lion.png (image/png)>

In [2]: dir(image)
Out[2]:

In [3]: image.field_name
Out[3]: 'image'

In [4]: image.name
Out[4]: 'lion.png'

In [5]: feed.image
Out[5]: <ImageFieldFile: lion_IGXO5qn.png>

In [6]: feed.image.url
Out[6]: 'lion_IGXO5qn.png' # 최상단 파일 # 유지관리 어려움

In [7]: exit
```

서버 용량을 늘리는 것은 좋지 않으므로, 외부 저장소에 따로 이미지를 저장하는 편이 좋다.



### admin 페이지에서 확인

#### admin.py

```python
from .models import Feed

admin.site.register(Feed)
```

```bash
$ python manage.py createsuperuser
```



### Index에 이미지 표시

#### views.py

```python
def index(request):
    feeds = Feed.objects.all()
    context = {
        'feeds': feeds,
    }
    return render(request, 'index.html', context)
```



#### index.html

```html
{% for feed in feeds %}
<div class="card" style="width: 18rem;">
  <img src="{{feed.image.url}}" class="card-img-top" alt="...">
  <div class="card-body">
    <h5 class="card-title">Card title</h5>
    <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
</div>
{% endfor %}
```



#### settings.py

> `STATIC_URL = '/static/'`아래에 추가한다.

````python
MEDIA_URL = '/medea/'

MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
````

BASE_DIR은 현재 폴더로 경로를 지정해준다.

`BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))`



### Media의 파일들에 고유한 url 경로를 생성

#### urls.py (insta)

```python
from django.contrib import admin
from django.urls import path, include
from django.conf.urls.static import static
from django.conf import settings

urlpatterns = [
    path('admin/', admin.site.urls),
    path('feeds/', include('feeds.urls')),
]

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```



### 사진 규격 맞추기

#### 사진 자르기

```bash
$ pip install pilkit django-imagekit
```



#### settings.py

````
INSTALLED_APPS = [
    'imagekit',
]
````



#### models.py

```python
from imagekit.models import ProcessedImageField
from imagekit.processors import ResizeToFill

class Feed(models.Model):
    content = models.CharField(max_length=150)
    created_at = models.DateTimeField(auto_now_add=True)
    image = ProcessedImageField(
                processors = [ResizeToFill(300, 300)],
                format = 'JPEG',
                options = {'quality': 90},
                upload_to = 'feeds',
            )
```



![instagram_index](assets/instagram_index.PNG)