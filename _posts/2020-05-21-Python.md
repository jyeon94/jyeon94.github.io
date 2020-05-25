---
title: "종합문제2"
tags: [Python, TIL]
style:
color:
description: Last Part of Regex
---
참고:[위키독스](https://wikidocs.net/17114)   

### Q11

C:\doit 디렉터리에 mymod.py 파이썬 모듈이 있다고 가정해 보자.   
명령 프롬프트 창에서 파이썬 셸을 열어 이 모듈을 import해서 사용할 수 있는    
방법을 모두 기술하시오. 


```python
import sys
sys.path.append('C;\doit')
import mymod
```

### Q12 오류와 예외 처리

다음 코드의 실행 결과를 예측하고 그 이유를 설명하라


```python
result = 0
try:
    [1,2,3][3]
    "a" + 1
    4 / 0
except TypeError:
    result += 1
except ZeroDivisionError:
    result += 2
except IndexError:
    result += 3
finally:
    result += 4
```


```python
# 첫번째 [1,2,3][3]은 indexError이 나서 3이더해지고 finally구문이 실행되어 4가 더해진다.
print(result)
```

    7
    

### Q13 DashInsert 함수

DashInsert 함수는 숫자로 구성된 문자열을 입력받은 뒤 문자열 안에서 홀수가 연속되면    
두 수 사이에 - 를 추가하고, 짝수가 연속되면 * 를 추가하는 기능을 갖고 있다.      
DashInsert 함수를 완성하시오.


```python
data = "4546793"
numbers = list(map(int,data))
result = []
```


```python
for i,num in enumerate(numbers):
    result.append(str(num))
    if i < len(numbers) -1:
        is_odd = num %2 ==1
        is_next_odd = numbers[i+1] % 2 ==1
        if is_odd and is_next_odd:
            result.append("-")
        elif not is_odd and not is_next_odd:
            result.append("*")
print("".join(result))
```

    454*67-9-3
    

### Q14 문자열 압축하기

문자열을 입력받아 같은 문자가 연속적으로 반복되는 경우에    
그 반복 횟수를 표시해 문자열을 압축하여 표시하시오.


```python
def str_iter(s):
    _c = ""
    cnt = 0
    result = ""
    for c in s:
        if c!=_c:
            _c = c
            if cnt: result += str(cnt)
            result += c
            cnt = 1
        else:
            cnt +=1
    if cnt: result += str(cnt)
    return result
```


```python
str_iter("aaabbbcc")
```




    'a3b3c2'



### Q15 Duplicate Numbers

"0~9"의 문자로 된 숫자를 입력받았을 때,이 입력값이 0~9의 모든 숫자를    
각각 한 번씩만 사용한 것인지 확인하는 함수를 작성하시오.


```python
def duplicate(s):
    result = []
    for num in s:
        if num not in result:
            result.append(num)
        else:
            return False
    return len(result) == 10
```


```python
print(duplicate("0123456789"))  
```

    True
    

### Q16 모스 부호 해독

모스 부호(dot:. dash:-)를 해독하여 영어 문장으로 출력하는 프로그램을 작성하시오.


```python
dic = {
    '.-':'A','-...':'B','-.-.':'C','-..':'D','.':'E','..-.':'F',
    '--.':'G','....':'H','..':'I','.---':'J','-.-':'K','.-..':'L',
    '--':'M','-.':'N','---':'O','.--.':'P','--.-':'Q','.-.':'R',
    '...':'S','-':'T','..-':'U','...-':'V','.--':'W','-..-':'X',
    '-.--':'Y','--..':'Z'
}
```


```python
def mose(src):
    result = []
    for i in src.split("  "):
        for char in i.split(" "):
            result.append(dic[char])
        result.append(" ")
    return "".join(result)
```


```python
mose(".... .  ... .-.. . . .--. ...  . .- .-. .-.. -.--")
```




    'HE SLEEPS EARLY '



### Q17 기초 메타 문자


```python
import re
p = re.compile("a[.]{3,}b")
print(p.match("accb"))
print(p.match("a....b"))
print(p.match("aaab"))
print(p.match("a.cccb"))
```

    None
    <re.Match object; span=(0, 6), match='a....b'>
    None
    None
    

### Q18 문자열 검색


```python
#이 코드의 결과값은?
import re
p = re.compile("[a-z]+")
m = p.search("5 python")
m.start() + m.end()
```




    10



위 식은 p의 시작위치인 2와 끝값이 n의 위치인 8이 더해져 10이라는 결과가 나온다

### Q19 그루핑

다음과 같은 문자열에서 휴대폰 번호 뒷자리인 숫자 4개를 "####"로 바꾸는 프로그램을    
정규식을 사용하여 작성하시오.


```python
a = """
park 010-9999-9988
kim 010-9909-7789
lee 010-8789-7768
"""
```


```python
p = re.compile("(\d{3}[-]\d{4})[-]\d{4}")
```


```python
p.sub("\g<1>-####",a)
```




    '\npark 010-9999-####\nkim 010-9909-####\nlee 010-8789-####\n'



### Q20 전방탐색

긍정형 전방 탐색 기법을 사용하여 .com, .net이 아닌 이메일 주소는    
제외시키는 정규식을 작성하시오.


```python
p = re.compile(".*[@].*[.](?=com$|net$).*$")
```


```python
print(p.match("pahkey@gmail.com"))
print(p.match("kim@daum.net"))
print(p.match("lee@myhome.co.kr"))
```

    <re.Match object; span=(0, 16), match='pahkey@gmail.com'>
    <re.Match object; span=(0, 12), match='kim@daum.net'>
    None
    
