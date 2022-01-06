---
layout: post
title: 2021-10-06-python-Variables&memory addresses
subtitle: 변수와 메모리 주소
tags: Variables, 메모리주소
description: >
sitemap: false
hide_last_modified: true
categories:
  - code
  - python
---

# 변수와 메모리 주소
>변수란?
정해지지 않은 임의의 값들이 저장되어 있는 그릇과 같습니다.

<br>

```py
var1 = "hello python"
print(var1)
print(id(var1))
```
var1이라는 그릇(변수)은 "hello python" 이라는 문자열을 담게 됐습니다.
그리고 출력할 때 앞에 id함수를 사용하면 var1에 대한 메모리 주소가 출력됩니다.
```py
#출력결과
hello python
58159816
```
<br>

아래는 var1 이라는 변수에 다른 값을 넣어 출력한 결과입니다.
```py
var1 = 100
print(var1)
print(id(var1))
```
```py
#출력결과
100
1720740576
```
<br>
var2라는 변수도 만들어서 출력해보면 다음과 같은 결과를 얻을 수 있습니다.
```py
var2 = 150.25
print(var2)
print(id(var2))
```
```py
#출력결과
150.25
55909296
```
<br>
이상으로 변수와 변수의 메모리 주소를 확인해보았습니다.  
감사합니다. :)
