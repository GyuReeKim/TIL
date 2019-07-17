# Python 1주차 (2019.07.16)

## 기본 값

default 사용자가 어떠한 데이터도 넣지 않은 경우를 말한다.

기본 값이 설정이 되어있더라도 사용자가 입력을 넣으면 동일하게 호출할 수 있다.



```python
def greeting(name='익명', age):
    print(f'{name}님은 {age}살입니다.')
```

 default 값이 없는 것이 default값이 있는 것 앞에 나왔다.

default 값을 뒤로 넣어야 한다.

## 가변인자 리스트

`*args`는 tuple 형태로 처리된다.

```python
def my_max(*args):
    result = args[0]
    for i in args:
        if result < i:
            result = i
        else:
            pass
    return result

my_max(-1, -2, -3, -4)
```



`**kwargs` 는 keyword arguments의 약자로, dictionary를 인자로 넘길 때 사용한다.

```python
def my_url(itemPerPage=10, **kwargs):
    if itemPerPage not in range(1, 11):
        return '1~10까지의 값을 넣어주세요.' # return을 만나면 다음 코드를 실행 안함
    if 'key' not in kwargs or 'targetDt' not in kwargs:
        return '필수 요청변수가 누락되었습니다.'
    
    base_url = ' http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?'
    base_url += f'itemPerPage={itemPerPage}&'

    for key, value in kwargs.items():
        base_url += f'{key}={value}&'
        
    return base_url
    
my_url(key='abc', targetDt='yyyymmdd', itemPerPage=10)
```





## 이름공간 및 스코프(Scope)

괄호는 함수를 의미한다. 

```python
str = '5' # str은 함수가 아닌 변수
print(str)
str(5) # 함수를 호출할 수 없다.
```



### 전역 변수

```python
global_num = 3
def localscope2():
    global_num = 5 # 위의 global_num과는 위치가 다르므로 다른 global_num이다.
    print(f'global_num이 {global_num}으로 바뀌었습니다.')
    
localscope2() # 'global_num이 5으로 바뀌었습니다.'
print(global_num) # 3이 print된다.
```



## 재귀 함수

분할정보법. 점화식을 푸는 것과 같다.

큰 일을 작게 쪼개는 것이 분할이다.



