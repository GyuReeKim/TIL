# Vue Subject Test

### 1.

vue cli를 사용하지 않아도 vue 개발이 가능하다



### 2.

HTML 요소의 속성

v-bind



### 3.

컴포넌트 이름

 name



### 4.

v-on

@



### 5.

 화면에 표시할 요소 정의

template



### 6.

`<img v-bind:src="imageUrl">`

속성에는 보간법`{{ }}` 사용 불가능하다



### 7.

````html
<p>HELLO VUE</p>
<p>{{message.toUpperCase()}}</p>
````



### 8.

computed에 화살표 함수를 사용하면?

this의 바인딩 대상이 변경되어 videoId에 접근할 수 없다. (화살표 함수의 this는 window를 찾는다.)



### 9.

MovieList 컴포넌트에서 props 속성에 movies와 genres를 추가해야한다.



### 10.

부모 컴포넌트와 자식 컴포넌트

부모 컴포넌트의 데이터가 바뀌면 자식 컴포넌트의 데이터도 바뀐다.

자식 컴포넌트는 물려받은 부모 컴포넌트의 데이터를 추가적으로 조작할 수 있다.

부모 컴포넌트의 데이터가 업데이트 되면 해당 데이터를 물려받은 자식 컴포넌트의 데이터는 자동 갱신된다.



### 11.

바인딩에서 사용 불가능한 것

{{const number = 1}}

변수 초기화 할당 불가능



### 12.

v-if / v-show

v-if문은 조건문 만족하면 렌더링

v-show는 항상 렌더링



### 13.

el에 접근하는 방법

app.$el



### 14.

Child Component가 받는 props이름

v-bind:d="[a, b, c, d]"

d를 받는다.



### 15.

emit의 이름

this.$emit('videoSelect', video)

videoSelect



### 16.

component의 data

함수와 return이 필요하다.

````vb
data(){
  return {
  }
}
````



### 17.

프로젝트 폴더 구성

사용자가 보는 페이지는 public/index.html 이다.

App.vue 컴포넌트는 root 인스턴스와 부모자식 관계이다.

main.js에서 vue인스턴스를 DOM에 마운트하는 코드를 작성한다.

components폴더에 파일 단위로 저장한다.




### 18.

`<span v-for="number in numbers" v-if="number % 3">{{number}}</span>`

124578



### 19.

`<template>`

template의 root를 하나의 엘리먼트로 묶지 않았다.



### 20.

양방향 바인딩

v-model



### 21.

.innerHTML

v-html



### 22.

v-bind:href="url"

:href="url"



### 23.

버튼을 눌렀을 때

v-on:click



### 24.

Welcome to the Vue world!

post.content