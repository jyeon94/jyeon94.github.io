---
title: "정규표현식1"
tags: [Python,TIL]
style: 
color:
description: First Part of Regex
---
참고: [위키독스](https://wikidocs.net/1642)   

### 정규표현식이란?

정규 표현식(Regular Expressions)는 복잡한 문자열을 처리할 때 사용하는 기법   
파이썬 뿐만 아니라 문자열을 다루는 모든 곳에서 사용한다.


```python
# 정규표현식을 사용하지 않았을 때 마스킹 방법
data = """
park 800905-1049118
kim  700905-1059119
"""
```


```python
result = []
for line in data.split("\n"):
    word_result = []
    for word in line.split(" "):
        if len(word) == 14 and word[:6].isdigit() and word[7:].isdigit():
            word = word[:6] + "-" + "*"*7
        word_result.append(word)
    result.append(" ".join(word_result))
print("\n".join(result))
```

    
    park 800905-*******
    kim  700905-*******
    
    


```python
# 정규표현식 사용하면 훨씬 간단히 작동하는 것을 볼 수 있다.
import re

data = """
park 800905-1049118
kim  700905-1059119
"""

pat = re.compile("(\d{6})[-]\d{7}")
print(pat.sub("\g<1>-*******",data))
```

    
    park 800905-*******
    kim  700905-*******
    
    

#### 메타 문자

밑의 문자들은 메타 문자들로 원래 그 문자가 아닌 특별한 용도로 사용하는 문자

. ^ $ * + ? { } [ ] \ | ( )

##### 문자 클래스 []

[] 사이의 문자들과 매치라는 의미를 가졌다

하이픈(-)을 사용하면 두 문자 사이의 범의를 의미한다.<br/>
[a-c]는 [abc]와 동일 <br/>
[0-5]는 [012345]와 동일

[a-zA-z]:알파벳 모두<br/>
[0-9]: 숫자

^ 메타 문자를 사용할 경우에는 반대라는 의미를 갖는다

[자주 사용하는 문자 클래스]
- \d - 숫자와 매치, [0-9]와 동일 표현
- \D - 숫자가 아닌 것과 매치, [^0-9]와 동일한 표현
- \s - whitespace(공백) 문자와 매치, [ \t\n\r\f\v]와 동일한 표현식
- \S - whitespace 문자가 아닌 것과 매치, [^ \t\n\r\f\v]와 동일한 표현식
- \w - 문자+숫자와 매치, [a-zA-Z0-9_]와 동일한 표현식
- \W - 문자+숫자가 아닌 문자와 매치, [^a-zA-Z0-9_]와 동일한 표현식

##### Dot(.)

\n을 제외한 모든 문자와 매칭됨

a.b

> "a + 모든문자 + b"를 의미한다

a와 b라는 문자사이에 어떤 문자가 들어가도 모두 매치된다.

a[.]b

> "a + Dot(.)문자 + b"를 의미한다.

##### 반복(*)

*바로 앞에 있는 문자가 0부터 무한대로 반복될 수 있다는 의미이다.


```python
cat = re.compile("ca*t")
```


```python
#"a"가 0번 반복되어 매치
cat.match("ct")
```




    <re.Match object; span=(0, 2), match='ct'>




```python
#a가 0번 이상 반복되어 매치 (1번 반복)
cat.match("cat")
```




    <re.Match object; span=(0, 3), match='cat'>




```python
#a가 3번 이상 반복되어 매치 (3번 반복)
cat.match("caaat")
```




    <re.Match object; span=(0, 5), match='caaat'>



##### 반복 (+)

+는 최소 1번 이상 반복될 때 사용한다.


```python
cat2 = re.compile("ca+t")
```


```python
#a가 0번 반복되어 매치되지 않음
cat2.match("ct")
```


```python
#a가 1번 이상 반복되어 매치 (1번 반복)
cat2.match("cat")
```




    <re.Match object; span=(0, 3), match='cat'>




```python
#a가 1번 이상 반복되어 매치 (3번 반복)
cat2.match("caaat")
```




    <re.Match object; span=(0, 5), match='caaat'>



##### 반복 ({m,n},?)

{m,n} 정규식을 사용하면 반복 횟수가 m부터 n까지 매치할 수 있다.<br/>
{3,} -> 반복 횟수가 3 이상인 겨우 <br/>
{,3} -> 반복 횟수가 3 이하를 의미한다 <br/>
생략된 m은 0과 동일하며, 생략된 n은 무한대(2억 개 미만)의 의미를 갖는다.<br/>

- {m}


```python
cat3 = re.compile("ca{2}t")
```


```python
#a가 1번만 반복되어 매치되지 않음
cat3.match("cat")
```


```python
#a가 2번 반복되어 매치
cat3.match("caat")
```




    <re.Match object; span=(0, 4), match='caat'>



- {m,n}


```python
# c + a(2~5회 반복) + t
cat4 = re.compile("ca{2,5}t")
```


```python
# a가 1번만 반복되어 매치도지 않음
cat4.match('cat')
```


```python
#a가 2번 반복되어 매치
cat4.match('caat')
```




    <re.Match object; span=(0, 4), match='caat'>




```python
#a가 5번 반복되어 매치
cat4.match('caaaaat')
```




    <re.Match object; span=(0, 7), match='caaaaat'>



- ?

반복은 아니지만 ?는 비슷한 개념이다. ? 메타문자가 의미하는 것은 {0,1}이다.


```python
# a + b(있어도 되고 없어도 된다) + c
abc = re.compile("ab?c")
```


```python
# b가 1번 사용되어 매치
abc.match('abc')
```




    <re.Match object; span=(0, 3), match='abc'>




```python
# b가 0번 사용되어 매치
abc.match('ac')
```




    <re.Match object; span=(0, 2), match='ac'>


