# Django (2019.10.07)

## Asked (댓글 작성하기)

### project & app 실행

```bash
$ django-admin startproject asked .
$ touch .gitignore
$ git init
$ git add .
$ git commit -m "내용"
$ django-admin startapp questions
```



### bootstrap 활용

> base.html의 body와 index.html의 body에 각각 Examples Album의  header과 main을 붙여넣는다.



### form 만들기

> 이전에 만든 create.html 대신 form.html을 만든다.

```
<div class="container w-50 my-3">
  <form action="" method="post">
    {% csrf_token %}
    <textarea class="form-control" name="content" id="" rows="5" placeholder="질문을 남겨주세요"></textarea>
    <input class="btn btn-danger" type="submit">
  </form>
</div>
```



### models.py 작성 이후

```bash
$ python manage.py makemigrations # 모델 생성
$ python manage.py migrate # 데이터베이스에 반영
```



### models.py

```python
class Question(models.Model):
    content = models.TextField()

class Answer(models.Model):
    content = models.CharField(max_length=100)
    # 값을 가져오기 위해 외래키를 사용한다.
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
```



### Answer 모델 생성 이후

```bash
$ pip install django-extensions
```

```python
# settings.py에 django-extensions 추가

INSTALLED_APPS = [
	'questions',
	'django_extensions'
]
```

```bash
$ pip install ipython # 초록색 표시
$ python manage.py shell_plus
```



### shell_plus (중요!)

```
In [1]: Qusetion
Out [1]: questions.models.Question

In [2]: Answer
Out [2]: questions.models.Answer

In [3]: Qusetion.objects.all()
Out[3]: <QuerySet [<Question: Question object (1)>, <Question: Question object (2)>]>

In [4]: Answer.objects.all()
Out[4]: <QuerySet []>
```



#### 게시물 불러오기

```shell
In [1]: question = Question.objects.get(id=1) # question에 Question id 1값 저장

In [2]: question
Out [2]: <Question: Question object (1)>
```



#### 댓글 생성

```shell
In [4]: answer = Answer() # 인스턴스 생성

In [5]: answer
Out [5]: <Answer: Answer object (None)>

In [6]: answer.content
Out [6]: ''

In [7]: answer.content = "이것은 댓글입니다." # 인스턴스 속성(변수)에 값을 저장

In [8]: answer
Out [8]: <Answer: Answer object (None)> # question 지정을 해주지 않아 값이 없다.

In [9]: answer.content
Out [9]: "이것은 댓글입니다."

In [11]: answer.question = question # answer에 question 지정

In [13]: answer.save() # 인스턴스 저장

In [14]: answer
Out [14]: <Answer: Answer object (1)>

In [15]: Answer.objects.create(content="두번째", question=question)
Out[15]: <Answer: Answer object (2)>
```



#### 댓글 정보

```shell
In [25]: answer.content
Out[25]: '이것은 댓글입니다.'

In [24]: answer
Out[24]: <Answer: Answer object (1)>

In [26]: answer.id
Out[26]: 1

In [27]: answer.pk
Out[27]: 1

In [31]: question.pk
Out[31]: 1

In [28]: answer.question_id # answer가 가지고 있는 id # 속도가 좀 더 빠름
Out[28]: 1

In [30]: answer.question.id # answer과 연결된 question이 가지고 있는 id
Out[30]: 1

In [32]: answer.question.pk
Out[32]: 1
```



#### 댓글 삭제

```shell
In [41]: answer.delete()
Out[41]: (1, {'questions.Answer': 1})
```



#### 1:N

- Question(1) => Answer(N) : `answer_set`

  - `question.answer`로는 가져올 수 없다.
  - 데이터가 없어도 항상 복수라고 생각한다.

  ```shell
  In [39]: question.answer_set.all()
  Out[39]: <QuerySet [<Answer: Answer object (1)>, <Answer: Answer object (2)>]>
  ```

  

- Answer(N) => Question(1) : `question`

  ```
  In [29]: answer.question
  Out[29]: <Question: Question object (1)>
  ```



#### dir

```shell
In [43]: dir(Question)
Out[43]: 
['DoesNotExist',
 'MultipleObjectsReturned',
 '__class__',
 '__delattr__',
 '__dict__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getstate__',
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
 '__setstate__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 '__weakref__',
 '_check_column_name_clashes',
 '_check_constraints',
 '_check_field_name_clashes',
 '_check_fields',
 '_check_id_field',
 '_check_index_together',
 '_check_indexes',
 '_check_local_fields',
 '_check_long_column_names',
 '_check_m2m_through_same_relationship',
 '_check_managers',
 '_check_model',
 '_check_model_name_db_lookup_clashes',
 '_check_ordering',
 '_check_property_name_related_field_accessor_clashes',
 '_check_single_primary_key',
 '_check_swappable',
 '_check_unique_together',
 '_do_insert',
 '_do_update',
 '_get_FIELD_display',
 '_get_next_or_previous_by_FIELD',
 '_get_next_or_previous_in_order',
 '_get_pk_val',
 '_get_unique_checks',
 '_meta',
 '_perform_date_checks',
 '_perform_unique_checks',
 '_save_parents',
 '_save_table',
 '_set_pk_val',
 'answer_set',
 'check',
 'clean',
 'clean_fields',
 'content',
 'date_error_message',
 'delete',
 'from_db',
 'full_clean',
 'get_deferred_fields',
 'id',
 'objects',
 'pk',
 'prepare_database_save',
 'refresh_from_db',
 'save',
 'save_base',
 'serializable_value',
 'unique_error_message',
 'validate_unique']

In [44]: dir(Answer)
Out[44]: 
['DoesNotExist',
 'MultipleObjectsReturned',
 '__class__',
 '__delattr__',
 '__dict__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getstate__',
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
 '__setstate__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 '__weakref__',
 '_check_column_name_clashes',
 '_check_constraints',
 '_check_field_name_clashes',
 '_check_fields',
 '_check_id_field',
 '_check_index_together',
 '_check_indexes',
 '_check_local_fields',
 '_check_long_column_names',
 '_check_m2m_through_same_relationship',
 '_check_managers',
 '_check_model',
 '_check_model_name_db_lookup_clashes',
 '_check_ordering',
 '_check_property_name_related_field_accessor_clashes',
 '_check_single_primary_key',
 '_check_swappable',
 '_check_unique_together',
 '_do_insert',
 '_do_update',
 '_get_FIELD_display',
 '_get_next_or_previous_by_FIELD',
 '_get_next_or_previous_in_order',
 '_get_pk_val',
 '_get_unique_checks',
 '_meta',
 '_perform_date_checks',
 '_perform_unique_checks',
 '_save_parents',
 '_save_table',
 '_set_pk_val',
 'check',
 'clean',
 'clean_fields',
 'content',
 'date_error_message',
 'delete',
 'from_db',
 'full_clean',
 'get_deferred_fields',
 'id',
 'objects',
 'pk',
 'prepare_database_save',
 'question',
 'question_id',
 'refresh_from_db',
 'save',
 'save_base',
 'serializable_value',
 'unique_error_message',
 'validate_unique']
```



#### 나가기

```shell
In [45]: exit
```

