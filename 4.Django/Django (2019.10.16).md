# Django (2019.10.16)

## EITHER FORM 구성

### templates 폴더 안에 app이름 폴더 생성

```
project
	project
		app1
			templates
				index.html
		app2
			templates
				index.html
		app3
			templates
			
		main_templates
			index.html
			index.html
```

중복되기 때문에 문제가 발생한다. app을 구분해줘야 한다.



```
project
	project
		app1
			templates
				app1
					index.html
		app2
			templates
				app2
					index.html
		app3
			templates
		main_templates
			app1
				index.html
			app2
				index.html
```

나중에 있을 충돌을 방지하기 위해 templates 안에 폴더를 따로 만들어준다. app_name을 설정해줘야 한다.



## Question 모델&폼 생성

```python
class Question(models.Model):
    title = models.CharField(max_length=100)
    choice_1 = models.CharField(max_length=100)
    choice_2 = models.CharField(max_length=100)
```

```python
class QuestionForm(forms.ModelForm):
    class Meta:
        model = Question
        fields = '__all__'
```



## create 완성

```python
from django.shortcuts import render, redirect
from .forms import QuestionForm

def create(request):
    # 1. 사용자가 데이터를 입력하기 위해서 GET요청(폼을 요청)
    # 6. 사용자가 올바르지 않은 데이터를 입력하고 POST요청
    # 12. 사용자가 올바른 데이터를 입력하고 POST요청

    # 7. POST method로 들어오기 때문에 if문 실행
    # 13. POST method로 들어오기 때문에 if문 실행
    if request.method == "POST":
        # 8. 사용자의 데이터를 모델폼에 입력
        # 14. 사용자의 데이터를 모델폼에 입력
        form = QuestionForm(request.POST)
        # 9. 데이터가 올바른지 검증
        # 15. 데이터가 올바른지 검증
        if form.is_valid():
            # 16. 데이터가 검증을 통과하고 저장
            form.save()
            # 17. 저장 후 메인으로 이동
            return redirect('questions:index')
    # 2. GET method로 들어오기 때문에 else문 실행
    else:
        # 3. 사용자에게 빈 폼을 보여주기 위해서 모델폼을
        #    인스턴스화 한 후 form 변수에 저장
        form = QuestionForm()
    # 4. dict로 만들기
    # 10. 검증을 통과 못했을경우 => 올바른 데이터는 남기고 다시 폼 보여주기
    context = {
        'form': form,
    }
    # 5. form.html 보여주기
    # 11. form.html 보여주기
    return render(request, 'questions/form.html', context)
```



## read & update & delete완성

```python
def index(request):
    questions = Question.objects.all()
    # 나중에 있을 충돌을 방지하기 위해 templates 안에 폴더를 따로 만들어준다.
    return render(request, 'questions/index.html', {'questions': questions})

def detail(request, id):
    question = get_object_or_404(Question, id=id)
    choice_form = ChoiceForm()
    context = {
        'question': question,
        'choice_form': choice_form,
    }
    return render(request, 'questions/detail.html', context)

def update(request, id):
    question = get_object_or_404(Question, id=id)
    if request.method == "POST":
        form = QuestionForm(request.POST, instance=question)
        if form.is_valid():
            form.save()
            return redirect('questions:detail', id)
    else:
        form = QuestionForm(instance=question)
    context = {
        'form': form,
    }
    return render(request, 'questions/form.html', context)

def delete(request, id):
    if request.method == "POST":
        question = get_object_or_404(Question, id=id)
        question.delete()
        return redirect('questions:index')
    else:
        return redirect('questions:detail', id)
```



### detail

```html
<div class="jumbotron">
    <h1 class="display-4 text-center">{{question.title}}</h1>
    <p class="lead text-center">{{question.choice_1}} vs {{question.choice_2}}</p>
    <div class="progress my-3">
    <a href="{% url 'questions:update' question.id %}" class="btn btn-warning">수정</a>
    <form action="{% url 'questions:delete' question.id %}" method="post" class="d-inline">
      {% csrf_token %}
      <input type="submit" value="삭제" class="btn btn-danger">
    </form>
    <hr class="my-4">
    <a href="" class="btn btn-primary">왼쪽</a>
    <a href="" class="btn btn-danger">오른쪽</a>
</div>
```



## Choice 모델&폼 구성

```python
class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    pick = models.IntegerField()
    comment = models.CharField(max_length=100)
```

```python
class ChoiceForm(forms.ModelForm):
    choices = [(1, '왼쪽'), (2, '오른쪽')]
    pick = forms.ChoiceField(choices=choices, widget=forms.RadioSelect)
    class Meta:
        model = Choice
        fields = ('pick', 'comment',)
```



## Choice create 완성

```python
def choice_create(request, id):
    question = get_object_or_404(Question, id=id)
    if request.method == "POST":
        choice_form = ChoiceForm(request.POST)
        if choice_form.is_valid():
            # 비어있는 값(question)이 있기 때문에 commit=False를 적어준다.
            choice = choice_form.save(commit=False)
            choice.question = question
            choice.save()
            return redirect('questions:detail', id)
    else:
        pass
```

```html
<form action="{% url 'questions:choice_create' question.id %}" method="post">
  {% csrf_token %}
  {% bootstrap_form choice_form %}
  <input type="submit" value="고르기" class="btn btn-primary">
</form>
```



## Choice delete 완성

```python
def choice_delete(request, question_id, choice_id):
    choice = get_object_or_404(Choice, id=choice_id)
    if request.method == "POST":
        choice.delete()
        return redirect('questions:detail', question_id)
    else:
        pass
```



### detail (수정전)

```html
{% for choice in question.choice_set.all %}

  {% if choice.pick == 1 %}

    <div class="alert alert-primary" role="alert">
      {{choice.comment}}
      <form action="" method="post">
        {% csrf_token %}
        <input type="submit" value="삭제" class="btn btn-dark">
      </form>
    </div>

  {% else %}

    <div class="alert alert-danger" role="alert">
      {{choice.comment}}
    </div>

  {% endif %}

{% endfor %}
```



### detail (수정후)

```html
{% for choice in question.choice_set.all %}

  {% if choice.pick == 1 %}

    <div class="alert alert-primary" role="alert">
        
  {% else %}
        
    <div class="alert alert-danger" role="alert">
        
  {% endif %}
        
      {{choice.comment}}
      <form action="{% url 'questions:choice_delete' question.id choice.id %}" method="post">
        {% csrf_token %}
        <input type="submit" value="삭제" class="btn btn-dark">
      </form>
        
    </div>
{% endfor %}
```



### percentage를 보여주는 막대 추가

````html
<div class="jumbotron">
    <h1 class="display-4 text-center">{{question.title}}</h1>
    <p class="lead text-center">{{question.choice_1}} vs {{question.choice_2}}</p>
    <div class="progress my-3">
        
      <div class="progress-bar bg-primary" role="progressbar" style="width: {{percent_1}}%" aria-valuenow="15" aria-valuemin="0" aria-valuemax="100"></div>
      <div class="progress-bar bg-danger" role="progressbar" style="width: {{percent_2}}%" aria-valuenow="30" aria-valuemin="0" aria-valuemax="100"></div>
        
    </div>
    <a href="{% url 'questions:update' question.id %}" class="btn btn-warning">수정</a>
    <form action="{% url 'questions:delete' question.id %}" method="post" class="d-inline">
      {% csrf_token %}
      <input type="submit" value="삭제" class="btn btn-danger">
    </form>
    <hr class="my-4">
    <a href="" class="btn btn-primary">왼쪽</a>
    <a href="" class="btn btn-danger">오른쪽</a>
    <form action="{% url 'questions:choice_create' question.id %}" method="post">
      {% csrf_token %}
      {% bootstrap_form choice_form %}
      <input type="submit" value="고르기" class="btn btn-primary">
    </form>
</div>
````





## 새로운 앱 accounts 생성 (로그인 기능, sign in)

```bash
$ python manage.py showmigrations # 지금까지 migrations 결과를 보여준다.
$ python manage.py createsuperuser
```

settings와 urls를 설정해준다.



### accounts의 urls.py

```python
from django.urls import path
from . import views

app_name = 'accounts'

urlpatterns = [
    path('signup/', views.signup, name="signup"),
]
```



### accounts views.py

```python
from django.contrib.auth.forms import UserCreationForm

# Create your views here.
def signup(request): # create 방식과 같다.
    if request.method == "POST":
        form = UserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('questions:index')
    else:
        form = UserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/form.html', context)
```

```html
<ul class="nav justify-content-center">
  <li class="nav-item">
    <a class="nav-link active" href="{% url 'accounts:signup' %}">회원가입</a>
  </li>
</ul>
```



## login 기능

쿠키 & 세션 (f12 - application - cookies에서 확인 가능하다)

user 로그인은 세션에 정보를 저장하는 것이고 그 로직은 create와 같다.

```python
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm
from django.contrib.auth import login as auth_login # 경로 이름이 겹치므로 함수 이름을 변경

def login(request):
    if request.method == "POST":
        form = AuthenticationForm(request, request.POST)
        if form.is_valid():
            # form에서 가져온 user로 로그인한다.
            auth_login(request, form.get_user())
            return redirect('questions:index')
    else:
        form = AuthenticationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/form.html', context)
```

```html
<ul>
  <li class="nav-item">
    <a class="nav-link active" href="{% url 'accounts:login' %}">로그인</a>
  </li>
</ul>
```





## logout 기능

```python
from django.contrib.auth import logout as auth_logout

def logout(request):
    auth_logout(request)
    return redirect('questions:index')
```

```html
<ul>
  <li class="nav-item">
    <a class="nav-link disabled">{{user.username}}</a>
  </li>
    
  <li class="nav-item">
    <a class="nav-link active" href="{% url 'accounts:logout' %}">로그아웃</a>
  </li>
</ul>
```



## login, logout 분기처리

```html
<ul class="nav justify-content-center">
  <li class="nav-item">
    <a class="nav-link active" href="{% url 'questions:index' %}">홈</a>
  </li>
  <li class="nav-item">
    <a class="nav-link active" href="{% url 'questions:create' %}">글쓰기</a>
  </li>
    
  {% if user.is_authenticated %}
    
    <li class="nav-item">
      <a class="nav-link disabled">{{user.username}}</a>
    </li>
    <li class="nav-item">
      <a class="nav-link active" href="{% url 'accounts:logout' %}">로그아웃</a>
    </li>
    
  {% else %}
    
    <li class="nav-item">
      <a class="nav-link active" href="{% url 'accounts:signup' %}">회원가입</a>
    </li>
    <li class="nav-item">
      <a class="nav-link active" href="{% url 'accounts:login' %}">로그인</a>
    </li>
    
  {% endif %}
    
</ul>
```

