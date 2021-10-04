---
title: "Write a function 외 2문제"
tags: [Python, TIL]
style:
color:
description: Python 문제풀이
---
**Write a function**: <br/>

An extra day is added to the calendar almost every four years as February 29, and the day is called a leap day.<br/>

It corrects the calendar for the fact that our planet takes approximately 365.25 days to orbit the sun. <br/>

A leap year contains a leap day. <br/>

In the Gregorian calendar, three conditions are used to identify leap years: <br/>

The year can be evenly divided by 4, is a leap year, unless: <br/>
The year can be evenly divided by 100, it is NOT a leap year, unless: <br/>
The year is also evenly divisible by 400. Then it is a leap year. <br/>
This means that in the Gregorian calendar, the years 2000 and 2400 are leap years, while 1800, 1900, 2100, 2200, 2300 and 2500 are NOT leap years. 

```python
def is_leap(year):
    leap = False
    
    if year % 400 == 0:
        leap = True
    elif year % 4 == 0 and year % 100 !=0:
        leap = True
    # Write your logic here
    
    return leap

year = int(input())
print(is_leap(year))
```

**Print Function**: <br/>

The included code stub will read an integer, n, from STDIN.<br/>

Without using any string methods, try to print the following: <br/>

123..n <br/>

Note that "..." represents the consecutive values in between. <br/>

```python
if __name__ == '__main__':
    n = int(input())
    print(*range(1,n+1),sep="")
```

**List Comprehensions**: <br/>

Let's learn about list comprehensions! <br/>
 You are given three integers x,y and z representing the dimensions of a cuboid along with an integer n. <br/>
 Print a list of all possible coordinates given by (i,j,k) on a 3D grid where the sum of i+j+k is not equal to n. <br/>
 Here, 0<= i <=x; 0<= j <=y, 0<= k <= z. Please use list comprehensions rather than multiple loops, as a learning exercise. 
 
 ```python
 if __name__ == '__main__':
    x, y, z, n = (int(input()) for _ in range(4))
    
    print([[a,b,c] for a in range(0,x+1) for b in range(0,y+1) for c in range(0,z+1) if a+b+c!=n])
 ```

