# JavaScript (2019.10.30)

## 06_array_method.js

### javascript array method

> ` https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array `



### for each

> array가 직접 들고있는 값을 가져온다.

```javascript
let colors = ['red', 'green', 'blue']

for (let color of colors) {
    console.log(color)
}

// callback 함수 
colors.forEach(function(color){
    console.log(color)
})

// currentValue, index, array 출력
colors.forEach(function(color, idx, array){
    console.log(color, idx, array)
})
```

```bash
$ node 06_array_method.js
red
green
blue
red
green
blue
red 0 [ 'red', 'green', 'blue' ]
green 1 [ 'red', 'green', 'blue' ]
blue 2 [ 'red', 'green', 'blue' ]
```



````javascript
function handlePosts(){
    const posts = [
        {id: 50, title: "javascript"},
        {id: 100, title: "python"},
        {id: 123, title: "css"}
    ]
    // for문
    for (let i = 0 ; i < posts.length ; i++) {
        console.log(posts[i])
        console.log(posts[i].id)
        console.log(posts[i].title)
    }
    // forEach문
    posts.forEach(function(post){
        console.log(post)
        console.log(post.id)
        console.log(post.title)
    })
    // forEach문 array함수
    // 소괄호 생략 가능
    posts.forEach((post) => {
        console.log(post)
        console.log(post.id)
        console.log(post.title)
    })
}
handlePosts()
````

```bash
$ node 06_array_method.js
{ id: 50, title: 'javascript' }
50
javascript
{ id: 100, title: 'python' }
100
python
{ id: 123, title: 'css' }
123
css
```



```javascript
// 예제 [200, 350, 750]
const images = [
    {height: 10, width: 20},
    {height: 14, width: 25},
    {height: 50, width: 15}
]
const areas = []

// forEach문
images.forEach(function(image){
    areas.push(image.height*image.width)
})

// forEach문 array함수
images.forEach(image=>areas.push(image.height*image.width))

console.log(areas)
```

```bash
$ node 06_array_method.js
[ 200, 350, 750 ]
```



### map

```javascript
// map연산은 반환값이 존재한다.
const numbers = [1,2,3,4,5]

// forEach문을 map문으로 바꿔보기
const doubleNumbers = []
numbers.forEach(function(number){
    doubleNumbers.push(number*2)
})
console.log(doubleNumbers)

// map문
const double = numbers.map(function(number){
    return number*2
})
console.log(double)

// map문 간략화하기
const double = numbers.map(number => number*2)
console.log(double)
```

```bash
$ node 06_array_method.js
[ 2, 4, 6, 8, 10 ]
```



```javascript
// map 예제 [200, 350, 750]
const images = [
    {height: 10, width: 20},
    {height: 14, width: 25},
    {height: 50, width: 15}
]

// map문
const areas = images.map(function(image){
    return image.height*image.width
})
console.log(areas)

// map문 간략화
const areas = images.map(image => image.height*image.width)
console.log(areas)
```

```bash
$ node 06_array_method.js
[ 200, 350, 750 ]
```



### filter

```javascript
// filter 함수 사용
const numbers = [1,2,3,4,5]
const evenNumber = numbers.filter(function(number){
    return number % 2 === 0
})
console.log(evenNumber)
```

```bash
$ node 06_array_method.js
[ 2, 4 ]
```



````javascript
const products = [
    {name: 'cucumber', type: 'vegetable'},
    {name: 'banana', type: 'fruit'},
    {name: 'carrot', type: 'vegetable'},
    {name: 'apple', type: 'fruit'}
]

// filter는 object 자체를 분류하기 때문에 name만 따로 쓰려면 forEach나 map을 사용해야한다.
const fruit = products.filter(function(product){
    return product.type === 'fruit'
})
console.log(fruit)
````

```bash
$ node 06_array_method.js
[ { name: 'banana', type: 'fruit' }, { name: 'apple', type: 'fruit' } ]
```



### reduce

```javascript
// reduce 함수 사용
const scores = [100, 80, 88, 92, 95, 70]

// reduce 함수의 특성때문에 초기값을 적지 않는다면 배열의 첫번째 요소를 사용한다.
// reduce문
const total = scores.reduce((total, score)=>{
    return total += score
}, 0)
console.log(total)

// reduce문 간략화
const total = scores.reduce((total, score) => total += score, 0)
console.log(total)
```

```bash
$ node 06_array_method.js
525
```



### find

```
// find 함수 사용
// find는 조건에 해당하는 처음으로 만난 값을 반환하고 함수를 끝낸다.
const users = [
    {name: 'change', location: 'gj'},
    {name: 'justin', location: 'gm'},
    {name: 'tak', location: 'dj'},
    {name: 'junho', location: 'dj'},
    {name: 'neo', location: 'so'}
]

const user = users.find(function(user){
    return user.name === 'neo'
})
console.log(user)
```

```
$ node 06_array_method.js
{ name: 'neo', location: 'so' }
```



```
const users = [
    {name: 'change', location: 'gj'},
    {name: 'justin', location: 'gm'},
    {name: 'tak', location: 'dj'},
    {name: 'junho', location: 'dj'},
    {name: 'neo', location: 'so'}
]

const user = users.find(function(user){
    return user.location === 'dj'
})
console.log(user)
```

```
$ node 06_array_method.js
{ name: 'tak', location: 'dj' }
```



## 07_event.html

> ` https://developer.mozilla.org/ko/docs/Web/Events `

### hello를 출력하기

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
  <div id="my"></div>
  <button id="this-button">click</button>
  <script>
    // 1. 무엇을 => id가 this-button 버튼을
    const btn = document.querySelector('#this-button')
    // console.log(btn)

    // 2. 언제 => 클릭하면
    btn.addEventListener('click', function(event){
      console.log(event)
      // 3. 어떻게 => id가 my인 div에 hello를 넣기
      const div = document.querySelector('#my')
      div.innerHTML = '<h1>hello!!!</h1>'
    })

    // 2-1. 언제 => 클릭하고 키보드를 누르면 KeyboardEvent  qkftod
    btn.addEventListener('keydown', function(event){
      console.log(event)
      // 3. 어떻게 => id가 my인 div에 hello를 넣기
      const div = document.querySelector('#my')
      div.innerHTML = '<h1>hello!!!</h1>'
    })
  </script>
</body>
</html>
```



## 08_dino.html

> ` https://gist.github.com/eduChange-hphk/cd53902df37b01ae171d5218da0b68af `

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
    <style>
      .bg {
        background-color: #f7f7f7;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
      }
    </style>
  </head>
  <body>
    <div class="bg">
      <img
        id="dino"
        width="100px"
        height="100px"
        src="https://is4-ssl.mzstatic.com/image/thumb/Purple118/v4/88/e5/36/88e536d4-8a08-7c3b-ad29-c4e5dabc9f45/AppIcon-1x_U007emarketing-sRGB-85-220-0-6.png/246x0w.jpg"
        alt="dino"
      />
    </div>

    <script>
    </script>
  </body>
</html>
```

```
window

window.document

document

document.querySelector('.bg')

const bg = document.querySelector('.bg')
undefined

bg

const dino = bg.querySelector('#dino')
undefined

dino

document.querySelector('.bg #dino')
```

````
const bg = document.querySelector('.bg')
undefined

bg

bg.firstElementChild

bg.firstElementChild.remove()
undefined
````



### dino 움직이기

````
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
    <style>
      .bg {
        background-color: #f7f7f7;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 95vh;
      }
    </style>
  </head>
  <body>
    <div class="bg">
      <img
        id="dino"
        width="100px"
        height="100px"
        src="https://is4-ssl.mzstatic.com/image/thumb/Purple118/v4/88/e5/36/88e536d4-8a08-7c3b-ad29-c4e5dabc9f45/AppIcon-1x_U007emarketing-sRGB-85-220-0-6.png/246x0w.jpg"
        alt="dino"
      />
    </div>

    <script>
      // 1. 무엇을
      const dino = document.querySelector('#dino')

      // 2. 언제
      dino.addEventListener('click', function(){
        // 3. 어떻게
        console.log('아야!')
        // const div = document.querySelector('.bg')
        // div.innerHTML += '<h1>아야!</h1>'
      })

      let x = 0
      let y = 0
      // 1. 무엇을
      // 2. 언제 (keydown, keypress, keyup)
      document.addEventListener('keydown', e=>{
        // 3. 어떻게
        // console.log(e.code)
        if (e.code === 'ArrowLeft') {
          // console.log('왼쪽 화살표 눌렀음')
          // dino.style.marginRight = '50px'
          if (dino.x > 0) {
            x = x - 20
            dino.style.marginLeft = `${x}px`
          }
        } else if (e.code === 'ArrowRight') {
          // console.log('오른쪽 화살표 눌렀음')
          if (dino.x < document.documentElement.clientWidth) {
            x = x + 20
            dino.style.marginLeft = `${x}px`
          }
        } else if (e.code === 'ArrowUp') {
          // console.log('위쪽 화살표 눌렀음')
          y = y - 20
          dino.style.marginTop = `${y}px`
        } else if (e.code === 'ArrowDown') {
          // console.log('아래쪽 화살표 눌렀음')
          y = y + 20
          dino.style.marginTop = `${y}px`
        } else {
          console.log('잘못된 입력입니다.')
        }
      })
    </script>
  </body>
</html>
````



## 09.shopping_list.html

> ` https://www.w3schools.com/jsref/prop_style_textdecoration.asp `

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
  <h1>shopping list</h1>
  <input id="item-input" type="text">
  <button id="add-button">add</button>
  <ul id="shopping-list">
    <!-- <li><del>샴푸</del></li>
    <li>세제</li> -->
  </ul>
  <script>
    // 1. input 태그 찾기
    const input = document.querySelector('#item-input')
    // 2. button 태그 찾기
    const button = document.querySelector('#add-button')
    // 3. ul 태그 찾기
    const ul = document.querySelector('#shopping-list')

    button.addEventListener('click', function(e){
      // 사용자가 입력한 데이터 가져오기
      const itemValue = input.value
      // 기존의 input.value를 초기화시킨다.
      input.value = ''
      // console.log(itemValue)
      
      // 데이터를 이용해서 li태그로 묶어주기
      // <li>과자</li>
      const tag = document.createElement('li')
      // 태그 안쪽에 text 넣어주기
      tag.innerText = itemValue
      // console.log(tag)

      // 취소선
      tag.addEventListener('click', function(){
        // 취소선이 있다면
        if (tag.style.textDecoration === 'line-through'){
          // 취소선 삭제
          tag.style.textDecoration = ''
        } else {
          // 취소선이 없다면 취소선 추가
          tag.style.textDecoration = 'line-through'
        }
      })

      // ul 태그의 자식요소로 tag를 추가
      ul.appendChild(tag)

      // 삭제 버튼 생성
      const deleteBtn = document.createElement('button')
      deleteBtn.innerText = '삭제'

      // 삭제 버튼 이벤트 추가
      deleteBtn.addEventListener('click', function(e){
        console.log(e)
        tag.remove()
      })

      // 삭제 버튼을 tag의 자식요소로 추가
      tag.appendChild(deleteBtn)
    })

  </script>
</body>
</html>
```



## 10_kakao_map.html

> 카카오 개발자 센터, 카카오 맵 api

카카오 맵 api의 가이드를 따라한다.

카카오 맵 api의 docs를 잘 읽어봐야 원하는 기능을 추가할 수 있다.

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
  <div id="map" style="width:100%;height:400px;"></div>
  <!-- html 문서의 형식이 파일이기 때문에 src에 https:를 추가해준다. -->
  <script type="text/javascript" src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=0a0d219f44ee1f5ecd18379d61a3c00b"></script>
  <script>
    const container = document.querySelector('#map') //지도를 담을 영역의 DOM 레퍼런스
    const options = { //지도를 생성할 때 필요한 기본 옵션
      center: new kakao.maps.LatLng(33.450701, 126.570667), //지도의 중심좌표.
      level: 3, //지도의 레벨(확대, 축소 정도)
      // draggable: false // 드래그를 불가능하도록 만든다.
    }
    const map = new kakao.maps.Map(container, options); //지도 생성 및 객체 리턴

    // 전체 마커를 저장하는 배열
    const markers = []

    // 이벤트 생성
    kakao.maps.event.addListener(map, 'click', function(e){
      console.log(e.latLng.Ga, e.latLng.Ha)
      createMarker(e.latLng)
    })

    // 마커를 생성하는 함수
    const createMarker = (position) => {
      // 마커 생성
      const newMarker = new kakao.maps.Marker({
        map, // map: map
        position
      })
      // 마커를 생성하면 markers 리스트에 담긴다.
      markers.push(newMarker)
      newMarker.setMap(map)
      console.log(markers)
    }
  </script>
</body>
</html>
```

