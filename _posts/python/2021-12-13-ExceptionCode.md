---
layout: post
title: OverlappingExeptionCode
accent_image: 
  background: url('/assets/img/blog/jj-ying.jpg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  중첩 예외 처리 방법
invert_sidebar: true
categories:
  - python
---

# 중첩 예외 처리 코드
> 예외 처리 코드란?  
> 프로그램을 실행하다보면 예상치 못한 오류로 인해 프로그램이 멈출 수 있다.  
> 그때 발생한 오류를 건너뛰고 다음 코드를 실행할 수 있도록 해주는 것이 예외 처리코드이다.

아래는 예외 발생 예시이다.

### 1) 예외 발생 코드 생성
```py
# 유형별 예외처리
print('\n유형별 예외처리')
try:
    div = 1000/2.53
    print('div=%5.2f' %(div))
    div = 1000 / 0 # 1차: 산술적 예외
    f = open('d:/coding/pyEdu/ch8_meterial/data/test.txt') # 2차: 파일 열기
    num = int(input('숫자 입력: ')) # 3차: 기타 예외
    print('num =', num)
```

```py
# 다중 예외처리 클래스
except ZeroDivisionError as e: # 산술적 예외처리
    print('오류 정보: ', e)
except FileNotFoundError as e: # 파일 열기 예외처리
    print('오류 정보: ', e)
except Exception as e : # 기타 예외 처리
    print('오류 정보: ', e)
finally :
    print('finally 영역 - 항상 실행되는 영역')

```
(수정중)