#  Vue & Django (2019.11.19)

## 01. front에서 데이터를 session storage에 넣기

```bash
$ npm install vue-session
```



### main.js에 VueSession추가

```js
import VueSession from 'vue-session'

Vue.use(VueSession) // 개발자도구의 Application 안에있는 Session Storage를 사용할 수 있도록 도와준다.
```



### Login Form에 token을 session storage에 저장

> 개발자도구 Application의 Session Storage에서 확인해보자.

```vue
<script>
import axios from 'axios'

export default {
  ...
  methods: {
    login(){
      console.log('로그인 시도!!!')
      // Django 서버로 요청을 보내야 한다. token 값도 가져와야 한다.
      axios.post('http://localhost:8000/api-token-auth/', this.credential) // object 자체를 넘겨준다.
        .then((res)=>{
          this.loading = true
          
          // console.log(res) // res를 출력하면 data의 안에 token 값이 있는 것을 확인할 수 있다.
          res.data.token

          this.$session.start()
          this.$session.set('jwt', res.data.token) // 저장을 한다.
        })
        .catch((error)=>{
          this.loading = true
          console.log(error)
        })
    }
  }
}
</script>
```



### redirect 기능

> router.push() 를 사용하여 redirect한다.

```vue
<script>
import axios from 'axios'
import router from '../router' // index.js가 불러와진다.

export default {
  ...
  methods: {
    login(){
      console.log('로그인 시도!!!')
      // Django 서버로 요청을 보내야 한다. token 값도 가져와야 한다.
      axios.post('http://localhost:8000/api-token-auth/', this.credential) // object 자체를 넘겨준다.
        .then((res)=>{
          this.loading = true
          
          // console.log(res) // res를 출력하면 data의 안에 token 값이 있는 것을 확인할 수 있다.
          res.data.token

          this.$session.start()
          this.$session.set('jwt', res.data.token) // 저장을 한다.

          // redirect 기능을 한다.
          router.push('/') // root 주소(Home.vue)로 밀어준다.
        })
        .catch((error)=>{
          this.loading = true
          console.log(error)
        })
    }
  }
}
</script>
```



## 02. front에서 validation

> vue validation 라이브러리를 사용하는 편이 더 좋다.

```vue
<script>
import axios from 'axios'
import router from '../router' // index.js가 불러와진다.

export default {
  data: function (){
    return {
      credential: {
        // django에서는 id가 username이다.
        username: '',
        password: ''
      },
      loading: false,
      errors: [],
    }
  },
  methods: {
    login(){
      if (this.checkForm()){
        console.log('로그인 시도!!!')
        // Django 서버로 요청을 보내야 한다. token 값도 가져와야 한다.
        axios.post('http://localhost:8000/api-token-auth/', this.credential) // object 자체를 넘겨준다.
        .then((res)=>{
          this.loading = true
          
          // console.log(res) // res를 출력하면 data의 안에 token 값이 있는 것을 확인할 수 있다.
          res.data.token

          this.$session.start()
          this.$session.set('jwt', res.data.token) // 저장을 한다.

          // redirect 기능을 해준다.
          router.push('/') // root 주소(Home.vue)로 밀어준다.
        })
        .catch((error)=>{
          this.loading = true
          console.log(error)
        })
      }
    },
    // validation 만들기. login 할 때 사용한다.
    // django처럼 validation을 할 때 vue library도 적용 가능하다.
    checkForm(){
      this.errors = []
      // 비밀번호 길이가 8보다 작을 경우
      if (this.credential.password.length < 8) {this.errors.push("비밀번호는 8글자가 넘어야합니다.")}
      // 아이디를 적지 않은 경우
      if (!this.credential.username) {this.errors.push("아이디를 입력해주세요.")}
      console.log(this.errors)
      // 데이터가 정상적으로 들어왔다면
      if (this.errors.length === 0) {
        return true
      }
    }
  }
}
</script>
```



### error 메세지 출력

> 서버 요청 보내기 전에 보여주는 error 메세지

![vue_login_valication](Vue & Django (2019.11.19).assets/vue_login_valication.PNG)

```vue
<template>
  <div>
    <div v-if="loading" class="spinner-border" role="status">
      <span class="sr-only">Loading...</span>
    </div>
    <!-- template에는 하나의 요소만 필요 -->
    <div v-else class="login-div col-6 offset-3">
        
      <div v-if="errors.length" class="error-list alert alert-danger">
        <!-- idx는 for문을 돌았을 때의 idx를 말한다. -->
        <!-- 에러 목록 -->
        <div v-for="(error, idx) in errors" :key="idx">{{error}}</div>
      </div>
        
      <div class="form-group">
        <label for="id">ID</label>
        <input
        id="id"
        class="form-control"
        type="text"
        v-model="credential.username"
        >
      </div>
      <div class="form-group">
        <label for="password">PASSWORD</label>
        <input
        id="password"
        class="form-control"
        type="password"
        v-model="credential.password"
        >
      </div>
      <button class="btn btn-primary" @click="login">로그인</button>
    </div>
  </div>
</template>
```



## 03. front에서 Todos가 나오는 Home 페이지

### 로그인 되어있는지 확인

> 로그인이 되어있지 않다면 login페이지로 redirect 해준다.

```vue
<template>
  <div class="home">
    <h1>Todos</h1>
  </div>
</template>

<script>
import router from '../router'

export default {
  name: 'home',
  components: {

  },
  methods: {
    checkLoggedIn(){
      // Session Storage에 token이 있으면 login 된 상태
      this.$session.start() // session 시작
      // session에 jwt가 없으면
      if (!this.$session.has('jwt')){
        // 로그인 페이지로 redirect
        router.push('/login')
      }
    }
  }
}
</script>
```



### checkLoggedIn 함수 실행

> mounted로 실행해준다.

```vue
<script>

export default {
  ...
  // checkLoggedIn 함수를 실행한다.
  mounted: function(){
    this.checkLoggedIn()
  }
}
</script>
```



## 04. back에서 직렬화

### todos 앱에 serializers.py 생성

```python
from rest_framework import serializers
from .models import Todo

# todo 하나를 조작하기 위한 serializer
class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = ('id', 'user', 'title', 'completed',)
```



### todoback 프로젝트 폴더의 urls.py에 경로 추가

```python
from django.contrib import admin
from django.urls import path, include
from rest_framework_jwt.views import obtain_jwt_token


urlpatterns = [
    path('admin/', admin.site.urls),
    path('api-token-auth/', obtain_jwt_token), # djangorestframeworkjwt
    path('api/v1/', include('todos.urls')),
]
```



### todos앱의 urls.py와 views.py

```python
from django.urls import path
from . import views

urlpatterns = [
    # localhost:8000/api/v1/todos/
    path('todos/', views.todo_create),
]
```

```python
from django.http import JsonResponse, HttpResponse
from django.shortcuts import render
from .serializers import TodoSerializer
# api_view는 api view 페이지를 만들어준다.
from rest_framework.decorators import api_view, permission_classes, authentication_classes
from rest_framework.permissions import IsAuthenticated
from rest_framework_jwt.authentication import JSONWebTokenAuthentication


# Create your views here.
@api_view(['POST'])
# 인증이 된 사람만 허가해준다. 튜플 형태로 넣어준다.
@permission_classes((IsAuthenticated,)) # 허가 : 조건에 맞는 사람들만 허가
@authentication_classes((JSONWebTokenAuthentication,)) # 인증
def todo_create(request):
    serializer = TodoSerializer(data=request.POST) # 인스턴스화 시켜준다.
    if serializer.is_valid():
        serializer.save()
        # 사용자가 방금 입력한 결과를 보여준다.
        # json형태로 다른 곳에서도 사용할 수 있도록 해준다.
        return JsonResponse(serializer.data)
    # 오류가 있는지 확인한다.
    return HttpResponse(status=400)
```



## 05. postman에서 확인

> ` chrome://apps/`의 postman에서 todo-back-test 폴더를 생성한다.



### 로그인 테스트

POST -  `http://localhost:8000/api-token-auth/ ` - Send

Body - username과 password - 입력후 token 확인

ctrl + s - 로그인테스트 저장



### 게시물 작성

POST -  `http://localhost:8000/api/v1/todos/ ` - Send

ctrl + s - 게시물작성 저장

Headers - Authorization 에 `JWT (띄어쓰기 필요)로그인테스트에서 복사한 키값` - 입력후 아무것도 뜨지 않아야 한다.

400 Bad Request를 확인할 수 있다. `serializer.is_valid()`를 통과하지 못한 것을 알 수 있다.

Body - title, user 작성 - Send하면 json 파일을 확인할 수 있다.

ctrl + s



### 게시물 읽기

```python
from django.shortcuts import render, get_object_or_404
from .models import Todo


@api_view(['GET', 'PUT', 'DELETE'])
@permission_classes((IsAuthenticated,))
@authentication_classes((JSONWebTokenAuthentication,))
def todo_detail(request, id):
    todo = get_object_or_404(Todo, id=id)

    # read
    if request.method == "GET":
        # 직렬화 시킨다.
        serializer = TodoSerializer(todo)
        return JsonResponse(serializer.data)
    
    elif request.method == "PUT":
        pass
    
    elif request.method == "DELETE":
        pass
```

게시물 작성 - Save as - 게시물 읽기

GET - ` http://localhost:8000/api/v1/todos/1/ ` - Send하면 json 파일을 확인할 수 있다.



### 게시물 수정

```python
@api_view(['GET', 'PUT', 'DELETE'])
@permission_classes((IsAuthenticated,))
@authentication_classes((JSONWebTokenAuthentication,))
def todo_detail(request, id):
    todo = get_object_or_404(Todo, id=id)

    # read
    if request.method == "GET":
        # 직렬화 시킨다.
        serializer = TodoSerializer(todo)
        return JsonResponse(serializer.data)
    
    # update
    elif request.method == "PUT":
        # rest framework에서 데이터를 가져오기 위해 request.data를 사용해 가져온다.
        serializer = TodoSerializer(todo, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data)
        # 사용자가 실패했을 경우 어떤 페이지를 혹은 어떤 메세지를 보여주는 것이 좋을지 생각해야한다.
        return HttpResponse(status=400)
    
    elif request.method == "DELETE":
        pass
```

게시물 읽기 - Save as - 게시물 수정

PUT - ` http://localhost:8000/api/v1/todos/1/ ` - Send하면 수정된 json 파일을 확인할 수 있다.



### 게시물 삭제

```python
@api_view(['GET', 'PUT', 'DELETE'])
@permission_classes((IsAuthenticated,))
@authentication_classes((JSONWebTokenAuthentication,))
def todo_detail(request, id):
    todo = get_object_or_404(Todo, id=id)

    # read
    if request.method == "GET":
        # 직렬화 시킨다.
        serializer = TodoSerializer(todo)
        return JsonResponse(serializer.data)

    # update
    elif request.method == "PUT":
        # rest framework에서 데이터를 가져오기 위해 request.data를 사용해 가져온다.
        serializer = TodoSerializer(todo, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data)
        # 사용자가 실패했을 경우 어떤 페이지를 혹은 어떤 메세지를 보여주는 것이 좋을지 생각해야한다.
        return HttpResponse(status=400)
        
    # delete
    elif request.method == "DELETE":
        todo.delete()
        # return JsonResponse({"msg": "삭제되었습니다."})
        return HttpResponse(status=204)
```

게시물 수정 - Save as - 게시물 삭제

Delete - ` http://localhost:8000/api/v1/todos/1/ ` - Send하면 204 status를 확인할 수 있다.



## 06. back에서 User의 Todos 정보 확인

### serializers.py에 UserSerializer 추가

```python
from rest_framework import serializers
from .models import Todo, User

# todo_list를 만들기 위해서 user의 todo목록을 가져옴
class UserSerializer(serializers.ModelSerializer):
    # 1 : N 관계이기 때문에 todo_set 사용 가능하다.
    todo_set = TodoSerializer(many=True)
    class Meta:
        model = User
        fields = ('id', 'username', 'todo_set')
```



### urls.py에 url 경로 추가

```python
from django.urls import path
from . import views

urlpatterns = [
    # localhost:8000/api/v1/todos/
    path('todos/', views.todo_create),
    # localhost:8000/api/v1/todos/10/
    path('todos/<int:id>/', views.todo_detail), # 읽기, 삭제, 수정
    
    path('users/<int:id>/', views.user_detail),
]
```



### user_detail 함수 생성

> 정보를 보여줘야하기 때문에 get 방식을 사용한다.

```python
@api_view(['GET'])
@permission_classes((IsAuthenticated,))
@authentication_classes((JSONWebTokenAuthentication,))
def user_detail(request, id):
    user = get_object_or_404(User, id=id)

    # 내가 작성한 todo에 대한 정보만 확인할 수 있도록 설정
    if request.user != user:
        return  HttpResponse(status=403)

    if request.method == "GET":
        serializer = UserSerializer(user)
        return JsonResponse(serializer.data)
```



## 07. front에서 TodoList.vue 생성

### TodoList.vue의 기본 틀 만들기

```vue
<template>
  <div>
    <h1>todo</h1>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data: function(){
    return {
      todos: []
    }
  }
}
</script>

<style>

</style>
```



### Home.vue에서 TodoList.vue 등록해주기

```vue
<template>
  <div class="home">
    <!-- <h1>Todos</h1> -->
    <TodoList/>
  </div>
</template>

<script>
import router from '../router'
import TodoList from '../components/TodoList.vue'

export default {
  name: 'home',
  components: {
    TodoList,
  },
  methods: {
    checkLoggedIn(){
      // Session Storage에 token이 있으면 login 된 상태
      this.$session.start() // session 시작
      // session에 jwt가 없으면
      if (!this.$session.has('jwt')){
        // 로그인 페이지로 redirect
        router.push('/login')
      }
    }
  },
  // checkLoggedIn 함수를 실행한다.
  mounted: function(){
    this.checkLoggedIn()
  }
}
</script>

<style>

</style>
```



## 07. front에서 django 서버에 요청 보내기

> mount됨과 동시에 django 서버에 요청을 보낸다.

### jwt-decode 설치

> 해석해서 사용할 수 있도록 decode 작업을 해준다.

```bash
$ npm install jwt-decode
```

어디에 데이터를 저장하는 것이 좋을지 고민해봐야 한다.



### Home에 데이터 저장

> TodoList의 data를 일단 지워준 후 home에 작성한다.

```vue
<template>
  <div class="home">
    <!-- <h1>Todos</h1> -->
    <TodoList v-bind:todos="todos"/>
  </div>
</template>

<script>
import router from '../router'
import TodoList from '../components/TodoList.vue'
import jwtDecode from 'jwt-decode'
import axios from 'axios'

export default {
  name: 'home',
  components: {
    TodoList,
  },
  data: function(){
    return {
      todos: []
    }
  },
  methods: {
    ...
    getTodos(){
      // console.log("사용자의 todo목록 가져오기")
      this.$session.start()
      const token = this.$session.get('jwt')
      const decodedToken = jwtDecode(token)
      // console.log(decodedToken)
      const user_id = decodedToken.user_id

      const requestHeader = {
        // headers 안에 적어줘야 한다.
        headers: {
          Authorization: "JWT " + token
        }
      }
    
      axios.get(`http://localhost:8000/api/v1/users/${user_id}/`, requestHeader)
      .then((res)=>{
        // console.log(res)
        this.todos = res.data.todo_set // vue devtools의 Home에서 확인할 수 있다.
      })
      .catch((error)=>{
        console.log(error)
      })
    },
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



### TodoList에 props 작성

```vue
<template>
  <div>
    <h1>todo</h1>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  props: {
    todos: Array,
  }
}
</script>

<style>

</style>
```



### TodoList에 리스트 출력

`````vue
<template>
  <div>
    <h1>todo</h1>
    <!-- <li v-for="todo in todos" :key="todo.id">{{todo.title}}</li> -->
    <div class="card my-2" v-for="todo in todos" :key="todo.id">
      <div class="card-body d-flex justify-content-between">
        <span>{{todo.title}}</span>
        <span>🗑️</span>
      </div>
    </div>
  </div>
</template>
`````



### deleteTodo 함수 생성

> 새로고침해야 삭제된다. django 서버에서는 삭제되지만 바로 보여지지는 않는다.

```vue
<template>
  <div>
    <h1>todo</h1>
    <!-- <li v-for="todo in todos" :key="todo.id">{{todo.title}}</li> -->
    <div class="card my-2" v-for="todo in todos" :key="todo.id">
      <div class="card-body d-flex justify-content-between">
        <span>{{todo.title}}</span>
        <span @click="deleteTodo(todo)">🗑️</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'TodoList',
  props: {
    todos: Array,
  },
  methods: {
    deleteTodo: function(todo){
      this.$session.start()
      const token = this.$session.get('jwt')
      const requestHeader = {
        headers: {
          Authorization: "JWT " + token
        }
      }

      axios.delete(`http://localhost:8000/api/v1/todos/${todo.id}/`, requestHeader)
      .then((res)=>{
        console.log(res)
      })
      .catch((error)=>{
        console.log(error)
      })
    }
  }
}
</script>

<style>

</style>
```



### 화면에서도  사라지도록 구성

```vue
<template>
  <div>
    <h1>todo</h1>
    <!-- <li v-for="todo in todos" :key="todo.id">{{todo.title}}</li> -->
    <div class="card my-2" v-for="todo in todos" :key="todo.id">
      <div class="card-body d-flex justify-content-between">
        <span>{{todo.title}}</span>
        <span @click="deleteTodo(todo)">🗑️</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'TodoList',
  props: {
    todos: Array,
  },
  methods: {
    deleteTodo: function(todo){
      this.$session.start()
      const token = this.$session.get('jwt')
      const requestHeader = {
        headers: {
          Authorization: "JWT " + token
        }
      }

      axios.delete(`http://localhost:8000/api/v1/todos/${todo.id}/`, requestHeader)
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
    }
  }
}
</script>

<style>

</style>
```



### 게시물 생성하기 위한 TodoInput 컴포넌트 생성

```vue
<template>
  <div class="todo-input">
    <div class="input-group">
      <input type="text" class="form-control" v-model="title" @keydown.enter="createTodo">
      <span class="input-group-btn">
        <button class="btn btn-primary" @click="createTodo">+</button>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoInput',
  data: function(){
    return {
      title: ''
    }
  },
  methods: {
    createTodo: function(){
      this.$emit('createTodo', this.title)
      // 다음 데이터와 충돌하지 않기 위해서 초기화한다.
      this.title = ''
    }
  }
}
</script>

<style>

</style>
```



### Home에서 데이터를 바로바로 볼 수 있도록 구성

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
import jwtDecode from 'jwt-decode'
import axios from 'axios'

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
  methods: {
    checkLoggedIn(){
      // Session Storage에 token이 있으면 login 된 상태
      this.$session.start() // session 시작
      // session에 jwt가 없으면
      if (!this.$session.has('jwt')){
        // 로그인 페이지로 redirect
        router.push('/login')
      }
    },
    getTodos(){
      // console.log("사용자의 todo목록 가져오기")
      this.$session.start()
      const token = this.$session.get('jwt')
      const decodedToken = jwtDecode(token)
      // console.log(decodedToken)
      const user_id = decodedToken.user_id

      const requestHeader = {
        // headers 안에 적어줘야 한다.
        headers: {
          Authorization: "JWT " + token
        }
      }
    
      axios.get(`http://localhost:8000/api/v1/users/${user_id}/`, requestHeader)
      .then((res)=>{
        // console.log(res)
        this.todos = res.data.todo_set // vue devtools의 Home에서 확인할 수 있다.
      })
      .catch((error)=>{
        console.log(error)
      })
    },
    createTodo(title){
      // getTodos에서 복사하였다.
      this.$session.start()
      const token = this.$session.get('jwt')
      const decodedToken = jwtDecode(token)
      const user_id = decodedToken.user_id

      const requestHeader = {
        headers: {
          Authorization: "JWT " + token
        }
      }

      // axios가 요청을 보낼 때, 일반적으로 key와 value로 된 table을 보낸다.
      // 표의 규격을 만들어 준 것이다.
      const requestForm = new FormData()
      requestForm.append('user', user_id)
      requestForm.append('title', title)
      
      axios.post('http://localhost:8000/api/v1/todos/', requestForm, requestHeader)
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



### 완료된 항목 설정

> 완료된 항목에 취소선을 넣어준다.

```vue
<template>
  <div>
    <h1>todo</h1>
    <!-- <li v-for="todo in todos" :key="todo.id">{{todo.title}}</li> -->
    <div class="card my-2" v-for="todo in todos" :key="todo.id">
      <div class="card-body d-flex justify-content-between">
        <span @click="updateTodo(todo)" :class="{completed: todo.completed}">{{todo.title}}</span>
        <span @click="deleteTodo(todo)">🗑️</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import jwtDecode from 'jwt-decode'

export default {
  name: 'TodoList',
  props: {
    todos: Array,
  },
  methods: {
    deleteTodo: function(todo){
      this.$session.start()
      const token = this.$session.get('jwt')
      const requestHeader = {
        headers: {
          Authorization: "JWT " + token
        }
      }

      axios.delete(`http://localhost:8000/api/v1/todos/${todo.id}/`, requestHeader)
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
      this.$session.start()
      const token = this.$session.get('jwt')
      const decodedToken = jwtDecode(token)
      const user_id = decodedToken.user_id

      const requestHeader = {
        headers: {
          Authorization: "JWT " + token
        }
      }

      const requestForm = new FormData()
      requestForm.append('user', user_id)
      requestForm.append('title', todo.title)
      requestForm.append('completed', !todo.completed)

      axios.put(`http://localhost:8000/api/v1/todos/${todo.id}/`, requestForm, requestHeader)
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



