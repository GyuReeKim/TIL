# Django (2019.10.28)

## API 서버만들기

> 데이터를 관리하는 서버를 만든다.

## django rest framework

> ` [https://www.django-rest-framework.org](https://www.django-rest-framework.org/) `



### Installation

```bash
$ pip install djangorestframework
```



### settings

```python
INSTALLED_APPS = [
    'rest_framework',
    'musics',
]
```



### models

```python
from django.db import models

# Create your models here.
class Artist(models.Model):
    name = models.TextField()
    def __str__(self):
        return self.name

class Music(models.Model):
    artist = models.ForeignKey(Artist, on_delete=models.CASCADE)
    title = models.TextField()
    def __str__(self):
        return self.title

class Comment(models.Model):
    music = models.ForeignKey(Music, on_delete=models.CASCADE)
    content = models.TextField()
    def __str__(self):
        return self.content
```



### admin

```python
from django.contrib import admin
from .models import Artist, Music, Comment

# Register your models here.
admin.site.register(Artist)
admin.site.register(Music)
admin.site.register(Comment)
```



### dump data

```bash
$ python manage.py dumpdata --indent 2 musics > my_dumpdata.json
```

dumpdata를 외부에 json형태로 변환시켜준다.

```json
[
{
  "model": "musics.artist",
  "pk": 1,
  "fields": {
    "name": "\ud2b8\uc640\uc774\uc2a4"
  }
},
{
  "model": "musics.artist",
  "pk": 2,
  "fields": {
    "name": "Aladin OST"
  }
},
{
  "model": "musics.music",
  "pk": 1,
  "fields": {
    "artist": 1,
    "title": "Feel Speical"
  }
},
{
  "model": "musics.music",
  "pk": 2,
  "fields": {
    "artist": 2,
    "title": "Speechless"
  }
},
{
  "model": "musics.comment",
  "pk": 1,
  "fields": {
    "music": 1,
    "content": "\uc88b\uc544\uc694!"
  }
},
{
  "model": "musics.comment",
  "pk": 2,
  "fields": {
    "music": 2,
    "content": "\ucd94\ucc9c\ud569\ub2c8\ub2e4!"
  }
}
]
```



### load data

> json 파일을 music 폴더로 옮긴 후 db.sqlite3 삭제 후 migrate 다시 해준다.

```bash
$ python manage.py loaddata musics/my_dumpdata.json
Installed 14 object(s) from 1 fixture(s)
```



### url 생성

> 데이터를 보낼 때 api/ 경로를 사용한다.

```python
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('musics.urls')), # 데이터를 보내는 url은 일반적으로 api/로 구성한다.
]
```



###  Django REST framework tutorial 2

> Requests and Responses, api_view를 search하자. api_view는 요청을 받는 방식을 말한다.

```python
from rest_framework.decorators import api_view
@api_view(['GET', 'POST'])
```



### Django REST framework tutorial 1

> serializers.py 생성

```python
from rest_framework import serializers
from .models import Music

# json처럼 직렬화 하는 것을 serialization이 해준다.
class MusicSerializer(serializers.ModelSerializer): # 모델을 가져와 처리를 해 줄 새로운 class 생성
    class Meta:
        model = Music
        fields = ('id', 'title', 'artist_id',)
```



### views.py

```python
from .models import Music
from rest_framework.decorators import api_view
from .serializers import MusicSerializer
from rest_framework.response import Response

# Create your views here.
@api_view(['GET'])
def music_list(request):
    musics = Music.objects.all() # musics를 json 형태로 바꿔줘야 한다. (musics는 query 형태)
    serializer = MusicSerializer(musics, many=True) # 직렬화 작업을 해준다.
    return Response(serializer.data)
```



### postman chrome extension

> 크롬 확장프로그램

설치 후 앱 클릭하고 google 로그인을 해야한다.

GET에 ` 127.0.0.1:8000/api/v1/musics/ `를 작성해보자.

나중에는 사용자가 아닌 Chrome, Vue.js, axios, android, ios, bixby 등이 요청을 보내면 django는 모든 곳에서 읽을 수 있는 json으로 응답해준다.



### detail 생성

> postman에 ` 127.0.0.1:8000/api/v1/musics/1/ ` 요청을 보내보자.

```python
urlpatterns = [
    path('musics/', views.music_list, name="music_list"), # api 서버에서는 name이 사실상 필요하지 않다.
    path('musics/<int:id>/', views.music_detail, name="music_detail",)
]
```

```python
@api_view(['GET'])
def music_detail(request, id):
    music = get_object_or_404(Music, id=id)
    serializer = MusicSerializer(music)
    return Response(serializer.data)
```



### drf-yasg - Yet another Swagger generator

>  사용설명서가 필요하다. ` https://github.com/axnsan12/drf-yasg#usage `

```bash
# Usage
# 0. Installation
$ pip install drf-yasg
```

settings에도 설정해준다.

```python
INSTALLED_APPS = [
   'drf_yasg',
]
```



### main url에 import 추가

> 개발자간 소통에 사용된다.

```python
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

schema_view = get_schema_view(
   openapi.Info(
      title="Music API",
      default_version='v1',
   ),
)

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('musics.urls')), # 데이터를 보내는 url은 일반적으로 api/로 구성한다.
    path('redoc/', schema_view.with_ui('redoc')),
    path('swagger/', schema_view.with_ui('swagger'))
]
```

redoc이나 swagger, 둘 중 하나를 사용하면 된다.



### artists도 비슷하게 만들기

> serializer를 좀 다르게 만들어준다.

```python
class ArtistDetailSerializer(serializers.ModelSerializer): # detail 페이지를 보여줄 때
    # music_set = MusicSerializer(many=True)
    # class Meta:
    #     model = Artist
    #     fields = ('id', 'name', 'music_set')
    # music_set이름을 바꾸고 싶을 때 source 사용
    musics = MusicSerializer(source="music_set", many=True)
    class Meta:
        model = Artist
        fields = ('id', 'name', 'musics')
```



### 댓글 생성

> serializer와 views 수정

```python
class CommentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Comment
        fields = ('id', 'content', 'music_id')
```

```python
@api_view(['POST'])
def comment_create(request, id):
    serializer = CommentSerializer(data=request.data)
    if serializer.is_valid():
        serializer.save(music_id=id) # commit=False와 같은 역할을 한다.
    return Response(serializer.data)
```



### validation 정확하게 수정

```python
@api_view(['POST'])
def comment_create(request, id):
    serializer = CommentSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True): # vaildation 과정에서 오류가 나면 404오류를 보내준다.
        serializer.save(music_id=id) # commit=False와 같은 역할을 한다.
    return Response(serializer.data)
```

빈칸을 제출하면 아래와 같이 나온다.

```json
{
    "content": [
        "This field may not be blank."
    ]
}
```



### GET, PUT, DELETE

```python
@api_view(['GET', 'PUT', 'DELETE'])
def comment_detail(request, id):
    comment = get_object_or_404(Comment, id=id)
    if request.method == "GET":
        serializer = CommentSerializer(comment)
        return Response(serializer.data)
    elif request.method == "PUT":
        serializer = CommentSerializer(data=request.data, instance=comment)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
            # return Response({'message':"업데이트!!!"}) # 수정되면 메세지를 보여준다.
    else:
        comment.delete()
        return Response({'message':"삭제되었습니다!!!"})
```



### Artist의 곡 count

```python
class ArtistDetailSerializer(serializers.ModelSerializer): # detail 페이지를 보여줄 때
    musics = MusicSerializer(source="music_set", many=True) # ArtistSerializer과 다르게 Artist의 모든 Music에 대한 정보를 볼 수 있다.
    musics_count = serializers.IntegerField(source="music_set.count") # source에는 music_set을 사용해야한다.
    class Meta:
        model = Artist
        fields = ('id', 'name', 'musics', 'musics_count')
```

