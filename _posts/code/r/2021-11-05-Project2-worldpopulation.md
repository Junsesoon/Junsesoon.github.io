---
layout: post
title: 2021-11-05-Project2-worldpopulation
#image:
#  path: /assets/img/code/r/.png
description: >
  R, 정형 데이터, 세계 인구 데이터 분석
sitemap: false
hide_last_modified: true
invert_sidebar: false
categories:
  - code
  - r
---


# 세계 인구 데이터 분석
월드 뱅크의 오픈소스 데이터를 통해,
R Language를 활용하여 2019년 기준 나라별 총 인구수를 토대로
나라별 가장 큰 도시의 인구 비율을 구하는 방법을 기술한 내용입니다.

>본 글은 R 4.0.0ver 에서 작성되었습니다.

* toc
{:toc .large-only}

## 데이터 수집
### 1) 월드뱅크 접속
<img src="/assets/img/code/r/world-population/1.jpg" width="800"/><br>

The World Bank 홈페이지에서 UNDERSTANDING POVERTY > OPEN DATA 카테고리로 들어가면<br>
다양한 공공데이터를 찾아볼 수 있다.

### 2) 데이터 검색
<img src="/assets/img/code/r/world-population/2.jpg" width="800"/><br>

국가별 총인구를 찾기 위해 population을 검색해보면 다음과 관련된 검색어들을 같이 확인할 수 있다.

### 3) 데이터 뱅크 이동
<img src="/assets/img/code/r/world-population/3.jpg" width="800"/><br>

들어가보면 바로 csv 또는 엑셀파일로 다운로드 받을 수 있는 버튼이 있지만 <br>
우리가 원하는 데이터를 선택해서 다운받기 위해 데이터 뱅크로 이동한다.

### 4) 국가 지정
<img src="/assets/img/code/r/world-population/4.jpg" width="800"/><br>

첫번째 옵션으로는 Countres 이다. 위와 같이 Country 항목에서 Countres를 체크해주지 않으면<br>
각종 기구나 연합들의 데이터가 섞일 수 있다.

### 5) 연도 선택
<img src="/assets/img/code/r/world-population/5.jpg" width="800"/><br>

두번째 옵션은 연도 지정이다. 우리는 2019년의 데이터만 다룰 예정이기 때문에 나머지 연도는 체크 해제하고<br>
Apply Changes버튼을 눌러보면 필터링 된 데이터를 볼 수 있다.

### 6) 다운로드
<img src="/assets/img/code/r/world-population/6.jpg" width="800"/><br>

마지막으로, 적용된 결과를 확인하고 CSV파일을 다운로드 해준다.

> 도시별 가장 큰 인구는 population in largest city 로 검색해서 위와 같은 과정을 반복한다.


## 1.각 나라별 가장 큰 도시에 거주중인 인구의 비율

* 계산과정

```r
country_name <-list()
pop_ratio <-list()
for (i in 1:length(country$Country.Name)) {
  if(country[i,2] > city[i,2]){

    # 각 나라별 인구비율 계산
    calc <- as.numeric(city[i,2]) / as.numeric(country[i,2]) * 100

    # 각 변수에 나라이름과 계산된 인구비율을 저장
    country_name <- append(country[i,1], country_name)
    pop_ratio <- append(calc, pop_ratio)

  }
}

# 인구비율 계산 결과 도출
new_country <- unlist(country_name)
new_pop_ratio <- unlist(pop_ratio)
result <- data.frame(new_country, new_pop_ratio)
result
```

계산조건)<br>
총인구가 도시의 인구보다 많을 경우 도시의 인구를 총인구로 나눈 백분위 비율을 차례대로 저장한다.
> 도시의 인구가 국가 총인구보다 많을 수 없기 때문에 잘못 입력된 값을 1차적으로 걸러준다.

* 출력내용

```r
##                        new_country new_pop_ratio
## 1                      Yemen, Rep.      9.856597
## 2            Virgin Islands (U.S.)            NA
## 3                          Vietnam      8.678463
## 4                          Vanuatu            NA
## 5                       Uzbekistan      7.416045
## 6                          Uruguay     50.399641
## 7                    United States      5.727385
## 8             United Arab Emirates     28.996177
## 9                          Ukraine      6.698782
## 10                          Uganda      7.087688

                        (중략)

## 127                       Barbados            NA
## 128                   Bahamas, The            NA
## 129                        Austria     21.569316
## 130                          Aruba            NA
## 131                        Armenia     36.626864
## 132                      Argentina     33.506241
## 133            Antigua and Barbuda            NA
## 134                        Andorra            NA
## 135                 American Samoa            NA
## 136                        Algeria      6.339446
```
<mark>결과해석</mark><br>
총 136개 국가의 도시 인구 밀집 비율이 도출됐다.
>예멘의 경우 약 9.85%의 인구가 예멘에서 가장 큰 도시에 거주중인 것을 알 수 있으며,<br>
NA값으로 나온 곳은 가장 큰 도시의 인구수 데이터를 가지고 있지 않은 국가이다.

## 2.인구 비율이 가장 높은 나라 20개국

```r
question2 <- result[order(-result$new_pop_ratio),]
answer2 <- head(question2,20)
answer2
```
```r
##              new_country new_pop_ratio
## 89  Hong Kong SAR, China      99.77857
## 39           Puerto Rico      76.75673
## 78                Kuwait      72.55615
## 106             Djibouti      58.42534
## 6                Uruguay      50.39964
## 61              Mongolia      48.14183
## 42              Paraguay      46.54830
## 83                Israel      45.25031
## 44                Panama      42.89930
## 111          Congo, Rep.      42.89702
## 131              Armenia      36.62686
## 75               Lebanon      35.10760
## 132            Argentina      33.50624
## 41                  Peru      32.46559
## 53           New Zealand      31.77210
## 47                  Oman      30.18367
## 73               Liberia      29.71268
## 8   United Arab Emirates      28.99618
## 97               Georgia      28.95966
## 65            Mauritania      27.81832
```

인구비율이 가장 높은 국가를 살펴보면 <strong>1위는 홍콩, 2위는 푸에르토리코, 3위는 쿠웨이트</strong> 순으로 도출됐다.

> 특이하게도 홍콩은 99.8%라는 수치가 나왔는데, 이는 홍콩이 중국의 특별행정구로 속해있으나<br>
일국양제라는 특별한 방식에 의해 중국과는 독립적으로 운영되어 도시국가의 형태를 띄기 때문인 것으로 보인다.

## 3.인구 비율이 가장 낮은 나라 20개국
```r
question3 <- result[order(result$new_pop_ratio),]
answer3 <- head(question3,20)
answer3
```

```r
##            new_country new_pop_ratio
## 86           Indonesia      3.931147
## 43    Papua New Guinea      4.267456
## 96             Germany      4.280497
## 40              Poland      4.677758
## 56               Nepal      4.810101
## 51               Niger      5.368822
## 59          Mozambique      5.495415
## 7        United States      5.727385
## 69              Malawi      5.769013
## 136            Algeria      6.339446
## 55         Netherlands      6.574501
## 9              Ukraine      6.698782
## 50             Nigeria      6.918477
## 10              Uganda      7.087688
## 82               Italy      7.088706
## 46            Pakistan      7.268664
## 5           Uzbekistan      7.416045
## 28     Slovak Republic      7.929893
## 116               Chad      8.600578
## 37  Russian Federation      8.639633
```

반면, 인구비율이 가장 낮은 국가는 <strong>1위 인도네시아, 2위 파푸아 뉴기니, 3위 독일</strong> 순으로 도출됐다.

## 4.결측치 확인
```r
answer4 <- result %>% filter(is.na(result$new_pop_ratio))
answer4
```
```r
##                       new_country new_pop_ratio
## 1           Virgin Islands (U.S.)            NA
## 2                         Vanuatu            NA
## 3                          Tuvalu            NA
## 4        Turks and Caicos Islands            NA
## 5                           Tonga            NA
## 6                     Timor-Leste            NA
## 7                        Suriname            NA
## 8  St. Vincent and the Grenadines            NA
## 9        St. Martin (French part)            NA
## 10                      St. Lucia            NA
## 11            St. Kitts and Nevis            NA
## 12                Solomon Islands            NA
## 13                       Slovenia            NA
## 14      Sint Maarten (Dutch part)            NA
## 15                     Seychelles            NA
## 16          Sao Tome and Principe            NA
... 생략
```

인구 밀집 비율을 계산할 수 없는 나라의 목록이다.
<br><br><br>

이상으로 월드뱅크 공공 데이터를 활용한 데이터 핸들링 포스팅을 마칩니다. 감사합니다.👏
