---
title: "Python 간단 알고리즘 연습"
tags: [Python, TIL]
style:
color:
description: 생활코딩 참조

---

### 

```python
import pandas as pd
```

### 최대공약수 구하기

공약수 중에서 가장 큰 수 구하기

참고:[주니온TV](https://www.youtube.com/watch?v=DJhobtMGbNQ&feature=youtu.be&fbclid=IwAR32id3_mtNhhlzGlvKRSumdggbJRrZ1hvYkw_6h9ohogAYUHKZ6U4VpPAk)

##### 약수 구하기


```python
def divisor(n):
    div = []
    for i in range(1, n+1):
        if n % i ==0:
            div.append(i)
    return div
```

#### 공통인 약수 구하기


```python
def commons(n,m):
    common = []
    for i in n:
        if (i in m):
            common.append(i)
    return common
```


```python
x = divisor(100)
y = divisor(150)
```


```python
def greatest_common(n,m):
    div_n = divisor(n)
    div_m = divisor(m)
    comm = commons(div_n,div_m)
    return max(comm)
```


```python
greatest_common(100,150)
```




    50



### 로마 숫자의 아라비아 숫자 변환 문제

참고:[생활코딩](https://www.facebook.com/groups/codingeverybody)


```python
from itertools import compress
```


```python
romanDict = {'M':1000,'D':500,'C':100,
             'L':50,'X':10,'V':5,'I':1}
```


```python
romans = ['CCCLXIX','LXXX','XXIX','CLV','XIV','X',
         'CDXCII','CCCXLVIII','CCCI','CDLXIX','CDXCIX']
```


```python
# 밑의 코드는 제가 직접 구현한 코드가 아닌 생활코딩 댓글에 있는 정산하님의 코드입니다
```


```python
def toArabicNumber2(roman):
    roman_num = [romanDict[letter]for letter in roman] # roman값에 해당하는 dictionary의 value값 반환
    in_order = list(x<y for x,y in zip(roman_num,roman_num[1:])) #앞뒷값을 묶고 뒷값이 앞값 보다 크다면 True 반환
                                                                 #마지막 숫자는 더 이상 뒷값이 없기 때문에 아무것도 반환하지 않음
                                                                #그렇기 때문에 in_order를 반환해보면 length가 하나 적은걸 알 수 있다
    return sum(roman_num)-2*sum(compress(roman_num,in_order)) #in_order이 True인 경우 앞숫자를 2번 빼주는 방식
```

예를들어, <br/>
CCCLXIX 경우 뒷값이 앞값보다 큰 경우는 IX의 경우 밖에 없습니다 <br/>
즉, in_order이 True인건 I 밖에 없습니다. <br/>
sum(roman_num)한 값에 - in_order이 True인 부분을 *2해서 빼줍니다 <br/>
*2를 해주는 이유는 IX를 11로 계산했기 때문에 원래 값이 9로 만들어주기 위함입니다


```python
arabics =[]
```


```python
for str in romans:
    arabics.append(toArabicNumber2(str))
print(arabics)
```

    [369, 80, 29, 155, 14, 10, 492, 348, 301, 469, 499]


#### 아리비아 숫자를 로마 숫자로 변환 문제

참고:['생활코딩'](https://ideone.com/d5SSIF?fbclid=IwAR1btj95rrVatuVkOzgctIiTvc1OHab_7Q1-4P2xm6pl_MpB9j8GRQD6SOE)

이 코드 또한 제가 직접 구현한 코드가 아닌 코드를 이해하면서 공부하기 위한 좋은 코드인것 같아 가져왔습니다. <br/>
김민철님의 코드입니다


```python
from functools import reduce
#reduce 함수는 reduce(function,iterable,initializer=None)
# 첫 번째 파라미터는 함수가 들어가게 됩니다. lambda가 될 수도 있고 def로 정의해놓은 함수가 될 수도 있다. 
# 두 번째 파라미터는 계산을 하고자 하는 리스트가 들어가게 된다
# 세 번째 파라미터는 필수는 아니나 초기값을 설정하수 있다
result = reduce(lambda x,y: x+y,[1,2,3,4,5])
print(result)
result = reduce(lambda x,y: x+y,[1,2,3,4,5],100)
print(result)
```

    15
    115



```python
lis = [(1000,'M'),(900,'CM'),(500,'D'),(400,'CD'),(100,'C'),(90,'XC'),(50,'L'),(40,'XL'),(10,'X'),
      (9,'IX'),(5,'V'),(4,'IV'),(1,'I')]
```


```python
f = lambda num:reduce(lambda x,y:(x[0]%y[0],x[1]+y[1]*(x[0]//y[0])),lis,(num,''))
```


```python
numbers = [4100,2345,658,900,23,6,239,244]
```


```python
for number in numbers:
    print(f"{number}:{f(number)}")
```

    4100:(0, 'MMMMC')
    2345:(0, 'MMCCCXLV')
    658:(0, 'DCLVIII')
    900:(0, 'CM')
    23:(0, 'XXIII')
    6:(0, 'VI')
    239:(0, 'CCXXXIX')
    244:(0, 'CCXLIV')
