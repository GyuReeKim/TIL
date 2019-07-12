# StartCamp 2주차 (2019.07.12)

## Telegram

> Telegram에서 '안녕'이라고 입력하면 Flask로 전송해주고, 그 전송된 값을 또 다른 api 서버로 요청 보낸 후, 그 데이터를 다시 사용자에게 보내주는 챗봇을 만들어본다.



Telegram을 핸드폰에서 가입해준다.

Telegram에서 'BotFather' 공식 계정을 찾고 'Start' 버튼을 누르면 대화할 수 있다.



### BotFather

BotFather에서 대화를 시작한다.

```
/start
/newbot
봇이름_bot
```

위를 순서대로 입력하게 되면 BotFather에서 고유한 token 값을 보내준다. 이때 token은 본인이 개발자라는 것을 알려주는 열쇠같은 역할을 하므로 다른 사람에게 공유하지 않은 것이 좋다.

BotFather에서 보내준 `t.me/봇이름_bot`을 클릭해주면 '봇이름_bot'이 생성된다.



### test.py

```
token = ""
url = f"https://api.telegram.org/bot{token}/"
print(url)
```



터미널에 돌린 주소를 열면 아래와 같이 뜨는 것을 확인할 수 있다.

```
{
  "ok": false,
  "error_code": 404,
  "description": "Not Found"
}
```

주소창에 /get updates를 붙여주면 아래와 같이 반환된다.

```
{
  "ok": true,
  "result": [
    
  ]
}
```



telegram에서 자신의 봇을 실행한 뒤에 페이지를 새로고침한 다음, from 아래의 아이디 값을 복사해준다.

```
user_id = ""
```





### Telegram Bot API

available methods의 sendMessage안에 있는 Parameter를 확인해본다. 

'&'는 파라미터 사이 공간을 의미한다.



#### 개인한테 메세지를 보내기

```python
import requests
token = "비밀"
url = f"https://api.telegram.org/bot{token}/"
print(url)

user_id = "비밀"

send_url = f"{url}sendMessage?chat_id={user_id}&text=하이하이"
requests.get(send_url)
```



`pip install python-decouple`

'token'과 같은. 숨겨야하는 변수들 라이브러리를 숨겨준다.



### .env

> .env 파일은 띄어쓰기를 하면 안된다. 일반적으로는 대문자와 '_'가 쓰인다.

```
TELEGRAM_TOKEN='비밀'
CHAT_ID='비밀'
```



### .gitignore

> git에 올리지 않는다.

Python, Windows, VisualStudioCode



### app.py

#### write/send

```python
from decouple import config # 환경변수를 밖으로 뺀다.

api_url = "https://api.telegram.org"
token = config("TELEGRAM_TOKEN")
chat_id = config("CHAT_ID")

@app.route("/write")
def write():
    return render_template("write.html")

@app.route("/send")
def send():
    msg = request.args.get('msg')
    url = f"{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={msg}"
    res = requests.get(url)
    return render_template("send.html")
```

#### write.html

```html
<body>
    <form action="/send">
        <input type="text" name="msg">
        <input type="submit">
    </form>
</body>
```

'127.0.0.1:5000/write'에 '이거 전달해줘'를 입력하면 '127.0.0.1:5000/send?msg=이거전달해줘'가 주소창에 뜬다.



url을 분석하는 것은 중요하다.

```python
url = f"{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={msg}"
```

#### send.html

````html
<body>
    <h1>성공적으로 메세지가 전달되었습니다.</h1>
</body>
````



## Webhook 기능

```python
@app.route(f"/{token}", methods=['POST'])
def telegram(): # 위 이름과 같을 필요 없다.
    print(request.get_json())
    data = request.get_json()
    user_id = data.get('message').get('from').get('id')
    user_msg = data.get('message').get('text')
    
    res_url = f"{api_url}/bot{token}/sendMessage?chat_id={user_id}&text={user_msg}"
    requests.get(res_url)
    return '', 200
```

Webhook을 설정한다. 우선 윈도우에서 cmd를 연 후에 `ngrok.exe http 5000`을 입력하면 'http://45688737.ngrok.io'의 주소가 나온다. 링크를 열어보면 not found 404가 나온다.



test.py에서 아래와 같은 코드를 작성한다.

```python
#send_url = f"{url}sendMessage?chat_id={user_id}&text=하이하이"
#requests.get(send_url)

ngrok_url = "https://45688737.ngrok.io"
webhook_url = f"{url}setWebhook?url={ngrok_url}/{token}" # 경로를 알려준다.
print(webhook_url)
```



실행해서 나오는 링크를 연결 해주면 아래와 같이 뜬다.

```
{
  "ok": true,
  "result": true,
  "description": "Webhook was set"
}
```



app.py를 다시 실행시킨 다음에 텔레그램에 메세지를 보내면 답이 오지는 않지만 터미널에 메시지가 뜬다.

```python
@app.route(f"/{token}", methods=['POST'])
def telegram():
    print(request.get_json())
    data = request.get_json()
    user_id = data.get('message').get('from').get('id') # 터미널을 분석해서 get함
    user_msg = data.get('message').get('text')
```



터미널에 나온 괄호를 분석해본다.

```
{'update_id': 82362131, 
'message': {'message_id': 15, 
'from': {'id': 비밀, 'is_bot': False, 'first_name': '규리', 'last_name': '김', 'language_code': 'ko'}, 
'chat': {'id': 비밀, 'first_name': '규리', 'last_name': '김', 'type': 'private'}, 
'date': 1562897130, 'text': '안녕'}
}
```





## Random 출력

```python
from bs4 import BeautifulSoup
import requests
import random
@app.route(f"/{token}", methods=['POST'])
def telegram():
    print(request.get_json())
    data = request.get_json()
    user_id = data.get('message').get('from').get('id')
    user_msg = data.get('message').get('text')

    if user_msg == "점심메뉴":
        menu_list = ["삼계탕", "철판낙지볶음밥", "물냉면"]
        result = random.choice(menu_list)
    elif user_msg == "로또":
        numbers = list(range(1, 46))
        result = sorted(random.sample(numbers, 6))
    elif user_msg == "가위":
        game = ['가위', '바위', '보']
        g = random.choice(game)
        if g == '가위':
            result = f"{g}, 당신은 이겼습니다."
        elif g == '바위':
            result = f"{g}, 당신은 졌습니다."
        else:
            result = f"{g}, 당신은 이겼습니다."
    elif user_msg == "환율":
        response = requests.get('https://finance.naver.com/marketindex/').text
        soup = BeautifulSoup(response, 'html.parser')
        exchange = soup.select_one('.value')
        result = f"현재 환율은 {exchange.text}원 입니다."
    elif user_msg == "오늘날씨":
        response = requests.get('https://search.naver.com/search.naver?where=nexearch&sm=tab_etc&query=%EC%98%A4%EB%8A%98%EB%82%A0%EC%94%A8').text
        soup = BeautifulSoup(response, 'html.parser')
        weather = soup.select_one('.todaytemp')
        result = f"오늘의 날씨는 {weather.text}도 입니다."
    else:
        result = user_msg
```

Telegram에 user_msg를 입력하면 result를 출력한다.



## 네이버 번역

>  파파고를 이용한다.

네이버 api 검색 가입

#### .env

```
NAVER_ID='비밀'
NAVER_SECRET='비밀'
```

네이버 api을 연결하기 위해 .env에 두 정보를 추가해준다.

#### app.py

```python
naver_id = config("NAVER_ID")
naver_secret = config("NAVER_SECRET")

@app.route(f"/{token}", methods=['POST'])
def telegram():
    print(request.get_json())
    data = request.get_json()
    user_id = data.get('message').get('from').get('id')
    user_msg = data.get('message').get('text')

    # (생략)
    elif user_msg[0:2] == "번역":
        # 번역 안녕하세요 저는 누구입니다.
        raw_text = user_msg[3:]
        # [3:] : 3부터 시작해서 끝까지
        papago_url = "https://openapi.naver.com/v1/papago/n2mt"
        data = {
            "source":"ko", # 네이버 api를 쓰려면 이 정보를 입력해주어야 함.
            "target":"en",
            "text":raw_text
        }
        header = {
            'X-Naver-Client-Id':naver_id,
            'X-Naver-Client-Secret':naver_secret
        }
        res = requests.post(papago_url, data=data, headers=header)
        translate_res = res.json()
        translate_result = translate_res.get('message').get('result').get('translatedText')
        result = translate_result
    # (생략)

```



###### 작성예시

```
curl "https://openapi.naver.com/v1/papago/n2mt" \
								# curl : 특정 주소를 알려주면 그 주소로 요청을 보낸다.
-H "Content-Type: application/x-www-form-urlencoded; charset=UTF-8" \
											# '-H'(헤더) : 필요한 정보를 가지고 있음
-H "X-Naver-Client-Id: 비밀" \
-H "X-Naver-Client-Secret: 비밀" \
-d "source=ko&target=en&text=만나서 반갑습니다." -v
```



## Telegram에 사진보내기

```python
if data.get('message').get('photo') is None:
        if :
        # (생략)
        else:
            result = user_msg
    else:
        # 사용자가 보낸 사진을 찾는 과정
        result="picture"
        file_id = data.get('message').get('photo')[-1].get('file_id')
        								# [-1]: 맨 마지막 요소에 접근할 때 사용
        file_url = f"{api_url}/bot{token}/getFile?file_id={file_id}"
        file_res = requests.get(file_url)
        file_path = file_res.json().get('result').get('file_path')
        file = f"{api_url}/file/bot{token}/{file_path}"
        print(file)
```



Telegram은 200을 받지 못하면 오류가 발생한다. 만약 사진을 파일로 잘못 보내 오류가 발생한다면 번역부분을 주석 처리 후 재 실행한 다음 다시 실행하면 정상적으로 작동한다.



## 클로바 얼굴인식

> 클로바 얼굴인식을 이용한다.

```python
	else:
		# 사용자가 보낸 사진을 클로버로 전송
	    res = requests.get(file, stream=True)
	    clova_url = "https://openapi.naver.com/v1/vision/celebrity"
	    header = {
	            'X-Naver-Client-Id':naver_id,
	            'X-Naver-Client-Secret':naver_secret
	        }
	    clova_res = requests.post(clova_url, headers=header, files={'image':res.raw.read()})
	    print(clova_res.text)
```



## python anywhere.com

>  서버를 분양받는 사이트. 서버 주소를 ngrok에서 이 사이트로 바꾼다.



decouple을 설치해준다.

```
pip3 install python-decouple --user
```

test.py에서 ngrok 주소를 python anywhere 주소로 바꾼후 실행시키면 아래와 같이 뜨는 것을 확인할 수 있다.

```
{
  "ok": true,
  "result": true,
  "description": "Webhook was set"
}
```

코드 수정 후에는 reload 항상 눌러줘야 한다.



### 최종 test.py

```
import requests
from decouple import config
token = config("TELEGRAM_TOKEN")
url = f"https://api.telegram.org/bot{token}/"
user_id = config("CHAT_ID")

ngrok_url = "https://gyureekim.pythonanywhere.com/"
webhook_url = f"{url}setWebhook?url={ngrok_url}/{token}"
print(webhook_url)
```



### 최종 app.py

```python
from flask import Flask, request, render_template
from decouple import config
from bs4 import BeautifulSoup
import requests
import random

app = Flask(__name__)

api_url = "https://api.telegram.org"
token = config("TELEGRAM_TOKEN")
chat_id = config("CHAT_ID")
naver_id = config("NAVER_ID")
naver_secret = config("NAVER_SECRET")

@app.route("/write")
def write():
    return render_template("write.html")

@app.route("/send")
def send():
    msg = request.args.get('msg')
    url = f"{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={msg}"
#    res = requests.get(url)
    return render_template("send.html")

@app.route(f"/{token}", methods=['POST'])
def telegram():
    print(request.get_json())
    data = request.get_json()
    user_id = data.get('message').get('from').get('id')
    user_msg = data.get('message').get('text')

    if data.get('message').get('photo') is None:
        if user_msg == "점심메뉴":
            menu_list = ["삼계탕", "철판낙지볶음밥", "물냉면"]
            result = random.choice(menu_list)
        elif user_msg == "로또":
            numbers = list(range(1, 46))
            result = sorted(random.sample(numbers, 6))
        # 가위바위보 게임
        elif user_msg == "가위":
            game = ['가위', '바위', '보']
            g = random.choice(game)
            if g == '가위':
                result = f"챗봇:{g}, 당신은 비겼습니다."
            elif g == '바위':
                result = f"챗봇:{g}, 당신은 졌습니다."
            else:
                result = f"챗봇:{g}, 당신은 이겼습니다."
        elif user_msg == "바위":
            game = ['가위', '바위', '보']
            g = random.choice(game)
            if g == '가위':
                result = f"챗봇:{g}, 당신은 이겼습니다."
            elif g == '바위':
                result = f"챗봇:{g}, 당신은 비겼습니다."
            else:
                result = f"챗봇:{g}, 당신은 졌습니다."
        elif user_msg == "보":
            game = ['가위', '바위', '보']
            g = random.choice(game)
            if g == '가위':
                result = f"챗봇:{g}, 당신은 졌습니다."
            elif g == '바위':
                result = f"챗봇:{g}, 당신은 이겼습니다."
            else:
                result = f"챗봇:{g}, 당신은 비겼습니다."
        # 급상승검색어
        elif user_msg == "급상승검색어":
            response = requests.get('https://www.naver.com/').text
            soup = BeautifulSoup(response, 'html.parser')
            search = soup.select_one('#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_list.PM_CL_realtimeKeyword_list_base > ul:nth-child(5) > li:nth-child(1) > a.ah_a > span.ah_k')
            result = f"현재 급상승검색어 1위는 {search.text}입니다."
        elif user_msg == "환율":
            response = requests.get('https://finance.naver.com/marketindex/').text
            soup = BeautifulSoup(response, 'html.parser')
            exchange = soup.select_one('.value')
            result = f"현재 환율은 {exchange.text}원 입니다."
        elif user_msg == "오늘날씨":
            response = requests.get('https://www.google.com/search?q=google+weather').text
            soup = BeautifulSoup(response, 'html.parser')
            weather = soup.select_one('#wob_tm')
            result = f"오늘의 날씨는 {weather.text}도 입니다."
        elif user_msg[0:2] == "번역":
            # 번역 안녕하세요 저는 누구입니다.
            raw_text = user_msg[3:]
            #[3:] : 3부터 시작해서 끝까지
            papago_url = "https://openapi.naver.com/v1/papago/n2mt"
            data = {
                "source":"ko",
                "target":"en",
                "text":raw_text
            }
            header = {
                'X-Naver-Client-Id':naver_id,
                'X-Naver-Client-Secret':naver_secret
            }
            res = requests.post(papago_url, data=data, headers=header)
            translate_res = res.json()
            translate_result = translate_res.get('message').get('result').get('translatedText')
            result = translate_result
            

        else:
            result = "점심메뉴, 로또, 가위, 바위, 보, 급상승검색어, 환율, 오늘날씨, 영어번역, 사진검색"
    else:
        # 사용자가 보낸 사진을 찾는 과정
        result="picture"
        file_id = data.get('message').get('photo')[-1].get('file_id') # [-1]: 맨 마지막 요소에 접근할 때 사용
        file_url = f"{api_url}/bot{token}/getFile?file_id={file_id}"
        file_res = requests.get(file_url)
        file_path = file_res.json().get('result').get('file_path')
        file = f"{api_url}/file/bot{token}/{file_path}"
        print(file)

        # 사용자가 보낸 사진을 클로버로 전송
        res = requests.get(file, stream=True)
        clova_url = "https://openapi.naver.com/v1/vision/celebrity"
        header = {
                'X-Naver-Client-Id':naver_id,
                'X-Naver-Client-Secret':naver_secret
            }
        clova_res = requests.post(clova_url, headers=header, files={'image':res.raw.read()})
        print(clova_res.text)

        if clova_res.json().get('info').get("faceCount"):
            # 누구랑 닮았는지 출력
            celebrity = clova_res.json().get('faces')[0].get('celebrity')
            name = celebrity.get('value')
            confidence = celebrity.get('confidence')
            result = f"{name}일 확률이 {confidence*100}%입니다."
        else:
            # 사람이 없음
            result = "사람이 없습니다."



    res_url = f"{api_url}/bot{token}/sendMessage?chat_id={user_id}&text={result}"
    requests.get(res_url)

    # res_url = f"{api_url}/bot{token}/sendMessage?chat_id={user_id}&text={user_msg}"
    # requests.get(res_url)

    return '', 200


if __name__ == "__main__":
    app.run(debug=True)
```

> Whitelist에는 네이버 api, 구글, 빗썸 등이 있다. 몇몇 코드들은 python anywhere에서 실행되지 않는다.