# Vue & Django (2019.11.18)

## jwt

> `https://jwt.io/ `

Header : 어떤 정보를 가지고 있는지

Payload : 유저 정보

Signature : 시크릿키값



## todo-project

> 프로젝트 2개 생성

```bash
$ vue create todo-front

$ mkdir todo-back
$ cd todo-back/
$ django-admin startproject todoback .
```

생성 후 각자의 폴더에서 vs code를 열어준다.

서로 다른 프로젝트이기  때문에 git도 따로 관리해주는 것이 좋다.



## 01. back에서 todos 앱 생성

기본 setting 변경 후 git init해준다.



## 02. front의 Vue 프로젝트 매니저

```bash
$ vue ui
```

위의 코드를 실행하면 'Vue 프로젝트 매니저'가 실행된다.

프로젝트 가져오기를 눌러 경로가 `C:` `Users` `student` `todo-project` `todo-front`인지 확인하자.

- 플러그인 추가
- router 검색
- +버튼 누르고 설치
- `Use history mode for router?`를 on 하고 설치 완료
  - 파일이 변경된 것을 알려준다.

- 커밋 변경사항을 누르고 `add router`를 커밋해준다.

- 서버를 끊어준다.



```bash
$ npm run serve
```

router를 등록하고 서버에 들어가면 `Home`과 `About`버튼이 생성된 것을 확인할 수 있다.

router를 사용하는 이유는, 컴포넌트를 이미 나누긴 했지만 비슷한 컴포넌트끼리 더 나눠주는 편이 좋기 때문이다.



개발자 도구를 눌렀을 때, App아래의 자식요소가 Home을 누르면 Home으로 About을 누르면 About으로 바뀌는 것을 확인할 수 있다.



###### Home

![router_home](Vue & Django (2019.11.18).assets/router_home.PNG)



###### About

![router_about](Vue & Django (2019.11.18).assets/router_about.PNG)



## 03. front에서 Login 만들기

### views에 Login.vue 생성

```vue
<template>
  <div class="login">
    <h1>로그인 페이지입니다.</h1>
  </div>

</template>

<script>
export default {
  name: 'Login',
}
</script>

<style>

</style>
```





### index.js에서 route 추가해주기

> about 대신 login 컴포넌트를 등록해준다.

```js
import Login from '../views/Login.vue' // Login 불러오기

const routes = [
  {
    path: '/', // 어떤 경로를 불러올지
    name: 'home',
    component: Home // 어떤 이름으로 불러올지
  },
  // Login 컴포넌트 등록
  {
    path: '/login',
    name: 'login',
    component: Login
  }
]
```



### App.vue의 router-link에 login 경로 지정

```vue
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/login">login</router-link>
    </div>
    <div class="container">
      <router-view/>
    </div>
  </div>
</template>
```



### bootstrap

> bootstrap 적용을 위해 public 폴더의 index.html에 css 코드를 붙여넣는다.



### LoginForm 생성

```vue
<template>
  <div>
    <div v-if="loading" class="spinner-border" role="status">
      <span class="sr-only">Loading...</span>
    </div>
    <!-- template에는 하나의 요소만 필요 -->
    <div v-else class="login-div col-6 offset-3">
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

  <script>
  import axios from 'axios'

  export default {
    data: function (){
      return {
        credential: {
          // django에서는 id가 username이다.
          username: '',
          password: ''
        },
        loading: false
      }
    },
    methods: {
      login(){
        console.log('로그인 시도!!!')
        // Django 서버로 요청을 보내야 한다.
        axios.post('http://localhost:8000', this.credential) // object 자체를 넘겨준다.
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



### Login.vue에 LoginForm 등록

```vue
<template>
  <div class="login">
  <h1>로그인 페이지입니다.</h1>
  <LoginForm/>
  </div>

</template>

<script>
import LoginForm from '../components/LoginForm.vue'
export default {
  name: 'Login',
  components: {
    LoginForm
  }
}
</script>

<style>

</style>
```



### loading 확인하기

###### loading 초기값 false

![login1](Vue & Django (2019.11.18).assets/login1.PNG)



###### 개발자도구에서 true 값으로 변경

![login2](Vue & Django (2019.11.18).assets/login2.PNG)





## 04. front에서 axios 요청 보내기

```bash
$ npm install --save axios
```



### django 서버가 꺼진 상태에서 로그인

![no_django_login](Vue & Django (2019.11.18).assets/no_django_login.PNG)



### django 서버가 켜진 상태에서 로그인

> 두 네트워크를 서로 다른 네트워크로 보고 오류메세지 발생

![django_login](Vue & Django (2019.11.18).assets/django_login.PNG)



## 05. back에서 요청 받기

### 필요한 앱 설치

```bash
$ pip install djangorestframework
$ pip install djangorestframework-jwt
$ pip install django-cors-headers
```

```python
INSTALLED_APPS = [
    'todos',
    'rest_framework',
    'corsheaders',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```



### django restframework 검색

> ` https://www.django-rest-framework.org/ `

```python
# https://jpadilla.github.io/django-rest-framework-jwt/

REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_jwt.authentication.JSONWebTokenAuthentication',
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.BasicAuthentication',
    ),
}
```



### django restframework jwt 검색

> ` https://jpadilla.github.io/django-rest-framework-jwt/ `

settings.py와 urls.py에 추가해준다.

````python
# Additional Settings 중 일부만 가져왔다.
# id에 대한 권한과 같다.
JWT_AUTH = {
    'JWT_SECRET_KEY': SECRET_KEY,
    'JWT_ALGORITHM': 'HS256',
    'JWT_ALLOW_REFRESH': True,
    'JWT_EXPIRATION_DELTA': datetime.timedelta(days=3), # key가 3일이 지나면 만료된다.
    'JWT_REFRESH_EXPIRATION_DELTA': datetime.timedelta(days=7), # 만료된 key를 7일 이내면 재발급 받을 수 있다.
}
````



#### 참고사항

##### hashing & sorting

hashing은 비밀번호 등을 암호화해준다. ex) sha256

sorting은 비밀번호가 같을 경우 같은 값이 암호화되므로 일부 문자열을 추가해줘 서로 다른 값으로 만들어준다.



### django cors 검색

` https://pypi.org/project/django-cors-headers/ `

````python
CORS_ORIGIN_ALLOW_ALL = True # 배포단계에서는 자신의 domain만 열어줘야한다.
````



### model 생성

```python
from django.db import models
from django.contrib.auth.models import AbstractUser

# Create your models here.
class User(AbstractUser):
    pass

class Todo(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    title = models.CharField(max_length=100)
    completed = models.BooleanField(default=False)
```



### api-token-auth/에서 create superuser한 계정 Post 요청보내기

> 아래처럼 token값이 나와야한다.

![api_token_auth_createsuperuser](Vue & Django (2019.11.18).assets/api_token_auth_createsuperuser.PNG)



### token 값을 복사해 jwt.io의 Encoded에 붙여넣기

> PAYLOAD의 값을 사용할 예정이다.

![jwt_token_encoded](Vue & Django (2019.11.18).assets/jwt_token_encoded.PNG)



## 06. 로그인 요청을 받기 위해 LoginForm의  axios 부분 수정

![axios_post_api_token_auth](Vue & Django (2019.11.18).assets/axios_post_api_token_auth.PNG)

````vue
<template>
  <div>
    ...
  </div>
</template>

<script>
import axios from 'axios'

export default {
  ...
  methods: {
    login(){
      ...
      // Django 서버로 요청을 보내야 한다. token 값도 가져와야 한다.
      axios.post('http://localhost:8000/api-token-auth/', this.credential)
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
````



### loading true 추가

> 로그인을 하면 loading이 된다.

###### login 전

![axios_post_loading_true1](Vue & Django (2019.11.18).assets/axios_post_loading_true1.PNG)



###### login 후

![axios_post_loading_true2](Vue & Django (2019.11.18).assets/axios_post_loading_true2.PNG)



```vue
<template>
  <div>
    ...
  </div>
</template>

<script>
import axios from 'axios'

export default {
  ...
  methods: {
    login(){
      ...
      // Django 서버로 요청을 보내야 한다. token 값도 가져와야 한다.
      axios.post('http://localhost:8000/api-token-auth/', this.credential)
        .then((res)=>{
          this.loading = true
          console.log(res)
        })
        .catch((error)=>{
          this.loading = true
          console.log(error)
        })
    }
  }
}
</script>

<style>

</style>
```

