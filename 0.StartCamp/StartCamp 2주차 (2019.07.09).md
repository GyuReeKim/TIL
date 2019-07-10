# StartCamp 2주차  (2019.07.09)

## Python 3.7.3과 Git Bash와 Visual Studio Code 설치

PATH : 환경변수 #설치 시 체크

Git, Unix # 리눅스 기반으로 쓸것이라는 뜻으로 설치 시 체크

Git Bash : 명령 프롬프트같은 것

## CLI

1. 유닉스 shell (sh, zsh, bash 등)
2. CP/M
3. DOS의 command.com
4. etc

## bash 명령어

ls : 현재 디렉토리 내용들을 나열 (파란색 : 폴더, 하얀색 : 파일)

pwd(present working directory) : 현재 작업하는 디렉토리 표시

cd(change directory) : 현재 작업하는 디렉토리 변경

cd tab키 : 폴더 검색/자동완성

cd .. : ..(상위)폴더로 들어가라

cd ~ : ~(홈)위치로 바로가기

mkdir(make directory) : 새로운 디렉토리 생성

touch : 파일 만드는 명령어

echo : 문자열 출력

rm(remove) : 파일 지우기

rm -r(recursive 재귀) : 폴더 삭제

exit : 터미널 종료

code . : vs code 실행

ctrl+shift+p : 검색

ctrl+l : 터미널 지우기

F11 : 전체화면

mv 파일.py .. : 파일 이동

mv 파일.py 파일.py : 파일 이름 변경

## 정보 스크랩하기

### webbrowser.open(주소)

웹 브라우저를 열어준다.

#### browser.py

```python
import webbrowser

url = "https://search.naver.com/search.naver?sm=top_hty&fbm=0&ie=utf8&query="
my_keywords = ["iu", "twice"]
for my_keyword in my_keywords:
    webbrowser.open(url + my_keyword)
```

### requests.get(주소)

주소에 요청보내서 정보를 받는다.

```
pip install requests
```

### from bs4 import BeautifulSoup

bs4 안에서 BeautifulSoup 꺼낸다.

```
pip install bs4
```

### .select(selector)

문서 안에 있는 특정 내용을 뽑는다. 여러개의 데이터를 가져온다.

### .select_one(selector)

문서 안에 있는 특정 내용 하나만 뽑는다. 하나의 데이터만 가져온다. 대괄호가 사라진다.

### .text

여는 태그 닫는 태그 사이의 문자

#### kospi.py

```python
import requests
from bs4 import BeautifulSoup

response = requests.get("https://finance.naver.com/sise/").text
soup = BeautifulSoup(response, 'html.parser')
kospi = soup.select_one('#KOSPI_now') # #KOSPI_now는 id
print(kospi.text)
```

#### naver.py

```python
import requests
from bs4 import BeautifulSoup

response = requests.get('https://naver.com').text
soup = BeautifulSoup(response, 'html.parser')
naver = soup.select_one('#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_list.PM_CL_realtimeKeyword_list_base > ul:nth-child(5) > li:nth-child(1) > a.ah_a > span.ah_k') # 복사된 정보는 'Copy Selector'
print(naver.text)
```

가져오고자 하는 정보의 인터넷 창에서 마우스 우클릭 후 '검사' 클릭.

옆에 뜨는 창에서 원하는 정보 위에 우클릭 후 'Copy' 클릭 후 'Copy Selector' 클릭.

#### exchange.py

```python
import requests
from bs4 import BeautifulSoup

response = requests.get('https://finance.naver.com/marketindex/').text
soup = BeautifulSoup(response, 'html.parser')
exchange = soup.select_one('#exchangeList > li:nth-child(1) > a.head.usd > div > span.value')
print(exchange.text)
```

#### exchange_all.py

```python
import requests
from bs4 import BeautifulSoup
url = 'https://finance.naver.com/marketindex/exchangeList.nhn'
res = requests.get(url).text
soup = BeautifulSoup(res, 'html.parser')

# tbody = soup.select_one('tbody')
# tr = tbody.select('tr')

tr = soup.select('tbody > tr') # 'tbody > tr' : tbody 안에 있는 tr을 가져옴

for r in tr:
    print(r.select_one('.tit').text.strip())
    print(r.select_one('.sale').text)
```

### faker()

이름 랜덤 생성

Faker('ko_KR') : 한글이름 생성

#### files.py

파일들이 student 파일 안에 생성되야 한다.

```python
from faker import Faker
import os

f = Faker('ko_KR')

for i in range(100):
    filename = f'{i}_{f.name()}.txt'
    cmd = f'touch {filename}'
    os.system(cmd)
```

### import os

1. os.chdir(r'폴더주소') : 작업위치 변경
2. os.listdir('폴더주소')
3. os.rename('파일이름', '추가정보'+'파일이름')

#### 파일에 SAMSUNG을 추가하기

```python
import os

os.chdir(r'C:\Users\student\startcamp\students')
for filename in os.listdir('.'): # os.listdir('.')에서 '.'은 현재주소
    os.rename(filename, 'SAMSUNG_' + filename)
```

#### SAMSUNG을 SSAFY로 바꾸기

```python
import os

os.chdir(r'C:\Users\student\startcamp\students')
for filename in os.listdir('.'):  # os.listdir('.')에서 '.'은 현재주소
    os.rename(filename, filename.replace('SAMSUNG_', 'SSAFY_'))
```

## git

버전 관리 시스템

공동작업을 위한 것

- GitHub : 포트폴리오 만드는 장소
- GitLab : 과제 제출 장소
- Gitbucket

### git의 작업 흐름

1. 작업(수정)한 파일 (Working directory)

- add : 커밋할 목록에 추가

2. 커밋할 목록 (Index)

- commit : 커밋(creat a snapshot) 만들기

3. 쌓인 커밋들 (Head)

- push : 현재까지의 역사 (commits) 가 기록되어 있는 곳에 새로 생성한 커밋들 반영하기

4. GitHub

### GitHub 계정만들기

http://bit.do/gjpy2에 들어가서 작성된 이메일로 만들기

### git 저장방법

```
git init #(master)가 붙음. 이 폴더를 git으로 관리함
git add .
git commit -m "day1추가 | 190708"
git remote add origin https://github.com/GyuReeKim/TIL.git
git push origin master
```

### gitignore

인터넷 주소창에 gitignore.io 작성

Python, Windows, VisualStudioCode 클릭 후 엔터

```
cd startcamp/
ls
git init
ls -a # 숨겨진 폴더 확인
touch .gitignore
ls -a # .gitignore 폴더 확인
git add . #임시저장
git commit -m "first commit" #이메일 주소가 없어서 오류 발생
git config --global user.email "이메일주소"
git config --global user.email
git config --global user.name "이름"
git config --global user.name
git add .
git commit -m "first commit"
```

GitHub에서 제일 긴 주소 복사

```
git remote add origin 복사된 주소
git push origin master
```

### README.md 추가

깃허브에는 README.md를 작성해야 한다.

VisualStudioCode에서 README.md 생성

```
# StartCamp 정리 자료
- 하루하루 열심히 정리하자!!!
```

```
git add .
git commit -m "add README.md" # 파일 추가할 때마다 add 사용
git push origin master
```



### 협업

#### A

```
mkdir end-to-end
cd end-to-end
touch text.md
```

탐색기를 열어 파일찾기. Typora로 끝말잇기 작성

```
git init #(master)
git add . #임시저장
git commit -m "끝말잇기 시작!"
```

깃허브에 들어가서 New Repository 선택. end-to-end / 끝말잇기 하면서 깃을 배워봅니다.

붙여넣기(paste) 합니다.

```
git push origin master
```

새로고침을 누른 후, 우측의 'Settings' 클릭 후 'Collaborators'를 클릭합니다.

#### B

메일로 'View invitation'을 클릭합니다. 우측의 초록색 버튼 'Clone or download'를 클릭하고 주소를 복사합니다.

```
cd ..
git clone 주소복사
cd end-to-end
git add.
git commit -m "두 번째 차례 끝"
git push origin master
```

#### A

```
git pull origin master
```

### 충돌

A와 B가 동시에 문서를 수정합니다.

#### A

```
git add .
git commit -m "승자 정함"
git push origin master
```

#### B

```
git add .
git commit -m "승자 결정"
git push origin master
```

충돌이 발생합니다.

```
git pull origin master #수행하면 (master|MERGING)이 생긴다.
```

파일을 열어 내용을 수정한다.

```
git add . #(master|MERGING)
git commit -m "merging" #(master|MERGING)
git push origin master #(master)
```

### GitHub 저장 특징

python 문서 작성할 때 마다 'add' 해줘야 한다.

폴더를 저장하면 내용들이 덮어씌어진다.

### 참고사항

구글에 TIL(Today I learned) 검색 해서 참고하면 좋다.

매일매일 저장해서 잔디밭을 만들기