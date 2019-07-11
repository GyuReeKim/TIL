# StartCamp 2주차 (2019.07.11)

## Flask를 활용

python에서 새로운 파일을 만들고 vs code를 실행한다.

```
cd startcamp/
mkdir first_app
mv first_app/ 04.first_app #파일 이름 수정
code . # vs code 실행
```

Flask ( http://flask.pocoo.org/ )에서 코드를 가져온다.



#### Flask is Fun

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"
```

flask가 설치되어 있지 않으므로 터미널에 flask를 설치해준다.

```
pip install flask
```

pip list : 설치 된 pip 목록을 확인할 수 있다.



'app.py'를 실행해주기 위해 Flask에 있는 코드를 수정해서 터미널에 돌린다.

```python
FLASK_APP=app.py flask run # Flask는 app.py이며, flask를 돌려라
```

링크를 연결해보면 'Hello World!'가 출력되는 것을 확인할 수 있다.

#### app.py

```python
@app.route("/hi")
def hi():
    return "안녕하세요!!!"
```

여기서 `route("/")`는 서버에 요청을 보내는 주소를 말하며, "/" 뒤의 경로를 받는다.

"/" 뒤에 붙어있지 않다면, 그것은 서버주소 그 자체를 말한다.

"/"가 뒤에 있으면 "/"앞에 있는 것은 주소이며 "/"뒤 서버 안의 자원을 탐색한다.

> ctrl+c키 : 터미널을 종료시킨다.



주소 뒤에 "/hi"를 적으면 ''안녕하세요!!!''를 출력한다.

여기서 Debug mode는 off 되어있는 상태이며, 개발자는 production 단계에서 Debug mode를 끈 상태에서 완성시킨다.



서버를 다시 켜야 하는 불편함이 있어, Debug mode on을 해준다.

####  Debug mode on

```python
if __name__ == '__main__':
    app.run(debug=True)
```

```
python app.py #실행
```

Debug mode on이 된다. 이렇게 설정하면 내용을 수정해도 서버를 다시 돌릴 필요가 없다.



#### route 추가

```python
@app.route("/html_tag")
def html_tag():
    return "<h1>안녕하세요</h1>"
```

한 페이지에 여러줄을 출력하고 싶을 때는 """를 입력한다.

```python
@app.route("/html_tags")
def html_tags():
    return """
    <h1>안녕하세요</h1>
    <h2>반갑습니다</h2>
    """
```



#### 수업의 남은 날짜를 체크

```python
import datetime # datetime import 해줘야함
@app.route("/dday")
def dday():
    today = datetime.datetime.now()
    endday = datetime.datetime(2019,11,29)
    d = endday - today
    return f"1학기 종료까지 {d.days}일 남음!!"
```

여기서 문자열(string)로 출력하지 않으면 Type Error가 발생



html을 app.py에 작성하는 것은 비효율적이므로 templates 파일 생성하고, 파일 안에 index.html 생성한다. 여기서 모든 html 파일은 html 안에 들어있어야 한다.

html의 기능 중에 '! + tab키'를 입력하면 head와 body 구조를 가지는 html이 자동으로 만들어진다.



내용은 body안에 작성한다.

#### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>안녕하세요</title> # 주소창의 탭 이름 변경
</head>
<body>
    <h1>여기는 나의 첫번째 웹페이지</h1>
</body>
</html>
```

app.py로 가서 route를 작성한다.

```python
from flask import Flask, render_template #render_template를 사용하므로 추가
```

```python
@app.route("/html_file")
def html_file():
    return render_template('index.html')
```

render_template은 불완전한 html파일을 완전한 html파일로 바꿔주는 역할을 한다.



### Variable routing

Variable은 변수의 개념이다. Variable routing은 route 설정을 변수화하는 것이다. url에 변수를 넣는다.



#### 이름을 출력

```python
@app.route("/greeting/<string:name>") # 어떤 이름을 적던간에 name에 이름 저장
def greeting(name): # name을 함수 안에 넣어준다. name을 쓰려면 괄호 안에 넣어줘야한다.
    return f"안녕하세요 {name}님!!"
```

주소창에 '127.0.0.1:5000/greeting/이름'을 입력하면, '안녕하세요 이름님!!' 이 출력된다.



#### 세제곱을 출력

```python
@app.route("/cube/<int:num>")
def cube(num):
    cube_num = num**3 # '**'는 지수를 표현. '**3'은 세제곱을 의미
    return f"{num}의 세제곱은 {cube_num}입니다." # 중괄호 안에 파이썬 문법 그대로 적용
												# 하지만 추천하지 않음
```

주소창에 '127.0.0.1:5000/cube/숫자'을 입력하면, '숫자의 세제곱은 숫자^3입니다.' 가 출력된다.



### render_template로 응용

#### 세제곱 출력

```python
@app.route("/cube_html/<int:num>")
def cube_html(num):
    cube_num = num**3
    return render_template("cube.html", num_html=num, cube_num_html=cube_num)
```

여기서 'num_html=num, cube_num_html = cube_num'은 오른쪽을 왼쪽에 대입한다고 생각하면 된다.

#### cube.html

```html
<body>
    <strong>{{num_html}}</strong>의 세제곱은 <i>{{cube_num_html}}</i>입니다.
    <!-- 중괄호 두개 사이에 파이썬 함수를 담을 수 있다. -->
</body>
```

' {{ }} '는파이썬 코드를 html에서도 사용가능하게 해준다.

파이썬 코드는 html 내부에서도 이용가능하다.

숫자 데이터만 바꿀 수 있다.



만약 '{{ }}'를 html의 주석에 넣고싶다면 {# {{}} #} 처럼 표현해야 한다.

' {{ }} '를 쓰게 되면 ' {{ }} '를 계속 찾게 되어 오류가 발생한다.



#### 인사 출력

```python
@app.route("/greeting_html/<string:name>")
def greeting_html(name):
    return render_template("greeting.html", name=name)
```

#### greeting.html

```html
<body>
    <p style="color:fuchsia">{{name}}</p>님 안녕하세요!!!
</body>
```



#### 점심 메뉴 추천하는 코드

```python
import random
@app.route("/lunch")
def lunch():
    menu = {
        "짜장면":"https://upload.wikimedia.org/wikipedia/commons/c/c6/Jajangmyeon_topped_with_Samgyeopsal.jpg",
        "짬뽕":"https://cdn.pixabay.com/photo/2014/06/18/09/33/spicy-seafood-371000_960_720.jpg",
        "스파게티":"https://cdn.pixabay.com/photo/2018/01/11/10/58/food-3075854_960_720.jpg",
    }

    menu_list = list(menu.keys()) # ["짜장면", "짬뽕", "스파게티"]
    pick = random.choice(menu_list)
    img = menu[pick]

    return render_template("lunch.html", pick=pick, img=img)
```

#### lunch.html

```html
<body>
    <h3>오늘의 점심은 {{pick}} 어떠세요??</h3>
    <img src="{{img}}" alt="">
</body>
```

html에서 ' img + tab키 '를 입력하면 여는 태그만 존재하는 것을 확인할 수 있다.



## html의 조건문

### html의 if문

greeting.html을 수정한다.

```python
@app.route("/greeting_html/<string:name>")
def greeting_html(name):
    return render_template("greeting.html", name=name)
```

#### greeting.html

```html
<body>
    {% if name == "규리" %}
        <p style="color:fuchsia">{{name}}</p> 야 안녕!!
    {% else %}
        <p style="color:fuchsia">{{name}}</p>님 안녕하세요!!!
    {% endif %}
</body>
```

if else의 경우, 로그인 했을 때 사용할 수 있다.



### html의 for문

#### 영화 추천하는 코드

```python
@app.route('/movies')
def movies():
    movie_list = ['스파이더맨', '토이스토리', '알라딘', '존윅']
    return render_template("movies.html", movie_list=movie_list)
```

#### movies.html

```html
<body>
    <ol>
        {% for movie in movie_list %}
            <li>{{movie}}</li> # 중괄호 안의 movie는 위의 movie와 같다.
        {% endfor %}
    </ol>
</body>
```

html에서 if문과 for문은 {% endif %}와 {% endfor %}로 끝을 닫아놓고 시작하는 것이 좋다.



## Home

사용자가 입력하는 데이터를 얻기 위하여 Home을 사용한다.

사용자가 url을 직접 수정하는 것은 사실상 어렵기 때문에 이용한다.

### ping/pong

```python
from flask import Flask, render_template, request # pong의 request를 가져옴

@app.route("/ping")
def ping():
    return render_template("ping.html")

@app.route("/pong")
def pong():
    user_input = request.args.get("test") # args는 매개변수의 약자. 딕셔너리를 나타냄
    									  # arguement
    return render_template("pong.html", user_input=user_input)
													#user_input은 html로 받는다.
```

https://search.naver.com/search.naver?sm=top_hty&fbm=1&ie=utf8&query=안녕

'?'뒤는 파라미터라고 하고, key와 value는 한 쌍으로 본다.

#### ping.html

````html
<body>
    <h1>여기는 핑입니다.</h1>
    <form action="/pong">
        <input type="text" name="test"> # name : 변수의 이름
        <input type="submit">
    </form>
</body>
````

'127.0.0.1:5000/ping'의 제출 창에 ssafy를 입력하면 '127.0.0.1:5000/ping?test=ssafy'가 나온다.

#### pong.html

```html
<body>
    <h1>여기는 퐁입니다.</h1>
    사용자가 방금 입력한 데이터는
    <p>{{user_input}}</p>
    입니다.
</body>
```



 #### 네이버 페이크 검색창

```python
@app.route("/naver")
def naver():
    return render_template("naver.html")
```

#### naver.html

```html
<body>
    <form action="https://search.naver.com/search.naver"> # 물음표 전까지 복사
        <input type="text" name="query">
        <input type="text" name="hihi"> # 입력해도 검색 결과에 영향을 주지 않음
        <input type="submit">
    </form> # 여는 태그를 작성하면 닫는 태그도 작성하는 것이 좋다.
</body>
```



## ASCII art API

> artii.herokuapp.com 입력한 글자를 아스키코드로 바꿔주는 사이트



```python
import requests

@app.route("/text")
def text():
    return render_template("text.html")

@app.route("/result")
def result():
    raw_text = request.args.get('raw')
    url = "http://artii.herokuapp.com/make?text="
    res = requests.get(url+raw_text).text # url뒤에 raw_text를 붙인 주소를 가져옴
    										# 내부에서 또 다른 요청을 보낼 수 있음
    return render_template("result.html", res=res)
```

#### text.html

```html
<body>
    <h1>입력한 글자를 아스키 코드로 바꿔줍니다.</h1>
    <form action="/result">
        <input type="text" name="raw">
        <input type="submit">
    </form>
</body>
```

#### result.html

```html
<body>
    <h1>결과는 다음과 같습니다!</h1>
    <pre>{{res}}</pre>
</body>
```

#### text.html type 변경

```html
<input type="email" name="raw"> # @가 없으면 제출 불가
<input type="password" name="raw"> # 검정색 점으로 출력
<input type="number" name="raw"> # 숫자만 출력 가능
<input type="submit" value="변경!!!"> #'제출' 버튼을 '변경!!!'으로 변경

```

text.html에서 type를 변경해보면, 언어에 맞는 단어로 바뀐다.



## 랜덤프로그램 만들기

```python
@app.route("/random_game")
def random_game():
    return render_template('random_game.html')

@app.route("/random_result")
def random_result():
    mood_list = ['안정적입', '불안정합', '행복합', '힘들어 보입', '제정신이 아닙']
    pick_mood = random.choice(mood_list)
    food_list = ['사과', '블루베리', '귤', '영양제', '영양식', '떡']
    pick_food = random.choice(food_list)
    return render_template('random_result.html', pick_mood=pick_mood, pick_food=pick_food)
```

#### random_game.html

```html
<body>
    <h1>색깔을 선택하세요. 당신의 상태를 알려줍니다.</h1>
    <form action="/random_result">
        <input type="color" name="write">
        <input type="submit" value="결과는?">
    </form>
</body>
```

#### random_result.html

```html
<body>
    <h1>당신의 상태는 {{pick_mood}}니다.</h1>
    <h2>당신에게 {{pick_food}}를 추천합니다.</h2>
</body>
```

랜덤 프로그램을 만들어보았다. 기회가 되면 수정해보자.



## ngrok

ngrok를 설치한다.

압축을 풀고 student 파일에 옮긴다.

vs code에서 새로운 터미널을 생성 후 아래와 같이 입력한다.

```
cd ~
./ngrok.exe http 5000
```



## 로또

```python
@app.route('/lotto')
def lotto():
    return render_template("lotto.html")

@app.route('/lotto_result')
def lotto_result():
    #사용자가 입력한 정보를 가져오기 request : 내부 정보 가져오기
    numbers = request.args.get('numbers').split()
    user_numbers = []
    for n in numbers:
        user_numbers.append(int(n))
    #user_numbers = [1,2,3,4,5,6] 입력을 문자열로 받기 때문에 int() 사용

    # 로또 홈페이지에서 정보를 가져오기 requests : 외부 정보 요청
    url = 'https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=866'
    res = requests.get(url)
    lotto_numbers = res.json()

    winning_numbers = []
    for i in range(1,7):
        winning_numbers.append(lotto_numbers[f'drwtNo{i}'])
    bonus_number = lotto_numbers['bnusNo']

    matched = len(set(user_numbers) & set(winning_numbers))
    
    if matched == 6:
        result = "1등"
    elif matched == 5:
        if bonus_number in user_numbers:
            	# user_numbers에 bonus_number가 있는지 확인
            result = "2등"
        else:
            result = "3등"
    elif matched == 4:
        result = "4등"
    elif matched == 3:
        result = "5등"
    else:
        result = "꽝"

    return render_template("lotto_result.html", u=user_numbers, w=winning_numbers, b=bonus_number, r=result)
```



### Set(집합)

> 'user_numbers=[]' 와 같이 집합으로 만든다.



집합들 중 겹치는 원소를 찾기 위해서는 u(user)라고 하는 집합을 만든 후, 집합을 겹쳐 교집합이 몇개인지 물어본다.

Set도 Dictionary처럼 중괄호를 사용한다. 하지만 Key와 Value가 있는 Dictionary와 달리, Set은 두 값을 가지지 않는다.



#### lotto.html

```html
<body>
    <form action="/lotto_result">
        <input type="text" name="numbers">
        <input type="submit">
    </form>
</body>
```

#### lotto_result.html

```html
<body>
    {{u}}
    {{w}}
    {{b}}
    {{r}}
</body>
```
