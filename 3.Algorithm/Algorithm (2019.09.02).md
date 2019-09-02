# Algorithm (2019.09.02)

## 리스트 (list)

### 순차리스트

연산에 따라 작업속도가 늘어나 문제가 발생한다.



### 노드

```python
import sys

class None:
    def __init__(self, data, link):
        self.data = data
        self.link = link
        
def addLast(data): # 마지막 노드 추가
    global pHead
    if pHead == None:
        pHead = Node(data, None)
    else:
        p = pHead
        while p.link != None:
            p = p.link
        p.link = Node(data, None)
    return

def add(data, idx): # idx 위치에 새 노드 추가
    global pHead
    p = pHead
    n = 0
    while n < idx - 1:
        p = p.link
        n += 1
    t = p.link
   	p.link = Node(data, t)
    return

def get(idx): # idx의 데이터 리턴
```



## 이중 연결 리스트

```python
import sys

class Node:
    def __init__(self, data, pre, link): # 이중 연결 리스트
        self.data = data
        self.pre = pre
        self.link = link
```



### 병합 정렬

병합 정렬은 메모리를 계속 새롭게 만들어서 쓰기 때문에 메모리 사용량이 많다는 단점이 있다.

