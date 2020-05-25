---
title: "정규표현식3"
tags: [Python, TIL]
style:
color:
description: Third Part of Regex
---
참고: [위키독스](https://wikidocs.net/1642)

### 추가 메타문자

#### "|"

"|" 메타 문자는 or과 동일한 의미로 사용


```python
import re
```


```python
p = re.compile('Crow|Servo')
m = p.match('CrowHello')
print(m)
```

    <re.Match object; span=(0, 4), match='Crow'>
    

#### "^"

"^" 메타 문자는 문자열의 맨 처음과 일치함을 의미


```python
print(re.search('^Life','Life is too short'))
```

    <re.Match object; span=(0, 4), match='Life'>
    


```python
print(re.search('^Life','My Life'))
```

    None
    

#### $

$ 메타 문자는 문자열의 끝과 매치함을 의미


```python
print(re.search('short$','Life is too short'))
```

    <re.Match object; span=(12, 17), match='short'>
    


```python
print(re.search('short$','Life is too short, you need python'))
```

    None
    

#### \A

\A는 문자열의 처음과 매치됨을 의미. "^" 메타 문자와 동일하지만 re.MULTILINE 옵션 사용시 다르게 해석된다. <br/>
re.MULTILINE 옵션을 사용할 경우 ^은 각 줄의 문자열의 처음과 매치되지만 \A는 줄과 상관없이 <br/>
전체 문자열의 처음하고만 매치된다.

#### \Z

\Z는 문자열의 끝과 매칭되는데 위의 경우와 마찬가지로 $메타 문자와는 달리 전체 문자열의 끝과 매치된다.

#### \b

\b는 단어 구분자로 보통 단어는 whitespace에 의해 구분된다.


```python
p = re.compile(r'\bclass\b')
print(p.search('no class at all'))
```

    <re.Match object; span=(3, 8), match='class'>
    


```python
print(p.search('the declassified algorithm'))
```

    None
    


```python
print(p.search('one subclass is'))
```

    None
    

\b 메타 문자 사용시 \b는 파이썬에선 백스페이스를 의미하므로 Raw String임을 알려주는 <br/>
기호 r을 반드시 붙여 주어야 한다.

#### \B

\B 메타 문자는 \b 메타 문자와 반대의 경우로 whitespace로 구분된 단어가 아닌 경우에만 매칭된다.


```python
p = re.compile(r'\Bclass\B')
print(p.search('no class at all'))
```

    None
    

### 그루핑

문자열이 계속해서 반복되는지 조사하는 정규식을 작성하고 싶다면 그루핑을 활용하면 된다.

그룹을 만들어 주는 메타 문자는 ()이다.


```python
p = re.compile('(ABC)+')
m = p.search('ABCABCABC OK?')
print(m)
```

    <re.Match object; span=(0, 9), match='ABCABCABC'>
    


```python
print(m.group())
```

    ABCABCABC
    


```python
p = re.compile(r"\w+\s+\d+[-]\d+[-]\d+")
m = p.search("park 010-1234-1234)
```

    park 010-1234-1234
    

위 예시에서 이름만 추출하고 싶다면 밑의 예시처럼 할 수 있다.


```python
p = re.compile(r'(\w+)\s+\d+[-]\d+[-]\d+')
m = p.search("park 010-1234-1234")
print(m.group(1))
```

    park
    

group(인덱스)|설명
-----------|----
group(0)| 매치된 전체 문자열
group(1)| 첫 번째 그룹에 해당되는 문자열
group(2)| 두 번째 그룹에 해당되는 문자열
group(n)| n 번째 그룹에 해당되는 문자열


```python
p = re.compile(r'(\w+)\s+(\d+[-]\d+[-]\d+)')
m = p.search("park 010-1234-1234")
print(m.group(2))
```

    010-1234-1234
    


```python
# 중첩 그룹핑도 가능하다
p = re.compile(r'(\w+)\s+((\d+)[-]\d+[-]\d+)')
m = p.search("park 010-1234-1234")
print(m.group(3))
```

    010
    

#### 그루핑된 문자열 재참조


```python
p = re.compile(r'(\b\w+)\s+\1')
p.search('Paris in the the spring').group()
```




    'the the'



정규식 (\b\w+)\s+\1은 (그룹) + " " + 그룹과 동일한 단어와 매치됨을 의미한다. <br/>
이렇게 정규식을 만든다면 2개의 동일한 단어를 연속적으로 사용해야만 매칭된다. <br/>
이것은 재참조 메타 문자인 \1로 가능하다. \1은 정규식의 그룹 중 첫 번째 그룹을 가리킨다.

#### 그루핑된 문자열에 이름 붙이기

정규식은 그룹을 만들 때 그룹 이름을 지정할 수 있게 했다.

(?P<그룹명>..)으로 가능하다.


```python
p = re.compile(r"(?P<name>\w+)\s+((\d+)[-]\d+[-]\d+)")
m = p.search("park 010-1234-1234")
print(m.group("name"))
```

    park
    

그룹 이름을 사용하면 재참조도 가능하다.


```python
p = re.compile(r'(?P<word>\b\w+)\s+(?P=word)')
p.search('Paris in the the spring').group()

```




    'the the'




```python
$ ipython locate
```


      File "<ipython-input-36-d870970cb45f>", line 1
        $ ipython locate
        ^
    SyntaxError: invalid syntax
    

