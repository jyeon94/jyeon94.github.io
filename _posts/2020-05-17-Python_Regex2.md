---
title: "정규표현식2"
tags: [Python, TIL]
style:
color:
description: Second Part of Regex
---
*참고: <https://wikidocs.net/1642>

### 정규식을 이용한 문자열 검색


```python
# 기본 라이브러리인 re를 사용
import re
```

re.compile을 활용하여 패턴을 객체화 하여 문자열 검색 수행을 할 수 있다.

|Method|목적
|------|----|
|match()| 문자열의 처음부터 정규식과 매치되는지 조사
|search()| 문자열 전체를 검색하여 정규식과 매치되는지 조사
|findall()| 정규식과 매치되는 모든 문자열(substring)를 리스트로 반환
|finditer()| 정규식과 매치되는 모든 문자열(substring)을 반복 가능한 객체로 반환

math, search는 정규식과 매치될 때는 매칭된 객체를 반환, 매치되지 않을 때는 None 반환


```python
# 소문자 알파벳 한번이상 반복
p = re.compile('[a-z]+')
```

#### match


```python
m = p.match('jupyter')
print(m)
```

    <re.Match object; span=(0, 7), match='jupyter'>
    


```python
m = p.match("4 jupyter")
print(m)
```

    None
    

#### search


```python
m = p.search("python")
print(m)
```

    <re.Match object; span=(0, 6), match='python'>
    


```python
#4와 공백은 패턴과 맞지 않으므로 2,9까지만 매칭된다
m = p.search("4 jupyter")
print(m)
```

    <re.Match object; span=(2, 9), match='jupyter'>
    

#### findall


```python
result = p.findall("life is too short")
print(result)
```

    ['life', 'is', 'too', 'short']
    

#### finditer


```python
result = p.finditer("life is too short")
print(result)
```

    <callable_iterator object at 0x0000020EC0EB5E88>
    


```python
for r in result:
    print(r)
```

    <re.Match object; span=(0, 4), match='life'>
    <re.Match object; span=(5, 7), match='is'>
    <re.Match object; span=(8, 11), match='too'>
    <re.Match object; span=(12, 17), match='short'>
    

finditer는 findall과 동일하지만 결과로 반복 가능한 객체를 반환하다.

### match 객체의 매서드

|Method|목적
|------|----|
|group()| 매치된 문자열을 반환
|start()| 매치된 문자열의 시작 위치를 반환
|end()| 매치된 문자열의 끝 위치를 반환
|span()| 매치된 문자열의 (시작,끝)에 해당하는 튜플을 반환


```python
m = p.search("4 jupyter")
m.group()
```




    'jupyter'




```python
m.start()
```




    2




```python
m.end()
```




    9




```python
m.span()
```




    (2, 9)



#### 모듈 단위로 수행


```python
p = re.compile('[a-z]+')
m = p.match("python")
```


```python
# 위의 코드를 축약된 형태로 작성 가능하다.
m = re.match('[a-z]+','python')
```

### 컴파일 옵션

- DOTALL(S) - .이 줄바꿈 문자를 포함하여 모든 문자와 매치할 수 있도록 한다.
- IGNORECASE(I) - 대소문자에 관계없이 매치할 수 있도록 한다.
- MULTILINE(M) - 여러줄과 매치 할 수 있도록 한다.(^,$ 메타문자의 사용과 관계가 있는 옵션)
- VERBOSE(X) - verbose 모드를 사용할 수 있도록 한다.(정규식을 보기 편하고 주석등을                                                     사용할 수 있게된다.)

#### DOTALL, S

. 메타문자는 줄바꿈 문자(\n)를 제외한 모든 문자와 매치되는 규칙이 있다.<br/>
만약 \n 문자도 포함하여 매치하고 싶다면 re.DOTALL 또는 re.S 옵션을 사용해 정규식을 컴파일하면 된다.


```python
p = re.compile('a.b')
m = p.match('a\nb')
print(m)
```

    None
    


```python
p = re.compile('a.b',re.DOTALL)
m = p.match('a\nb')
print(m)
```

    <re.Match object; span=(0, 3), match='a\nb'>
    

#### IGNORECASE, I

대소문자 구별 없이 매치를 수행할 때 사용하는 옵션


```python
p = re.compile('[a-z]',re.I)
p.match('jupyter')
```




    <re.Match object; span=(0, 1), match='j'>




```python
p.match('Jupyter')
```




    <re.Match object; span=(0, 1), match='J'>




```python
p.match('JUPYTER')
```




    <re.Match object; span=(0, 1), match='J'>



#### MULTILINE, M

위 옵션은 ^, $와 연관된 옵션 <br/>
^는 문자열의 처음을 의미하고, $는 문자열의 마지막을 의미 <br/>
^python은 문자열의 처음은 항상 python으로 시작해야 매치 <br/>
python$이라면 문자열의 마지막은 항상 python으로 끝나야 매치


```python
p = re.compile("^python\s\w+")
```


```python
data = """python one
life is too short
python two
you need python
python three"""

print(p.findall(data))
```

    ['python one']
    


```python
p = re.compile("^python\s\w+",re.MULTILINE)
print(p.findall(data))
```

    ['python one', 'python two', 'python three']
    

MULTILINE 옵션을 통해 문자열 전체가 아닌 각 줄의 처음이라는 의미 <br/>
re.MULTILINE 옵션은 ^,$ 메타 문자를 문자열의 각 줄마다 적용

#### VERBOSE, X

복잡한 정규식을 주석 또는 줄 단위로 구분할 수 있게 한다.


```python
charref = re.compile(r"""
 &[#]                # Start of a numeric entity reference
 (
     0[0-7]+         # Octal form
   | [0-9]+          # Decimal form
   | x[0-9a-fA-F]+   # Hexadecimal form
 )
 ;                   # Trailing semicolon
""", re.VERBOSE)
```
