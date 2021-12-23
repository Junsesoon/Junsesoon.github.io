---
layout: post
title: ExeptionCode
accent_image: 
  background: url('/assets/img/blog/jj-ying.jpg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  단순 예외 처리 방법
invert_sidebar: true
categories:
  - python
  - code
---

# 단순 예외 처리 코드
> 예외 처리 코드란?  
> 프로그램을 실행하다보면 예상치 못한 오류로 인해 프로그램이 멈출 수 있다.  
> 그때 발생한 오류를 건너뛰고 다음 코드를 실행할 수 있도록 해주는 것이 예외 처리코드이다.

아래는 예외 발생 예시이다.

### 1) 예외 발생 코드 생성
```py
print('프로그램 시작!!!')
x= [10, 30, 25.2, 'num', 14, 51] #예외 발생 원인
for i in x :
    print(i)
    y= i**2 #예외 발생
    print('y= ', y)
print('프로그램 종료')
```
```py
# 실행결과
프로그램 시작!!!
10
y=  100
30
y=  900
25.2
y=  635.04
num
Traceback (most recent call last):
  File "C:\python\lib\code.py", line 90, in runcode
    exec(code, self.locals)
  File "<input>", line 7, in <module>
TypeError: unsupported operand type(s) for ** or pow(): 'str' and 'int'
```
다음과 같이 x변수 안에 있는 값들을 순서대로 계산하다가 문자형 자료를 만나면  
더이상 계산할 수 없는 오류(예외)가 발생한다.

### 2) 예외 처리 코드 적용
예외처리는 try ~ except 함수를 이용한다.
```py
print('프로그램 시작!!!')
for i in x:
    try:
        y = i**2
        print('i=', i, 'y= ', y)
    except :
        print('숫자 아님: ', i)
print('프로그램 종료')
```
```py
# 실행결과
프로그램 시작!!!
i= 10 y=  100
i= 30 y=  900
i= 25.2 y=  635.04
숫자 아님:  num
i= 14 y=  196
```
예외 처리 코드를 적용하면 위와 같이  
try: 에 포함된 코드에서 예외가 발생했을 경우  
except: 에 포함된 코드가 실행된 후 이어서 프로그램이 실행된다.

