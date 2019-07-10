# StartCamp 2주차 (2019.07.10)

### Slack, Trello, GitHub 활용



## txt 문서 작성

##### 'w' : write (파일 작성)

```python
f = open('student.txt', 'w')
f.write("안녕하세요")
f.close()
```

##### 'a' : append (파일 덮어쓰기+추가)

```python
with open('ssafy.txt', 'a') as f: # with 안에서는 f를 open()으로 생각함
    f.write('화이팅 싸피!!!!!!!!!')
```

##### 'r' : read (파일 읽기)



### make_file.py

```python
import requests
from bs4 import BeautifulSoup
url = 'https://finance.naver.com/marketindex/exchangeList.nhn'
res = requests.get(url).text
soup = BeautifulSoup(res, 'html.parser')
tr = soup.select('tbody > tr') # F12의 정보 체크 필요

with open('exchange_all.txt', 'w') as f:
    for r in tr:
        print(r.select_one('.tit').text.strip()) # '.strip()':공백 제거
        print(r.select_one('.sale').text)
        f.write(r.select_one('.tit').text.strip())
        f.write(r.select_one('.sale').text + '\n') # '\n':줄바꿈
```

- 확장자 ex).md

- 프로그램 ex) Typora



## csv(파일 형식)

csv는 가공할 때 txt 보다 편리하다. 

### csv_file.py

```python
import csv

lunch = {
    "BBQ":"123123",
    "중국집":"789789",
    "한식":"456456"
}

with open("lunch.csv", 'w', encoding="utf-8", newline="") as f:
    									#한글 호환되는 인코딩
    									#window에서 빈칸 쓰지 않음.오류 방지
    csv_writer = csv.writer(f)  # csv를 쓸 수 있는 writer에게 명령.
    							#확장자에게 프로그램을 지정
    
    for item in lunch.items():
        csv_writer.writerow(item) # 한 줄을 써줘
```

코드를 실행하게 되면 .csv 파일이 만들어진다.



만약 이 파일의 이름을 'csv.py'처럼 import되는 'csv'와 동일하다면, 'csv.writer'가 파일이름 'csv'를 가져오게 된다. 다음과 같은 오류가 발생한다.

```
module 'csv' has no attribute 'writer'
```

- MarketPlace에서 Excel 설치
- .csv파일의 Overview로 표를 확인한다.

### exchange_csv.py

```python
import csv
import requests
from bs4 import BeautifulSoup
url = 'https://finance.naver.com/marketindex/exchangeList.nhn'
res = requests.get(url).text
soup = BeautifulSoup(res, 'html.parser')

tr = soup.select('tbody > tr')
with open("exchange_all.csv", 'w', encoding='utf-8', newline='') as f:
    csv_writer = csv.writer(f)
    for r in tr:
        row = [r.select_one('.tit').text.strip(), r.select_one('.sale').text]
        csv_writer.writerow(row)
```

환율정보를 .csv 파일로 만든다.

### bithumb_file.py

```python
import csv
import requests
from bs4 import BeautifulSoup
url = "https://www.bithumb.com/"
response = requests.get(url).text
soup = BeautifulSoup(response, 'html.parser')

tr = soup.select('tbody > tr')

with open('bithumb.csv', 'w', encoding='utf-8', newline='') as f:
    csv_writer = csv.writer(f)
    for r in tr:
        # print(r.select_one('strong').text.strip())
        # print(r.select_one('.sort_real').text)
        row = [r.select_one('strong').text.strip(), r.select_one('.sort_real').text]
        csv_writer.writerow(row)

```

빗썸 정보를 .csv 파일로 만든다.



수정할 부분을 블록 지정하고 tab키나 shift+tab키를 누르면 블록 부분의 위치의 앞부분 공백을 수정할 수 있다. 

나중에는 .txt나 .csv파일보다는 데이터베이스에 저장하게 된다.



## HTML(HyperText Markup Language)

마크업으로 구조를 만들며 뼈대만 구성한다.

HyperText : 다른 문서로 들어갈 수 있게 누를 수 있게 하는 하이퍼링크

사람들이 보기 좋게 만드는 것이 웹브라우저의 역할이다.

```html
<태그이름 속성명="속성값" 속성명2="속성값2">내용</태그이름>
```

'<h1>' : 여는 태그

'</h1>' : 닫는태그 /로 명시

'<a>' : 속성명, 속성값

'<ol>' : 순서있는 리스트 (order list)

#### HTML 수정방법

옮기기 원하는 부분을 블록 씌운 후 alt+화살표 키를 눌러서 <body> 안으로 들어가게 위치를 조정한다.

들여쓰기는 매우 중요하고, 처음부터 정리하는 습관 가지는 것이 좋다.

w3schools로 들어가면 html의 최신 버전을 공부할 수 있다.

태그 안에 태그 넣는것도 가능하다.

<strong>은 굵은글씨로 바꿔주고, <i>는 이탈릭체로 바꿔준다.

### intro.html

```html
<!DOCTYPE html>
<html>
    <head></head>
    <body>
        <h1 style="background-color:red;color:blue">HTML</h1>
        <h2>HyperText Markup Lanaguage</h2>
        <a style="color:brown" href="https://naver.com"> 네이버 </a>
        
        <!-- <태그이름 속성명="속성값" 속성명2="속성값2">내용</태그이름> -->
        
        <h3>우리가 공부한것</h3>
        <ol>
            <li> <strong><i>파이썬</i></strong> </li>
            <li>HTML</li>
            <li>Git</li>
        </ol>
    </body>
</html>
```

'<head>' 사이에 '<style>'을 넣어준다.

여기서 '<head>'는 생각할 때 쓰는 코드를 말한다.

```html
<style>
            h1 {
                background-color:red; # h1이 존재하는 모든 부분에 red 컬러를 준다.
            }							# 배경색을 지정한다.
            a {
                color: brown;
            }
            .blue {
                background-color: blue;
            }
        </style>
```

코드를 추가하면 다음과 같이 된다.

```html
<!DOCTYPE html>
<html>
    <head>
        <style>
            h1 {
                background-color:red;
            }
            a {
                color: brown;
            }
            .blue {
                background-color: blue;
            }
            #git {
                background-color: black;
            }
        </style>
    </head>
    <body>
        <h1>HTML</h1>
        <h1 class="blue">CSS</h1>
        <h2 class="blue">HyperText Markup Lanaguage</h2>
        <a href="https://naver.com"> 네이버 </a>
        
        <!-- <태그이름 속성명="속성값" 속성명2="속성값2">내용</태그이름> -->
        
        <h3>우리가 공부한것</h3>
        <ol>
            <li> <strong><i>파이썬</i></strong> </li>
            <li class="blue">HTML</li>
            <li id="git" class="blue">Git</li>
        </ol>
    </body>
</html>
```



## CSS

css는 포장을 말하며 presentation, appearance라고 할 수 있다.

javascript 움직임을 표현하며 dynamism, action이라고도 할 수 있다.

참고) http://info.cern.ch - home of the first website



css에서 '.'이 붙으면 class이며, class 가 좀더 우선시 된다.

우선순위는 id > class 이다.

> id : ex) 고유 아이디
>
> class : ex) 해시태그
>
> 

css파일 만든 후에 <head> 안에 link + tab 하면 자동완성이 된다.

w3schools에서 원하는 정보 찾아서 수정할 수 있다.

### 최종 intro.html

```
<!DOCTYPE html>
<html>
    <head>
        <link rel="stylesheet" href="./intro.css">
    </head>
    <body>
        <h1>HTML</h1>
        <h1 class="blue">CSS</h1>
        <h2 class="blue">HyperText Markup Lanaguage</h2>
        <a href="https://naver.com"> 네이버 </a>
        
        <!-- <태그이름 속성명="속성값" 속성명2="속성값2">내용</태그이름> -->
        
        <h3>우리가 공부한것</h3>
        <ol>
            <li> <strong><i>파이썬</i></strong> </li>
            <li class="blue">HTML</li>
            <li id="git" class="blue">Git</li>
        </ol>
    </body>
</html>
```

### 최종 intro.css

```
/* 여기는 css파일입니다!!! */
h1 {
    background-color:red;
}
a {
    color: brown;
}
.blue {
    background-color: blue;
}
#git {
    background-color: black;
}
```



## HTML 활용한 블로그 만들기

무료 탬플릿 사이트 https://startbootstrap.com/themes/

fab : font awesome https://fontawesome.com/

탬플릿을 다운받은 후 파일을 정리한다.

html을 수정할 수 있다.

```
git init
git add .
git commit -m "mypage"
주소복사
git push origin master
```





국내 개발자 블로그 모음 https://github.com/sarojaba/awesome-devblog/blob/master/README.md