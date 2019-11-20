# Vue & Django (2019.11.20)

## 01. front에서 logout

### App.vue

```vue
<template>
  <div id="app">
    <div id="nav">
      <!-- router-link의 역할은 to에 대한 component를 보여주는 것이다. -->
      <router-link to="/">Home</router-link> |
      <router-link to="/login">login</router-link>
      <!-- logout은 특별히 보여줄 component가 없다. -->
      <!-- prevent는 a태그 기능을 사용하지 않고 이벤트 기능만 활성화한다. -->
      <a href="#" @click.prevent="logout">logout</a>
    </div>
    <div class="container">
      <router-view/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  methods: {
    logout: function(){
      // console.log("로그아웃버튼 눌림")
      this.$session.destroy() // 세션 정보를 전부 날린다.
      this.$router.push('/login') // 로그인페이지로 redirect
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

```



### 로그인상태와 로그아웃 상태 구분

```vue
<template>
  <div id="app">
    <div id="nav">
      <!-- 로그인 되어있다면 -->
      <div v-if="isAuthenticated">
        <!-- router-link의 역할은 to에 대한 component를 보여주는 것이다. -->
        <router-link to="/">Home</router-link> |
        <!-- logout은 특별히 보여줄 component가 없다. -->
        <!-- prevent는 a태그 기능을 사용하지 않고 이벤트 기능만 활성화한다. -->
        <a href="#" @click.prevent="logout">logout</a>
      </div>
      <!-- 로그인이 되어있지 않다면 -->
      <div v-else>
        <router-link to="/login">login</router-link>
      </div>
    </div>
    <div class="container">
      <router-view/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: function(){
    return {
      // 만들어지는 순간 할당이 되고 바뀌지 않는다.
      // 세션이 바뀌는 것은 computed와 watch는 탐지할 수 없다.
      isAuthenticated: this.$session.has('jwt')
    }
  },
  methods: {
    logout: function(){
      // console.log("로그아웃버튼 눌림")
      this.$session.destroy() // 세션 정보를 전부 날린다.
      this.$router.push('/login') // 로그인페이지로 redirect
    }
  },
  // 일 부분이라도 렌더링이 다시 된다면 함수를 다시 실행한다.
  // logout을 실행시키면서 router를 다른 곳으로 보낸다.
  // App.vue는 최상위 컴포넌트기 때문에 로그인 상태인지 모든 컴포넌트에서 확인할 수 있다.
  updated: function(){
    this.isAuthenticated = this.$session.has('jwt')
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

```



## 02. front에서 vue ui

플러그인 추가 - vuex - 설치

파일이 4개 변경된 것을 알 수 있다.

저장공간인 store 폴더가 추가되었다.

vue ui 종료



## 03. front에서 vuex를 사용하기

### store 폴더의 index.js

```js
import Vue from 'vue'
import Vuex from 'vuex' // vuex는 상태들을 관리해주는 역할을 한다.
import auth from './modules/auth.js'

Vue.use(Vuex)

export default new Vuex.Store({
  // vue에서 data
  state: {
  },
  // vue에서 methods
  mutations: {
  },
  // vue에서 methods들을 묶어놓음 (비동기적으로 처리)
  actions: {
  },
  // component 처럼 분리할 때
  modules: {
    auth,
  }
})

```



store 폴더의 modules 폴더의 auth.js

```js
// forms.py
// -----------------------
// class TodoForm(forms.ModelForm):
//   pass
// -----------------------

// views.py
// -----------------------
// from .forms import TodoForm
// -----------------------


// 변수로 만들어준다.
const state = {
  token: null,
}

const mutations = {
  // 사용자가 로그인을 하면 token을 저장해준다.
  setToken: function(state, token){
    state.token = token
  }
}

const actions = {
  login: function(options, token){
    options.commit('setToken', token)
  },
  logout: function(options){
    options.commit('setToken', null)
  },
}

export default {
  state,
  mutations,
  actions,
}
```



### 디코딩

```js
// forms.py
// -----------------------
// class TodoForm(forms.ModelForm):
//   pass
// -----------------------

// views.py
// -----------------------
// from .forms import TodoForm
// -----------------------

import jwtDecode from 'jwt-decode'

// 변수로 만들어준다.
const state = {
  token: null,
}

const mutations = {
  // 사용자가 로그인을 하면 token을 저장해준다.
  setToken: function(state, token){
    state.token = token
  }
}

const actions = {
  login: function(options, token){
    options.commit('setToken', token)
  },
  logout: function(options){
    options.commit('setToken', null)
  },
}

// state에 들어있는 정보를 수정하지 않는 선에서 가져온다.
// computed 속성과 비슷하다.
const getters = {
  isAuthenticated: function(state){
    // token값이 있으면 true를 반환하고 없으면 false를 반환한다.
    return state.token ? true: false
    // if (state.token) {
    //   return true
    // }
    // else {
    //   return false
    // }
  },
  requestHeader: function(){
    return {
      headers: {
        Authorization: "JWT " + state.token
      }
    }
  },
  userId: function(state){
    return state.token ? jwtDecode(state.token).user_id : null
    // // 로그인하지 않았는데 디코딩하려고 하면 에러가 난다.
    // return jwtDecode(state.token).user_id
  }
}

export default {
  state,
  mutations,
  actions,
  getters,
}
```



### TodoList와 Home에 computed 추가

> computed의 `원소`들을 `this.원소`로 전부 바꿔준다.

###### TodoList.vue

```vue
<template>
  <div>
    <h1>todo</h1>
    <!-- <li v-for="todo in todos" :key="todo.id">{{todo.title}}</li> -->
    <div class="card my-2" v-for="todo in todos" :key="todo.id">
      <div class="card-body d-flex justify-content-between">
        <span @click="updateTodo(todo)" :class="{completed: todo.completed}">{{todo.title}}</span>
        <!-- https://emojipedia.org/wastebasket/ -->
        <span @click="deleteTodo(todo)">🗑️</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
// import jwtDecode from 'jwt-decode'

export default {
  name: 'TodoList',
  // data: function(){
  //   return {
  //     todos: []
  //   }
  // }
  props: {
    todos: Array,
  },
  computed: {
    requestHeader: function(){
      // store의 header가 바뀌면 나머지도 바뀐다.
      return this.$store.getters.requestHeader
    },
    userId: function(){
      return this.$store.getters.userId
    }
  },
  methods: {
    deleteTodo: function(todo){
      // this.$session.start()
      // const token = this.$session.get('jwt')
      // const requestHeader = {
      //   headers: {
      //     Authorization: "JWT " + token
      //   }
      // }
      // requestHeader를 위에서 가져온다.

      axios.delete(`http://localhost:8000/api/v1/todos/${todo.id}/`, this.requestHeader)
      .then((res)=>{
        console.log(res)
        const targetTodo = this.todos.find(function(el){
          return el === todo
        })
        
        const idx = this.todos.indexOf(targetTodo)

        if (idx > -1) {
          this.todos.splice(idx, 1)
        }
      })
      .catch((error)=>{
        console.log(error)
      })
    },
    updateTodo(todo) {
      // this.$session.start()
      // const token = this.$session.get('jwt')
      // const decodedToken = jwtDecode(token)
      // const user_id = decodedToken.user_id

      // const requestHeader = {
      //   headers: {
      //     Authorization: "JWT " + token
      //   }
      // }

      const requestForm = new FormData()
      requestForm.append('user', this.userId)
      requestForm.append('title', todo.title)
      requestForm.append('completed', !todo.completed)

      axios.put(`http://localhost:8000/api/v1/todos/${todo.id}/`, requestForm, this.requestHeader)
      .then((res)=>{
        console.log(res)
        todo.completed = !todo.completed
      })
      .catch((error)=>{
        console.log(error)
      })
    }
  }
}
</script>

<style>
.completed {
  text-decoration: line-through;
  color: dimgrey
}
</style>
```

###### Home.vue

```vue
<template>
  <div class="home">
    <TodoInput @createTodo="createTodo"/>
    <!-- <h1>Todos</h1> -->
    <TodoList v-bind:todos="todos"/>
  </div>
</template>

<script>
import router from '../router'
import TodoList from '../components/TodoList.vue'
import TodoInput from '@/components/TodoInput.vue' // @는 src
// import jwtDecode from 'jwt-decode' // vuex에서 번역작업을 거쳐서 온다.
import axios from 'axios'
import { mapGetters } from 'vuex'

export default {
  name: 'home',
  components: {
    TodoList,
    TodoInput,
  },
  data: function(){
    return {
      todos: []
    }
  },
  computed:{
    // 구조를 분해하는 문법
    ...mapGetters([
      'isAuthenticated',
      'requestHeader',
      'userId',
    ])
  },
  methods: {
    checkLoggedIn(){
      // Session Storage에 token이 있으면 login 된 상태
      this.$session.start() // session 시작
      // session에 jwt가 없으면
      // if (!this.$session.has('jwt')){
      if (!this.isAuthenticated){
        // 로그인 페이지로 redirect
        router.push('/login')
      }
    },
    getTodos(){
      // // console.log("사용자의 todo목록 가져오기")
      // this.$session.start()
      // const token = this.$session.get('jwt')
      // const decodedToken = jwtDecode(token)
      // // console.log(decodedToken)
      // const user_id = decodedToken.user_id

      // const requestHeader = {
      //   // headers 안에 적어줘야 한다.
      //   headers: {
      //     Authorization: "JWT " + token
      //   }
      // }
    
      axios.get(`http://localhost:8000/api/v1/users/${this.userId}/`, this.requestHeader)
      .then((res)=>{
        // console.log(res)
        this.todos = res.data.todo_set // vue devtools의 Home에서 확인할 수 있다.
      })
      .catch((error)=>{
        console.log(error)
      })
    },
    createTodo(title){
      // // getTodos에서 복사하였다.
      // this.$session.start()
      // const token = this.$session.get('jwt')
      // const decodedToken = jwtDecode(token)
      // const user_id = decodedToken.user_id

      // const requestHeader = {
      //   headers: {
      //     Authorization: "JWT " + token
      //   }
      // }

      // axios가 요청을 보낼 때, 일반적으로 key와 value로 된 table을 보낸다.
      // 표의 규격을 만들어 준 것이다.
      const requestForm = new FormData()
      requestForm.append('user', this.userId)
      requestForm.append('title', title)
      
      axios.post('http://localhost:8000/api/v1/todos/', requestForm, this.requestHeader)
      .then((res)=>{
        console.log(res)
        this.todos.push(res.data) // 데이터가 화면에서 바로 추가되도록 구성한다.
      })
      .catch((error)=>{
        console.log(error)
      })
    }
  },
  // checkLoggedIn 함수를 실행한다.
  mounted: function(){
    this.checkLoggedIn() // 로그인 되었는지 확인
    this.getTodos()
  }
}
</script>

<style>

</style>
```



#### `...`

> `...`은 구조를 분해하는 문법이다.



### main.js

> Vue Session을 사용하지 않으므로 지워준다.
