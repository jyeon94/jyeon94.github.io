---
title: "종합문제1"
tags: [Python,TIL]
style:
color:
description: First Part of Problem
---
참고: [위키독스](https://wikidocs.net/17114)   

### Q1 문자열 바꾸기

split와 조인을 사용하여 아래같이 변환하시오 <br/>
a:b:c:d -> a#b#c#d


```python
a = "a:b:c:d"
b = a.split(":")
```


```python
"#".join(b)
```




    'a#b#c#d'



### Q2 딕셔너리 값 추출하기


```python
a = {'A':90,'B':89}
a['C']
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-5-434632f09ba2> in <module>
          1 a = {'A':90,'B':89}
    ----> 2 a['C']
    

    KeyError: 'C'


C라는 key가 없으므로 위와 같은 오류가 발생한다. 오류 대신 70을 얻을 수 있도록 수정하라


```python
try:
    a['C']
except KeyError:
    print(70)
```

    70
    


```python
a.get('C',70)
```




    70



### Q3 리스트의 더하기와 extend 함수


```python
a = [1,2,3]
```


```python
a = a + [4,5]
a
```




    [1, 2, 3, 4, 5]




```python
a = [1,2,3]
a.extend([4,5])
a
```




    [1, 2, 3, 4, 5]



+와 extend의 차이점은 무엇인가?


```python
a = [1,2,3]
id(a)
```




    1786070448072




```python
a = a + [4,5]
id(a)
```




    1786070446216



id가 다른것을 확인할 수 있다 <br/>
두 리스트를 더한 새로운 리스트가 생성된 것이다


```python
a = [1,2,3]
print(id(a))
a.extend([4,5])
print(id(a))
```

    1786070390152
    1786070390152
    

새로운 리스트가 아닌 기존의 리스트에 값을 추가한것을 확인 할 수 있다.

### Q4 리스트 총합 구하기

밑의 리스트에서 50점 이상 점수의 총합을 구하시오


```python
A = [20,55,67,82,45,33,90,87,100,25]
```


```python
# 리스트 컴프리핸션을 통해 간단히 풀 수 있다.
sum([i for i in A if i >= 50])
```




    481



### Q5 피보나치 함수

입력을 정수 n으로 받았을 때, n 이하까지의 피보나치 수열을 출력하는 함수를 작성하라


```python
def fib(n):
    if n == 0:
        return 0
    if 0 < n < 3:
        return 1
    return fib(n-2) + fib(n-1)
```


```python
for i in range(10):
    print(fib(i))
```

    0
    1
    1
    2
    3
    5
    8
    13
    21
    34
    

### Q6 숫자의 총합 구하기

다음과 같은 숫자를 입력받아 입력받은 숫자의 총합을 구하는 프로그램을 작성하시오.


```python
num = input("숫자를 입력해주세요(,로 구분해주세요):")
number = num.split(',')
total = 0
for i in number:
    total += int(i)
print(total)
```

    숫자를 입력해주세요(,로 구분해주세요):1,2,3,4,5,6
    21
    

### Q7 한줄 구구단

사용자로부터 2~9의 숫자 중 하나를 입력받아 해당 숫자의 구구단을 한 줄로 출력하는 프로그램 작성하시오


```python
num = int(input('구구단을 출력할 숫자를 입력하세요(2-9):'))
for i in range(1,10):
    print(num*i,end=' ')
```

    구구단을 출력할 숫자를 입력하세요(2-9):2
    2 4 6 8 10 12 14 16 18 

### Q8 역순 저장

다음과 같은 내용의 파일 abc.txt가 있다.<br/>
AAA <br/>
BBB <br/>
CCC <br/>
DDD <br/>
EEE

다음과 같이 역순으로 바꾸어 저장하라 <br/>
EEE <br/>
DDD <br/>
CCC <br/>
BBB <br/>
AAA 


```python
f = open('abc.txt', 'r')
lines = f.readlines() 
f.close()

lines.reverse()     
```


```python
lines = [i.strip() for i in lines]
```


```python
f = open('abc.txt', 'w')
for line in lines:
    f.write(line)
    f.write('\n')        
f.close()
```

### Q9 평균값 구하기

sample.text 파일의 숫자 값을 모두 읽어 총합과 평균값을 구한 후 평균 값을 result.txt 파일에 작성하라


```python
f = open('sample.txt','r')
lines = f.readlines()
f.close()
```


```python
lines = [int(i.strip()) for i in lines]
```


```python
r = open('result.txt','w')
average = sum(lines)/len(lines)
r.write(str("평균{}".format(average)))
f.close()
```

### Q10 사칙연산 계산기

다음과 같이 작동하는 클래스 Calculator를 작성하시오 <br/>
cal1 = Calculator([1,2,3,4,5]) <br/>
cal1.sum() # 합계 <br/>
15 <br/>
cal1.avg() # 평균 <br/>
3.0


```python
class Calculator:
    def __init__ (self, num_list):
        self.num_list = num_list
    
    def sum(self):
        result = sum(self.num_list)
        return result
    def avg(self):
        total = self.sum()
        average = total/len(self.num_list)
        return average
```


```python
cal1 = Calculator([1,2,3,4,5])
print(cal1.sum())
print(cal1.avg())
```

    15
    3.0
    
