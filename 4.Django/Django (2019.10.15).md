# Django (2019.10.15)

## Form 사용하기

### `http://127.0.0.1:8000/` 실행시 나오는 페이지 변경

```python
from django.contrib import admin
from django.urls import path, include
from movies import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('movies/', include('movies.urls')),
    # `http://127.0.0.1:8000/`와 `http://127.0.0.1:8000/movies/`의 실행결과가 같다.
    path('', views.index),
]
```



### movies 앱폴더 안에 form.py 생성

> ` https://docs.djangoproject.com/en/2.2/topics/forms/ `의 'Building a form in Django'를 참고한다.

```python
class MovieForm(forms.Form):
    title = forms.CharField(max_length=100)
    title_en = forms.CharField(max_length=100)
    audience = forms.IntegerField()
    open_date = forms.DateField()
    genre = forms.CharField(max_length=100)
    watch_grade = forms.CharField(max_length=100)
    score = forms.FloatField()
    poster_url = forms.CharField(max_length=100)
    description = forms.CharField(max_length=100)
```

form의 형태를 자동으로 생성해주기 위해 사용한다.

DateField의 경우 원하는 형태로 나오지 않기 때문에 수정해줘야 한다.

```python
class MovieForm(forms.Form):
    title_en = forms.CharField(
                    max_length=100,
                    label="영문제목",
                    widget=forms.TextInput(
                        attrs={
                            'placeholder': '영문제목을 입력해주세요'
                        }
                    )
                )
    open_date = forms.DateField(
                    widget=forms.DateInput(
                        attrs={
                            'type': 'date'
                        }
                    )
                )
    # 다른 형태로 바꾸려면 widget으로 수정해야한다.
    poster_url = forms.CharField(widget=forms.Textarea)
    description = forms.CharField(widget=forms.Textarea)
```



### Create 생성

> create 함수의 else를 우선 작성한다.

```python
from django.shortcuts import render
from .forms import MovieForm

# Create your views here.
def index(request):
    return render(request, 'index.html')

def create(request):
    if request.method == "POST":
        pass
    else:
        form = MovieForm() # MovieForm을 불러온다.
        context = {
            'form': form,
        }
        return render(request, 'form.html', context)
```



#### forms.html

> form을 사용하는 이유는 데이터를 검증하기 위함이다. label과 id를 자동으로 생성해준다.

```html
{% extends 'base.html' %}
{% block body %}
  <h1>create</h1>
  <form action="" method="post">
    {% csrf_token %}
    {{form}}
    <input type="submit">
  </form>
{% endblock %}
```

html 코드를 살펴보면 required id가 있다. required는 validate(검증)을 위해 필요하다. 검증은 매우 중요한 과정이다.



### embed 확인

> views.py에서 embed를 확인해준다.

```python
from IPython import embed

def create(request):
    if request.method == "POST":
        form = MovieForm(request.POST)
        embed() # embed 함수 호출
        if form.is_valid():
            pass
        else:
            pass
```

```shell
In [1]: form
Out[1]: <MovieForm bound=True, valid=Unknown, fields=(title;title_en;audience;open_date;genre;watch_grade;score;poster_url;description)>
```



### embed 위치 변경

```python
def create(request):
    if request.method == "POST":
        form = MovieForm(request.POST)
        if form.is_valid():
            embed()
            pass
        else:
            pass
```

```shell
In [1]: form
Out[1]: <MovieForm bound=True, valid=True, fields=(title;title_en;audience;open_date;genre;watch_grade;score;poster_url;description)>

In [4]: form.cleaned_data
Out[4]:
{'title': '1',
 'title_en': '1',
 'audience': 1,
 'open_date': datetime.date(1111, 12, 12),
 'genre': '1',
 'watch_grade': '1',
 'score': 1.0,
 'poster_url': '1',
 'description': '1'}
```

위의 값과 비교하면 valid 값이 변경되는 것을 알 수 있다.

cleaned_data는 저장되는 값의 의미없는 공백을 없애준다.



### 개발자 도구에서 required를 지우고 제출

```python
def create(request):
    if request.method == "POST":
        form = MovieForm(request.POST)
        if form.is_valid():
            embed()
            pass
        else:
            embed()
            pass
```

```shell
In [1]: form
Out[1]: <MovieForm bound=True, valid=False, fields=(title;title_en;audience;open_date;genre;watch_grade;score;poster_url;description)>
```

valid가 False로 나온다.



### form 수정하기

```html
{% extends 'base.html' %}
{% block body %}
  <h1>create</h1>
  <form action="" method="post">
    {% csrf_token %}
    <!-- form 형태를 바꿔준다. -->>
    {{form.as_p}}
    {{form.as_ul}}
    <table>
      {{form.as_table}}
    </table>
    <!-- title.label만 출력 -->>
    {{form.title.label_tag}}
    <!-- title만 출력 -->>
    {{form.title}}
    <input type="submit">
  </form>
{% endblock %}
```



### create 함수 생성

```python
def create(request):
    if request.method == "POST":
        form = MovieForm(request.POST)
        # 데이터 검증이 되었을 때
        if form.is_valid():
            movie = Movie()
            movie.title = form.cleaned_data.get('title')
            movie.title_en = form.cleaned_data.get('title_en')
            movie.audience = form.cleaned_data.get('audience')
            movie.open_date = form.cleaned_data.get('open_date')
            movie.genre = form.cleaned_data.get('genre')
            movie.watch_grade = form.cleaned_data.get('watch_grade')
            movie.score = form.cleaned_data.get('score')
            movie.poster_url = form.cleaned_data.get('poster_url')
            movie.description = form.cleaned_data.get('description')
            movie.save()
            return redirect('movies:index')
        # 데이터 검증이 되지 않았을 때
        # else:
            # context = {
            #     'form': form,
            # }
            # return render(request, 'form.html', context)
            # pass
    else:
        form = MovieForm()
        # context = {
        #     'form': form,
        # }
        # return render(request, 'form.html', context)
    # 공통된 부분이므로 밖으로 빼줄 수 있다.
    context = {
        'form': form,
    }
    return render(request, 'form.html', context)
```



### get_object_or_404

```python
from django.shortcuts import render, redirect, get_object_or_404

def detail(request, id):
    # movie = Movie.objects.get(id=id)
    # 사용자가 찾을수 없는 정보에 접근할 때 404페이지를 보여준다.
    movie = get_object_or_404(Movie, id=id)
    context = {
        'movie': movie,
    }
    return render(request, 'detail.html', context)
```



### delete 함수 생성

> delete도 post방식으로 넘겨주는 것이 좋다.

```python
def delete(request, id):
    movie = get_object_or_404(Movie, id=id)
    if request.method == "POST":
        movie.delete()
        return redirect('movies:index')
    else:
        # GET요청이 들어오면 삭제시키지 않는다.
        return redirect('movies:detail', id)
```

```html
{% extends 'base.html' %}
{% block body %}
  <h1>detail</h1>
  <h3>{{movie.title}}({{movie.title_en}})</h3>
  <h4>{{movie.audience}} | {{movie.open_date}} | {{movie.genre}} | {{movie.watch_grade}} | {{movie.score}}</h4>
  <h5>{{movie.poster_url}}</h5>
  <h5>{{movie.description}}</h5>
  
  <!-- comment안의 내용을 html에서 보이지 않도록 한다. -->
  {% comment%}
    <a href="{% url 'movies:delete' movie.id %}">삭제</a>
  {% endcomment %}
  
  <form action="{% url 'movies:delete' movie.id %}" method="post">
    {% csrf_token %}
      <input type="submit" value="삭제(post)">
  </form>
  <a href="{% url 'movies:update' movie.id %}">수정</a>
{% endblock %}
```



#### comment & endcomment

```html
<!-- comment안의 내용을 html에서 보이지 않도록 한다. -->
{% comment%}
  <a href="{% url 'movies:delete' movie.id %}">삭제</a>
{% endcomment %}
```



### update 함수 생성

> create와 같은 'form.html'에 연결해주는 것이 가능하다.

```python
def update(request, id):
    movie = get_object_or_404(Movie, id=id)
    if request.method == "POST":
        form = MovieForm(request.POST)
        if form.is_valid():
            movie.title = form.cleaned_data.get('title')
            movie.title_en = form.cleaned_data.get('title_en')
            movie.audience = form.cleaned_data.get('audience')
            movie.open_date = form.cleaned_data.get('open_date')
            movie.genre = form.cleaned_data.get('genre')
            movie.watch_grade = form.cleaned_data.get('watch_grade')
            movie.score = form.cleaned_data.get('score')
            movie.poster_url = form.cleaned_data.get('poster_url')
            movie.description = form.cleaned_data.get('description')
            movie.save()
            return redirect('movies:detail', id)
    else:
        form = MovieForm(initial=movie.__dict__) # initial에 원본 데이터를 넣어준다.
    # 사용자가 옳지 않게 저장했을 때 기존의 데이터를 불러오기 위해서 코드를 아래로 빼서 작성하였다.
    context = {
        'form': form,
    }
    return render(request, 'form.html', context)
```



## Form 수정

> form을 사용하는 것이 불편하기에 수정이 필요하다.

### 새로운 form 생성

> MovieForm으로 쓰는것이 일반적

```python
# Model과 관련있는 form 생성
class MovieModelForm(forms.ModelForm):
    # 원래의 정보를 수정하거나 확장할 수 있다.
    open_date = forms.DateField(
                    widget=forms.DateInput(
                        attrs={
                            'type': 'date'
                        }
                    )
                )
    # 추가적인 데이터는 Meta에 저장된다.
    class Meta:
        model = Movie
        fields = '__all__'
```



### Create 생성

```python
def create_model_form(request):
    if request.method == "POST":
        form = MovieModelForm(request.POST)
        if form.is_valid():
            movie = form.save()
            return redirect('movies:detail', movie.id)
    else:
        form = MovieModelForm()
    context = {
        'form': form,
    }
    return render(request, 'form.html', context)
```



### Update 생성

```python
def update_model_form(request, id):
    movie = get_object_or_404(Movie, id=id)
    if request.method == "POST":
        form = MovieModelForm(request.POST, instance=movie)
        if form.is_valid():
            form.save()
            return redirect('movies:detail', id)
    else:
        form = MovieModelForm(instance=movie) # ModelForm에서는 instance가 기존의 정보를 보여준다.
    context = {
        'form': form,
    }
    return render(request, 'form.html', context)
```



### form 수정

> create인지 update인지 들어오는 값에 따라 다르게 보여주도록 설정한다.

```html
{% extends 'base.html' %}
{% block body %}
  {% if request.resolver_match.url_name == "create_model_form" %}
    <h1>create</h1>
  {% else %}
    <h1>update</h1>
  {% endif %}
  <form action="" method="post">
    {% csrf_token %}
    {{form.as_p}}
    <!-- {{form.title.label_tag}}
    {{form.title}} -->
    <input type="submit">
  </form>
{% endblock %}
```



## bootstarp 4

>  django bootstrap 4을 사용하여 form을 꾸민다. ` https://pypi.org/project/django-bootstrap4/ `

```html
{% load bootstrap4 %}
<head>
  {% bootstrap_css %}
</head>
<body>
  {% bootstrap_javascript jquery='full' %}
</body>
```

`{% load bootstrap4 %}`를 페이지마다 붙여줘야 한다.

` https://django-bootstrap4.readthedocs.io/en/latest/ `를 참고한다.



#### form.html

```html
{% extends 'base.html' %}
{% load bootstrap4 %}
{% block body %}
  {% if request.resolver_match.url_name == "create_model_form" %}
    <h1>create</h1>
  {% else %}
    <h1>update</h1>
  {% endif %}
  <form action="" method="post">
    {% csrf_token %}
    {% bootstrap_form form %}
    {% buttons submit="제출" %}{% endbuttons %}
  </form>
{% endblock %}
```



## Comment 만들기

### detail 함수 수정

> detail 페이지에 댓글을 보여준다.

```python
def detail(request, id):
    # movie = Movie.objects.get(id=id)
    # 사용자가 찾을수 없는 정보에 접근할 때 404페이지를 보여준다.
    movie = get_object_or_404(Movie, id=id)
    comment_form = CommentModelForm()
    context = {
        'movie': movie,
        'comment_form': comment_form,
    }
    return render(request, 'detail.html', context)
```

```html
{% extends 'base.html' %}
{% load bootstrap4 %}
{% block body %}
  <form action="{% url 'movies:comment_create' movie.id %}" method="post">
    {% csrf_token %}
    <!-- {{comment_form.as_p}}
    <input type="submit" value="제출"> -->
    {% bootstrap_form comment_form %}
    {% buttons submit="제출" %}{% endbuttons %}
  </form>
{% endblock %}
```



### form 추가

> `content`에 대한 정보만 필요할 경우 `fields`에 `content`를 넣어준다.

```python
class CommentModelForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = ('content',)
```



### comment_create

```python
def comment_create(request, id):
    movie = get_object_or_404(Movie, id=id)
    if request.method == "POST":
        form = CommentModelForm(request.POST)
        if form.is_valid():
            comment = form.save(commit=False)
            comment.movie = movie
            comment.save()
            return redirect('movies:detail', id)
        else:
            pass
    else:
        pass
```



## 순서 정렬

### 1. views.py에서 order_by 사용

```python
def index(request):
    movies = Movie.objects.all().order_by('-id') # 내림차순
    context = {
        'movies': movies,
    }
    return render(request, 'index.html', context)
```



### 2. models.py에서 ordering 사용

```python
class Comment(models.Model):
    movie = models.ForeignKey(Movie, on_delete=models.CASCADE)
    content = models.CharField(max_length=500)

    class Meta:
        # 순서 내림차순
        ordering = ('-id',)
```

