---
layout: post
title: Simple-Regression
image: 
  path: /assets/img/posts/SimpleRegression.jpeg
description: >
  단순선형 회귀분석, 잔차, 회귀방정식
sitemap: false
hide_last_modified: true
invert_sidebar: false
categories:
  - r
  - code
---

# 단순선형 회귀분석
> 회귀분석이란?  
> : 특정 변수(독립변수)가 다른 변수(종속변수)에 어떠한 영향을 미치는가를 분석하는 방법

* 회귀분석은 독립변수와 종속변수가 모두 등간척도 또는 비율척도로 구성되어 있어야 한다.

## 환경설정
본 글은 R3.6.3ver에서 작성되었습니다.  
선형회귀 분석을 하기 위한 라이브러리 환경설정
```R
  # 환경설정
rm(list=ls())
getwd()
setwd("c:/rwork")

  # 라이브러리 모음
install.packages("lme4")
library(lme4)
install.packages("car") #다중공선성 확인
library(car)
install.packages('lmtest') #잔차분석
library(lmtest)
```
확인 창이 뜨면 전부 아니오 또는 no를 클릭한다.

## 1) 데이터 셋 불러오기

```R
# 1)데이터 가져오기
product <- read.csv("product.csv", header = TRUE)
```

회귀분석에 사용된 데이터 셋(product.csv)의 내용

| 제품_친밀도  | 제품_적절성  | 제품_만족도 |
|:---------|:----------|:----------|
| 3 | 4 | 3 |
| 3 | 4 | 2 |
| 4 | 4 | 4 |
|...|...|...|
|==========|===========|===========|

## 2) 회귀모델 생성

```R
# 독립변수와 종속변수 생성
y = product$제품_만족도
x = product$제품_적절성
df <- data.frame(x, y)

# 단순 선형회귀 모델 생성
result.lm <- lm(formula = y ~ x, data = df)
```
제품 만족도와 제품 적절성 항목을 데이터 프레임으로 변환하고,  
회귀분석 모델을 생성한다.


## 3) 회귀방정식
모델을 실행하여 회귀방정식을 세운다.
```R
# 회귀분석의 절편과 기울기
> result.lm
Call:
lm(formula = y ~ x, data = df)

Coefficients:
(Intercept)            x  
     0.7789       0.7393  
```
Coefficients를 보면 절편이 0.7789 x 값이 0.7393이 나왔으므로  
회귀방정식은  
Y = 0.7789 + 0.7393 이 된다.

## 4) 모델 적합값
관측값과 적합값을 비교해본다.
```R
# 적합값 보기
> names(result.lm)
> fitted.values(result.lm)[1:2]
       1        2 
3.735963 2.996687  

# 관측값 보기
> head(df, 2)
  x y
1 4 3
2 3 2

# 모델의 잔차 보기
> residuals(result.lm)[1:2]
         1          2 
-0.7359630 -0.9966869 

# 잔차 직접 계산 하는 방법
> 3 - 3.735963
[1] -0.735963

# 모델의 잔차와 회귀방정식에 의한 적합값으로부터 관측값 계산
> -0.7359630 + 3.735963
[1] 3
```

## 5) 결과
```R
> # 회귀분석 결과보기
> summary(result.lm)

Call:
lm(formula = y ~ x, data = product)

Residuals:
     Min       1Q   Median       3Q      Max 
-1.99669 -0.25741  0.00331  0.26404  1.26404 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  0.77886    0.12416   6.273 1.45e-09 ***
x            0.73928    0.03823  19.340  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.5329 on 262 degrees of freedom
Multiple R-squared:  0.5881,	Adjusted R-squared:  0.5865 
F-statistic:   374 on 1 and 262 DF,  p-value: < 2.2e-16
```
주어진 데이터 셋에서 제품 만족도를 종속변수 제품 적절성을 독립변수로 설정하고  
단순선형 회귀분석을 실시한 결과이다.  

**잔차**의 최소값은 -1.99669이고, 최대값은 1.26404가 나왔다.

**회귀방정식**은 절편이 0.77886, x값은 0.73928이 나왔으므로,
y = 0.77886 + 0.73928(제품 적절성)이 된다.
따라서, 0.7789 + 0.7393(제품 적절성)만큼 증감할 때 제품 만족도가 1씩 증감했음을 알 수 있다.  

예를들어, 제품 적절성이 5의 평가를 받았다면  
0.7789 + 0.7393 * 5 = 4.4754 이므로  
제품 만족도는 4.4754의 평가를 받게 됨을 예측할 수 있다.

**Multiple R-squared**는 반응 변수와 예측 변수 사이의 다중 상관관계를 나타낸다.

**Adjusted R-squared**는 수정 결정계수로 0.5865가 나왔는데,  
이 모델로 해당 데이터셋의 58%만큼을 설명할 수 있다는 뜻이다.  
즉, 나머지 42%의 데이터는 해당모델로 설명하기 어려우므로  
예측력이 다소 떨어지는 모델이라고 볼 수 있다.




*[잔차]: 관측치와 예측치의 차
*[최소자승법]: 잔차들의 제곱의 합이 최소가 되도록 정하는 방법
*[독립변수]: 종속변수를 설명하기 위하여 사용되는 변수
*[종속변수]: 독립변수의 영향을 받아 추정되는 값을 가진 변수
