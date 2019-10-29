# JavaScript (2019.10.29)

## node JS

> 기존의 JS는 컴퓨터를 조작할 수 없었지만 node JS를 만들면서 가능해졌다.

 `https://nodejs.org/ko/ `에서 안정성, 신뢰도 높음을 설치한다.



## JavaScript Intro

### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <h1>index</h1>
  <!-- JavaScript가 가장 무겁고 로딩이 느리기 때문에 가장 마지막에 넣어준다. -->
  <script src="./main.js"></script>
</body>
</html>
```

javascript 잘 불러왔는지 확인하려면 개발자도구에서 Sources에 main.js가 있는지 확인한다.

만약 제대로 불러오지 않았다면 Console창에서 오류가 발생한다.

JavaScript가 가장 무겁고 로딩이 느리기 때문에 가장 마지막에 넣어준다.

` http://stevesouders.com/examples/move-scripts.php ` 사이트를 참고해보면 알 수 있다.



### 개발자도구 console창

```javascript
document

window.document

document.querySelector('h1')

document.querySelector('h1').innerText
"hello world"

document.querySelector('h1').innerText = "bye"
"bye"

document.querySelector('h1').innerText
"bye"

const user = "1"

user
"1"

const num = 1

num
1

user == num
true

user === num
false
```



### javascript의 비교

> javascript triangle을 검색해보자.

```
0 == []
true
0 == "0"
true
"0" == []
false
```

` https://eqeq.js.org/ `사이트를 참고하면 어떤 값이 같고 다른지 알 수 있다.



### main.js

```javascript
alert("hello world!!!") // 팝업창을 표시해준다.

// 여기는 주석입니다.
/*
여기서부터
여기까지는
주석입니다.
*/

document.write('<h1>hello world</h1>') // 문서에 작성하라는 명령어

// h1태그를 가져와!
document.querySelector('h1')

// 가져온 태그 내부 text를 바꿔줘
document.querySelector('h1').innerText = "bye"

// 변수 사용
var name = "change"
console.log(name)

// var은 잘 사용하지 않을 예정.
// 하지만 다른사람의 코드를 읽기 위해서는 알아둬야 한다.
var b = 30
// (var 시작값 ; 특정 조건 ; 증가)
for (var b = 0 ; b < 10 ; b++) {
    console.log(b)
}
console.log('!!!!!!!!!!!!!!!')
console.log(b)

// 주로 사용하게 될 코드는 let과 const
// 재할당 가능
let name = 'change' // let은 데이터를 변경 가능하게 해준다.
document.write(name)

name = "changhee"
document.write(name)

// 재할당 불가능
const loca = 'GJ' // const는 수정이 불가능하다.
document.write(loca)

// 오류 발생
loca = 'Seoul'
document.write(loca)

const first_name = "change"
const last_name = "oh"

const full_name = last_name + first_name
document.write('<h1>'+full_name+'</h1>')
document.write(`<h1>${full_name}</h1>`) // `${}`는 문자열 사이에 데이터를 넣을 수 있도록 해준다.
console.log(`<h1>${full_name}</h1>`)

const userName = prompt("hello who are you???") // 제출기능이 있는 팝업창을 띄워준다.
let message = `<h1>hello! ${userName}</h1>`
document.write(message) // 사용자가 제출한 문자열을 넣은 message를 출력해준다.

if (userName === 'change') {
  message = `<h1>admin page</h1>`
} else if (userName === 'happy') {
  message = `<h1>happy coding</h1>`
} else{
  message = `<h1>hello! ${userName}</h1>`
}
document.write(message)

const num1 = 0
const num2 = "0"

// 느슨한 같음(값을 비교)
console.log(num1==num2)
// 엄격한 같음(타입을 비교)
console.log(num1===num2)
```



## JS 2 space로 변경

extension에서 ESLint와 Beautify 설치한다.



## 00_variable.js

### 변수 선언

```javascript
// let 키워드는 같은 이름의 변수를 한번만 선언가능 (할당은 여러번 가능하다.)
let x = 1
// let x = 2
x = 3
console.log(x)
```

```bash
$ node 00_variable.js
let x = 2
    ^

SyntaxError: Identifier 'x' has already been declared
```

let은 같은 이름의 변수를 설정할 수 없다.



### 함수 (var, let, const 구분이  중요하다!)

```javascript
// block scope 블록 유효범위
let x = 1

if (x === 1) {
    let x = 2 // 함수 안에서는 변수가 따로 저장
    console.log(x)
}
console.log(x)
```

```bash
$ node 00_variable.js
2
1
```



```javascript
let x
const y = 9 // const를 쓸 때는 변수에 데이터를 넣어줘야한다.
// y = 10 // const는 재할당이 불가능하다. const는 변경 가능하지 않은 값이다.

if (y === 9) {
    let y = 20
    console.log(y)
}
console.log(y)
```

```bash
$ node 00_variable.js
20
9
```



```javascript
// def varTest():
// true는 소문자로 작성한다.
function varTest() {
    // block scope는 안에서 밖을 참조 가능하다.
    var x = 1
    if (true) {
        var x = 2
        console.log(x) // 2를 참조
    }
    console.log(x) // 2를 참조
}
varTest()
console.log(x) // 2를 참조하지 않는다.
```

```bash
student@DESKTOP MINGW64 ~/Javascript/Intro
$ node 00_variable.js
2
2
```



var : 선언, 할당 => 자유 / 함수 스코프 // 자유도가 높으면 오류 발생 확률이 커진다.

let : 할당 => 자유 / 선언 => 한번만 / 블록 스코프

const : 할당, 선언 => 한번만 / 블록스코프



### 변수 작성

```javascript
// Camel case, Snake case, Pascal case, Kebab case를 주로 사용한다.
let dog
let variableName // Camel case 사용

let dogs = [] // 배열

function getName() {

}

// event를 작성할 때 on을 붙여주는 것이 일반적이다.
const onClick = () => {}

// true/false에 대한 값을 가질 경우 is를 붙여준다.
let isValid = false

// class는 Pascal case로 작성
class User {
    constructor(value) {
        this.name = value.name
    }
}

// 모두 대문자로 구성되어있으며 _로 연결되어 있다면, 절대 바뀌지 않는 완전한 상수를 말한다.
const API_KEY = "asdff:123980124"
```



### const 숫자 표현

```javascript
const a = 13
const b = -5
const c = 3.14
const d = 2.9e8
const e = Infinity
const f = -Infinity
const g = NaN

console.log(e)
console.log(typeof e)
console.log(f)
console.log(typeof f)
console.log(g)
console.log(typeof g)
console.log(Math.sqrt(-2)) // NaN (Not a Number) // 허수는 NaN가 출력된다.
```

```bash
$ node 00_variable.js
Infinity
number
-Infinity
number
NaN
number
NaN
```



### 문자열 작성 방법

```javascript
const sentence1 = 'hello\n'
const sentence2 = "hello"
const sentence3 = `helloword${sentence2}` // `는 내부의 변수를 쓸 경우 아니면 사용할 일이 거의 없다.
console.log(sentence3+sentence1)

const isValid = true // false
```

```bash
$ node 00_variable.js
hellowordhellohello

undefined
```



### Null & Undefined

```javascript
let first_name
console.log(typeof first_name) // undefined: 비어있음

let last_name = null
console.log(typeof last_name) // null: 비어있음

// console.log(null == undefined) // 비어있다는 의미는 같으므로 true값이 나온다.
console.log(null === undefined) // 완벽한 비교를 위해서는 ===을 사용해야 한다.

console.log(null + 1)
console.log(undefined + 1)
```

```bash
$ node 00_variable.js
object
false
1
NaN
```



## 01_operator.js

> 연산은 기본적으로 앞에서부터 계산한다.

### 사칙연산

```javascript
let a = 0
// + - * / 모두 가능하다.
a = a + 3
console.log(a)
a += 3
console.log(a)
a++
console.log(a)
```

```
$ node 01_operator.js
3
6
7
```



### 비교

```javascript
console.log(2>3)
console.log('가' < '나')
console.log('A' < 'a')
```

````
$ node 01_operator.js
false
true
true
````



### 비교 연산

````javascript
const a = 1
const b = '1'
console.log(a == b)
console.log(a === b)
console.log(a === Number(b))
````

```
$ node 01_operator.js
true
false
true
```



### 논리 연산

```javascript
// and
console.log(true && false)
// or
console.log(false || false)
// not
console.log(!false)
```

```
$ node 01_operator.js
false
false
true
```



### 삼항연산자

```javascript
// 참일때 실행 if 조건 else 거짓일때 실행
// print("hello") if a > 3 else print("bye")
const result1 = true ? 1 : 2
console.log(result1)
const result2 = false ? 1 : 2
console.log(result2)
const a = 10
const result3 = a > 5 ? "5이상" : "5미만"
console.log(result3)
```

```bash
$ node 01_operator.js
1
2
5이상
```



## 02_control_of_flow.js

### 외부 라이브러리 불러오기

```javascript
const readline = require('readline') // 외부 라이브러리 불러오기
const userName = readline.createInterface({
        input: process.stdin,
        output: process.stdout
})

userName.question('이름을 입력하세요', (answer) => {
    console.log(answer)
    userName.close()
})
```

```bash
$ node 02_control_of_flow.js
이름을 입력하세요mandarine
mandarine
```



### if elif else문

```javascript
// if elif else문
const userName = prompt('who are you?')

let message = ''

if (userName === "change") {
    message = "hi admin"
} else if (userName === "happy") {
    message = "happy coding"
} else {
    message = `hello ${userName}`
}
console.log(message)
```



### switch문

```javascript
// switch문
const userName = prompt('who are you?')

let message = ''

// switch 문은 특정 case에서만 작동한다.
switch(userName) {
    case 'admin': {
        message = 'hi_admin'
        break // default를 실행하지 않고 바로 탈출
    }
    case 'change': {
        message = 'welcome'
        break
    }
    // 모든 case를 돌고 마지막으로 실행해야하는 코드
    default: {
        message = `hi ${userName}`
    }
}
console.log(message)
```



### while문

```javascript
// while문
let i = 0
while (i < 5) {
    console.log(i)
    i++
}
```

```bash
$ node 02_control_of_flow.js
0
1
2
3
4
```



### for문

```javascript
// for문
/*
for ( 초기화 ; 값 비교 ; 값 증가) {
    실행시킬 코드
}
*/
for (let a = 0 ; a < 5 ; a++) {
    console.log(a)
}
```

```bash
$ node 02_control_of_flow.js
0
1
2
3
4
```



### let of

```javascript
// let of
const numbers = [0, 1, 2, 3, 4, 5]
for (let num of numbers) {
    console.log(num)
}
```

```bash
$ node 02_control_of_flow.js
0
1
2
3
4
5
```



## 03

### 선언식 & 표현식

```javascript
// 선언식
function add(num1, num2) {
    return num1 + num2
}
console.log(add(2, 10))

// 표현식
// 함수를 변수에 넣을 수 있음을 보여준다.
const sub = function(num1, num2) {
    return num1 - num2
}
console.log(sub(10, 2))
```

````bash
$ node 03_function.js
12
8
````



### 함수 표현식

```javascript
// 함수 표현식
const ssafy1 = function(name) {
    console.log(`hello ${name}`)
}
ssafy1('change')
```

```bash
$ node 03_function.js
hello change
```



### 화살표 함수

```javascript
// 화살표 함수, arrow function
const ssafy2 = (name) => {
    console.log(`hello ${name}`)
}
ssafy2('changhee')

// 화살표 함수 소괄호 생략, 단 매개변수가 하나일 때
const ssafy3 = name => {
    console.log(`hello ${name}`)
}
ssafy2('mandarine')

// 화살표 함수 중괄호 생략, 표현식이 하나일 때
const ssafy4 = name => `hello ${name}`
console.log(ssafy4('kim'))
```

```bash
$ node 03_function.js
hello changhee
hello mandarine
hello kim
```



### 함수 변환 연습

```
// 함수 표현식
let square = function(num) {
    return num ** 2
}

// 화살표 함수
square = (num) => {
    return num ** 2
}

square = num => {
    return num ** 2
}

square = num => num ** 2
```



### noArgs

```
let noArgs = _ => 'no args' // _는 인자 없음을 말한다.
let noArgs = () => 'no args'
console.log(noArgs())
```

```

```



### dictionary

```
// dictionay
const a = {}
console.log(typeof a)

let returnObject = () => {
    return {key: 'value'}
}

let returnObject = () => ({key: 'value'}) // return과 중괄호 생략
```

```

```



### 사용자가 데이터를 넣지 않았을 경우

```
// 사용자가 데이터를 넣지 않았을 때 undefined 출력
const hello = (name) => `hello ${name}`
console.log(hello('change'))
console.log(hello())

// 사용자가 데이터를 넣지 않았을 경우 기본값 설정
const hello = (name="noName") => `hello ${name}`
console.log(hello('change'))
console.log(hello())
```

```
$ node 03_function.js
hello change
hello undefined

$ node 03_function.js
hello change
hello noName
```



### 기명함수 & 익명함수

```
// 기명함수
const hello = function (name) {
    console.log(name)
}
hello("change")

// 익명함수: 일회용으로 쓰는 데이터나 callback에서 사용한다. 잠깐동안 사용하는 데이터에 사용한다.
(function (name) {
    console.log(name)

})("change")
```

```
$ node 03_function.js
change

$ node 03_function.js
change
```



## 04_array.js

### 

```
const numbers = [1, 2, 3, 4]

console.log(numbers[0])
console.log(numbers[-1]) // 음수로는 접근이 불가능하다
console.log(numbers.length)

console.log(numbers.reverse()) // reverse는 원본을 변경한다.
console.log(numbers)

numbers.push(5) // 배열의 맨 끝에 값이 추가된다.
console.log(numbers)

numbers.pop() // 배열의 맨 끝의 값이 삭제된다.
console.log(numbers)

numbers.unshift(0) // 배열의 맨 앞에 값이 추가된다.
console.log(numbers)

numbers.shift()
console.log(numbers) // 배열의 맨 앞의 값이 삭제된다.

console.log(numbers.includes(100)) // 100값을 포함하지 않으면 false 
console.log(numbers.indexOf(100)) // 100값의 인덱스가 없으면 -1
console.log(numbers.join()) // 문자열을 하나로 합쳐준다.
console.log(numbers)
```



## 05

### `.` 접근

```
const me = {
    name: 'change',
    'phone number': '123123123',
    product: {
        phone: 'iphone',
        notebook: 'mac',
    }
}
console.log(me.name) // javascript에서는 .으로 접근한다.
console.log(me['name']) // 가능은 하지만 잘 사용하지 않는다.
console.log(me.product.notebook)
```



### 리스트 접근

```
let books = ['javascript', 'python']
let comics = {
    DC: ['Aquaman', 'SuperMan'],
    Marvle: ['Avengers', 'IronMan'],
}

let bookStore = {
    books, // books: books, 와 같다.
    comics //comics: comics 와 같다.
}
console.log(bookStore)
```



### JSON

```
// JSON: 항상 object 형태로 바꿔줘야한다.
const me = {
    name: 'change',
    'phone number': '123123123',
    product: {
        phone: 'iphone',
        notebook: 'mac',
    }
}
console.log(me)
const jsonData = JSON.stringify(me)
console.log(jsonData)

const parseData = JSON.parse(jsonData)
console.log(parseData)
```

