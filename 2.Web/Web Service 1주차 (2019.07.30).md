# Web Service 1주차 (2019.07.30)

## CSS

html은 정보의 구조화를 말하고, css는 styling을 정의한다.

html과 css는 각자 문법이 다른 별개의 언어지만 html없는 css는 무의미하다.

html과 css는 에러가 없는 언어지만, 사용자가 원하는 것 처럼 바꿔줄지는 의문이다.



### 기본 사용법

`h1{color: blue; font-size: 15px}`

`h1`: 셀렉터(selector)

`color`: 프로퍼티(property)

`blue`: 값(value)



![1564473487621](assets/1564473487621.png)

#### 1. inline (인라인)

> 이 방법은 지양한다.

```html
<h1 style="color: aqua;">CSS intro</h1>
```

html의 style에 직접 넣어준다. 끝에 `;`을 적어줘야 구분 가능하다. 



#### 2. Embedding (내장)

```html
<head>
    <style>
        h2 {
            color: blue;
        }
    </style>
</head>
<body>
    <h2>CSS is awesome</h2>
</body>
```

`<style>`을 `<head>`안에 사용하면 내부를 css파일처럼 인식해준다.



#### 3. link file (외부 참조)

```css
p {
    color: green;
}
```

```html
<head>
    <link rel="stylesheet" href="00_intro.css">
</head>
<body>
    <p>Lorem ipsum dolor sit amet.</p>
</body>
```

`<head>`안에 링크를 걸어준다.

lorem5 + tab :  의미없는 단어 5개 나열해준다.



## CSS 단위

값에는 키워드, 크기 단위, 색깔 등이 들어갈 수 있다.

![1564473544442](assets/1564473544442.png)

### 1. 키워드

f12키를 누르고, element.style 클릭하면 정보 수정이 가능하다. 하지만 새로고침하면 사라진다.



### 2. 크기 단위

#### 픽셀

![1564473576164](assets/1564473576164.png)

> 고화질은 픽셀이 엄청 많다. 디바이스별로 픽셀의 크기가 제각각이다.

```css
/* #으로 시작하는건 id이다!!! */
#hello {
    font-size: 50px;
}
```

```html
<head>
    <link rel="stylesheet" href="01_css_val.css">
</head>
<body>
    <p id="hello">안녕하세요</p>
</body>
```

특정 `<p>`를 타겟해주기 위해 id가 필요하다. css에서 hello를 쓰면 `<hello>`를 찾기 때문에 #을 붙여줘야한다.



#### %

![1564473591082](assets/1564473591082.png)

> %는 백분율 단위의 상대 단위이다.

```css
#welcome {
    font-size: 50%;
}
```

```html
<p id="welcome">반갑습니다</p>
```

요소에 지정된 사이즈(상속된 사이즈나 디폴트 사이즈)에 상대적인 사이즈를 설정한다. %는 상속가능하다.



![1564473609936](assets/1564473609936.png)

```css
/* div는 부모 */
div {
    width: 50%;
}

/* h1은 자식, 부모의 width를 상속받아서 결국은 50% * 50% = 25% */
h1 {
    width: 50%;
}
```

```html
<div>
    <h1>배고파</h1>
</div>
```



#### em

> 배수단위로 상대단위이다.

![1564473629563](assets/1564473629563.png)

```css
#lunch {
    font-size: 0.5em;
}
```

```html
<p id="lunch">점심시간입니다</p>
```



#### rem

> rem은 root값에 따라 일정하다.

![1564473640984](assets/1564473640984.png)

```css
#snack {
    font-size: 0.5rem;
}
```

```html
<h2>
    <p id="snack">오늘 간식은 도시락</p>
</h2>
```



#### Viewport

> 화면의 비율이 바뀌면 그 상황에 따라 조절된다.

![1564473665372](assets/1564473665372.png)

```css
#menu {
    background-color: red;
    width: 50vw;
    height: 50vh;
}
```

```html
<h1 id="menu">오늘의 메뉴</h1>
```

vw, vh를 지원하지 않는 브라우저도 있다. 'caniuse.com', 'html5test'에서 확인 가능하다.



### 3. 색상

> HEX, RGB, RGBA

HEX와 RGB는 사실상 같다. 구글에 colorpicker검색하면 확인 가능하다.

```css
color {
    color: lightpink;
    color: #ffb6c1;
    color: rgb(255, 182, 193);
}
```



## Box model

![1564473731215](assets/1564473731215.png)

```css
div {
    width: 200px;
    height: 200px;
    background-color: lightpink;
}
```

```html
<div></div>
```



### class

>  class는 중복이 가능하다.

```css
/* '.'은 class */
.margin {
    margin-top: 10px;
    margin-bottom: 10px;
    margin-left: 10px;
    margin-right: 10px;
}

.padding {
    padding-top: 20px;
    padding-bottom: 10px;
}

.border {
    border-width: 2px;
    border-color: blue;
    border-style: dotted;
    border: 3px blue dotted;
}
```

```html
<div class="margin border"></div>
<div class="margin padding"></div>
<div class="margin border"></div>
<div class="margin padding"></div>
```



![1564473799383](assets/1564473799383.png)

```css
/* 모든방향 10 */
.margin-1 {
    margin: 10px;
}
/* 상하 10, 좌우 20 */
.margin-2 {
    margin: 10px 20px;
}
/* 상 10, 좌우 20, 하 30 */
.margin-3 {
    margin: 10px 20px 30px;
}
/* 상우좌하 순으로 */
.margin-4 {
    margin: 10px 20px 30px 40px;
}
```

```html
<div class="margin-1"></div>
<div class="margin-2"></div>
<div class="margin-3"></div>
<div class="margin-4"></div>
```



## display 속성

![1564474179582](assets/1564474179582.png)

![1564474208194](assets/1564474208194.png)



![1564474249441](assets/1564474249441.png)

```css
div {
    background-color: red;
    margin-right: auto;
    margin-left: auto;
}

.half {
    width: 50%;
}
```

```html
<head>
    <link rel="stylesheet" href="03_display.css">
</head>
<body>
    <div>
        <h1>div는 block입니다.</h1>
    </div>
    <div class="half">
        <h1>여기는 절반만!</h1>
    </div>
    <h1>여기는 h1입니다.</h1>
    <div class="half">
        <h1>여기는 절반만!</h1>
    </div>
</body>
```



### 1. block

![1564474179582](assets/1564474179582.png)

항상 새로운 라인에서 시작한다. 화면 크기 전체의 가로폭을 차지한다.

width를 사용하지 않으면 왼쪽 끝부터 오른쪽 끝까지 차지한다.

```css
.b {
    display: block;
}
```

```html
<input class="b" type="text">
<input type="date">
```



### 2. inline

![1564474438564](assets/1564474438564.png)

새로운 라인에서 시작하지 않으며 문장의 중간에 들어갈 수 있고, content의 너비만큼 가로폭을 차지한다.

margin을 사용할 수 없지만, 상하 여백은 line-height로 줄 수 있다.

inline은 붙어서 나온다.

```css
.i {
    display: inline;
    margin-top: 100px;
}
```

```html
<div class="half i">
    <span>test</span>
</div>
<div class="half i">
    <span>test</span>
</div>
```



### 3. inline-block

![1564474489810](assets/1564474489810.png)

inline과는 다르게 margin을 사용할 수 있다.

```css
.bi {
    display: inline-block;
    /* 오류가 나지는 않지만 실행되지 않는다. */
    margin-top: 20px
}
```

```html
<span class="bi">여기는 span</span>
```



### 4. None

데이터를 숨길 때 사용한다. 해당 요소를 화면에 표시하지 않는다.(공간조차 사라진다)

f12키를 누른 후 element.style에서 'display: None'을 작성해보면 화면이 숨겨지는 것을 확인할 수 있다.



### 5. hidden

```html
<h1>안녕하세요.</h1>
<h1>반갑습니다.</h1>
```

f12키를 누른 후 '안녕하세요'를 누른 후 element.style 안에 'visibility: visible'을 해주면 '안녕하세요'가 보인다.

f12키를 누른 후 '반갑습니다.'를 누른 후 element.style 안에 'visibility: hidden'을 해주면 '반갑습니다'가 보이지 않는다. 

None은 배치 자체가 안되는 반면, hidden은 눈에만 보이지 않고, 영역은 차지한다.



### 6. background image

![1564474551516](assets/1564474551516.png)

```css
<h1 class="bg">안녕하세요.</h1>
```

```html
.bg {
    background-image: url(주소);
}
```

주소에는 이미지의 주소를 복사해서 넣으면 된다.



### 7. font

![1564474561004](assets/1564474561004.png)

```css
.text {
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    font-family: 'Merriweather', serif;
    font-size: 5rem;
}
```

```html
<h1 class="text">반갑습니다.</h1>
```

'font.google.com'에서 +버튼 누르면 나오는 `<link>`를 `<head>`의 `<title>`아래에 복사해서 붙여준다.

css에는 'Specify in CSS'를 넣어주면 된다.



## Position

![1564474733805](assets/1564474733805.png)

```css
div {
    height: 100px;
    width: 100px;
}
```

```html
<head>
    <link rel="stylesheet" href="04_position.css">
</head>
<body>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
</body>
```



### 1. static (기본위치)

```css
.blue {
    background-color: blue;
    position: static;
}
```

```html
<div class="blue"></div>
```





### 2. relative (상대위치)

자신의 위치를 기준으로 변경한다. 자신의 위의 position이 바뀐 경우에 바뀔 수 있다.

```css
.red {
    background-color: red;
    position: relative;
    /* static기준으로 자신의 원래 위치에서 왼쪽과 아래에 공백을 준다. */
    left: 50px;
    bottom: 50px;
}
```

```html
<div class="red"></div>
```





### 3. absolute (절대위치)

static을 제외한 position을 찾아 그 위치를 기준으로 한다.

```css
.green {
    background-color: green;
    /* 빈 공간이 있음에도 원본이 있으므로 아래에 생성 */
    /* static 제외 위의 position 기준으로 생성 */
    position: absolute;
    right: 0px;
    bottom: 0px;
}
```

```html
<div class="green"></div>
```



### 4. 상속위치

```css
.parent {
    height: 200px;
    width: 100%;
    background-color: gray;
    position: relative;
}

.children {
    /* div가 가지고있는 크기를 가져옴 */
    background-color: pink;
    position: absolute;
    /* 왼쪽과 아래에 공백 */
    left: 100px;
    bottom: 100px;
}
```

```html
<div class="parent">
    <div class="children"></div>
</div>
```

부모 위치를 'relative'로 작성해야 자식의 위치가 부모 위치 안에 위치할 수 있다.



### 5. fixed (고정위치)

스크롤을 내려도 위치가 고정되어있다.

```css
.yellow {
    background-color: yellow;
    /* 스크롤을 내려도 따라온다. */
    position: fixed;
    right: 0px;
    top: 0px;
}
```

```html
<div class="yellow"></div>
```

Youtube등의 사이트에서 스크롤바를 내려도 움직이지 않는 부분처럼 만드는 것에 사용할 수 있지만 추천하지 않는다.



### Position 실습

![1564475829418](assets/1564475829418.png)

```css
.big-box {
    position: relative;
    /* 위치를 지정해주어야 상속받는 박스들의 위치를 정할 수 있다. */
    margin: 100px auto 500px;
    border: 5px solid black;
    width: 500px;
    height: 500px;
  }
  
  .small-box {
    width: 100px;
    height: 100px;
  }
  
  #red {
    background-color: red;
    /* 큰 사각형 내부의 우측 하단 모서리에 빨간 사각형 위치시키기 */
    position: absolute;
    right: 0px;
    bottom: 0px;
  }
  
  #gold {
    background-color: gold;
    /* 브라우저의 하단에서 50px, 우측에서 50px 위치에 고정하기 */
    position: fixed;
    right: 50px;
    bottom: 50px;
  }
  
  #green {
    background-color: green;
    /* absolute 이용해서 큰 사각형의 가운데 위치시키기 */
    position: absolute;
    left: 200px;
    top: 200px;
  }
  
  #blue {
    background-color: blue;
    /* relative를 사용해서 큰 사각형 좌측 상단 모서리에서 100px, 100px 띄우기 */
    position: relative;
    left: 100px;
    top: 100px;
  }
  
  #pink {
    background-color: pink;
    /* 큰 사각형 내부의 좌측 상단 모서리로 옮기기*/
    /* 첫번째 방법1 */
    position: relative;
    bottom: 100px;
    /* 두번째 방법2 */
    position: absolute;
    left: 0px;
    top: 0px;
  }
```

```html
<head>
  <link rel="stylesheet" href="05_box.css">
</head>
<body>
  <div class="big-box">
    <div class="small-box" id="red"></div>
    <div class="small-box" id="gold"></div>
    <div class="small-box" id="green"></div>
    <div class="small-box" id="blue"></div>
    <div class="small-box" id="pink"></div>
  </div>
</body>
```



## Subway

![1564473194996](assets/1564473194996.png)

```html
<h1>서브웨이 주문하기</h1>
<p>주문서를 작성해주세요.</p>

<form action="">
    <div>
        <label for="name">이름:</label>
        <!-- placeholder는 사용자가 넣어야 하는 데이터에 대한 설명을 적어준다. -->
        <input id="name" type="text" name="name" placeholder="이름을 입력해주세요">
    </div>
    <div>
        <label for="when">날짜:</label>
        <input id="when" type="date" name="when">
    </div>

    <h3>1. 샌드위치 선택</h3>
    <!-- <label for="">메인메뉴</label> -->
    <!-- radio에서 라벨은 좋지 않은 선택 -->
    <input id="option1" type="radio" name="main" value="1"><label for="option1">에그 마요</label>
    <input id="option2" type="radio" name="main" value="2"><label for="option2">이탈리안 비엠티</label>
    <input id="option3" type="radio" name="main" value="3"><label for="option3">터키 베이컨 아보카도</label>

    <h3>2. 사이즈 선택</h3>
    <!-- <select name="" id="">
<option value="">15cm</option>
<option value="">30cm</option>
</select> -->
    <input name="size" type="number" value="15" min="15" max="30" step="15">

    <h3>3. 빵</h3>
    <!-- select는 radio처럼 하나만 선택 가능하다. -->
    <select name="bread" id="">
        <!-- url에는 value값만 찍힌다. -->
        <option value="1">허니오트</option>
        <option value="2" disabled="">플랫브래드</option>
        <option value="3">하티 이탈리안</option>
    </select>

    <h3>4. 야채/소스</h3>
    <input id="tomato" type="checkbox" name="tomato" value="True"><label for="tomato">토마토</label>
    <input id="cucumber" type="checkbox" name="cucumber" value="True"><label for="cucumber">오이</label>
    <input id="hal" type="checkbox" name="hal" value="True"><label for="hal">할라피뇨</label>
    <input id="hot" type="checkbox" name="hot" value="True"><label for="hot">핫 칠리</label>
    <input id="bar" type="checkbox" name="bar" value="True"><label for="bar">바베큐</label>

    <br><br>
    <input type="submit">
</form>
```

