# Database Subject Test

### 1번

SQLite - Relational DBMS



### 2번

각 레코드의 고유값 - Primary Key



### 3번

DML - SELECT, DELETE, INSERT / CREATE는 TABLE을 생성



### 4번

CREATE TABLE classmates (

id INTEGER PRIMARY KEY,

name TEXT

); - 레코드는 가로줄을 말한다. 아직 데이터 생성 전



### 5번

CREATE TABLE classmates (

id INTEGER PRIMARY KEY,

name TEXT,

age INT,

address TEXT

); - INSERT INTO classmates VALUES (2, '홍길동', 20, '서울');



### 6번

name, phone 가져오기 - SELECT name, phone FROM students;



### 7번

삭제 - DELETE classmates WHERE id=3;



### 8번

SELECT COUNT(*) FROM users



### 9번

오름차순 ASC



### 10번

SELECT * FROM users WHERE age LIKE '2%'

SELECT * FROM users WHERE age LIKE '2-'

SELECT * FROM users WHERE age LIKE '2_%'



### 11번

SELECT name FROM classmates LIMIT 1 OFFSET 2;



### 12번

SELECT first_name FROM users WHERE age >= 20 and last_name="김";



### 13번

Article.objects.all()



### 14번

Article.new(content="Hello") -> 사용한 적 없음



### 15번

Article.objects.get(pk=2)

Article.objects.get(id=2)

Article.objects.filter(id=2)[0] -> filter는 list로 반환

find 사용한 적 없음



### 16번

comment.delete()



### 17번

auto_now_add



### 18번

CASCADE



### 19번

article.comment_set.all()

Comment.objects.filter(article_id=article.id)

Comment.objects.filter(article=article).all()

article.comments.all() -> related name이 선언되지 않았으므로 오류 발생



### 20번

title 내림차순 - -title



### 21번

User/Article M:N - user.like_articles.all()



### 22번

pizza.toppings.add(pepperoni, pineapple)



###23번

Profile.objects.create(user=user, nickname="Hello") - 1:1 관계

user.profile.create(nickname="Hello") -> 말이 되지 않음

profile = Profile.objects.create(nickname="Hello")

user.profile = profile -> user에 profile x



### 24번

ADD COLUMN



### 25번

LIMIT OFFSET