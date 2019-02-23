## 딕셔너리 만들기

- 가장 쉬운 방법

```python
import random

res = ["피자헛", "버거킹", "파파존스"]
name = random.choice(res)

restaurants={
  "피자헛" : "02-333-1111",
  "버거킹" : "02-111-2222",
  "파파존스" : "02-111-2222"
}
number = restaurants[name]
print(f"{name}의 전화번호는 {number}입니다.")
```



- 어려운 방법

```python
import random


restaurants={
  "피자헛" : "02-333-1111",
  "버거킹" : "02-111-2222",
  "파파존스" : "02-111-2222"
}
a = restaurants.keys()
print(a)
```

##### 리스트 형태가 아니므로  list(restaurants.keys())를 추가

##### 결과값

```
['피자헛', '버거킹', '파파존스']
```



결과값이 아직 리스트형태이기 때문에 다시 수정하면,

```python
import random

restaurants={
  "피자헛" : "02-333-1111",
  "버거킹" : "02-111-2222",
  "파파존스" : "02-111-2222"
}
name = random.choice(list(restaurants.keys()))
number = restaurants[name]
print(name,number)
```



## 반복문

```python
dust = [5,3,2]

for i in dust:
    print(i)
```

while True:  계속해서 반복되기 때문에 종료조건이 필요하지만

for i in dust:는 종료조건이 필요 없다





##### 다양한 for문 사용

```python
greeting="hi!"
for i in range(1,10):
#for i in "abcdefghij":
#for i in [0,1,2,3,4,5,6,7,8,9]:
#for i in (0,1,2,3,4,5,6,7,8,9):
  print(greeting)

```



### 웹 API 받아오기

api : 서비스간의 정보제공

##### 웹 커뮤니케이션 방식 : 요청(request) 응답(response)

