# Seoul's (Seoul, South Korea) Land Price Prediction

Entire Report in Korean : https://github.com/GullyMac/seoul_land_price/blob/master/report_korean.pdf

20.01.18 last edit

---

## 0. Introduction

There are many features that determine land price.

In particular, since 'home prices' is very sensitive issue in Korean society,
the interest in features that determine land price is always hot.

For example, recently, Gangseo-gu (in Seoul, South Korea) has been in conflict with parents of students with disabilities, 
arguing that residents opposing the construction of special schools lower land prices and housing prices.

Therefore, the distribution of major facilities and infrastructure 
(such as special schools, large hospitals, bar or pub, lodging houses, and large supermarkets) 
are selected as potential variables that can affect land prices.

We tested whether those variables are actually related to land prices.

In addition, we tried to predict land prices using various machine learning algorithms 
and make a prediction model with small errors.

---

## 1. Data Set

#### Response Variable : 2017년 서울특별시 법정동별 개별공시지가

* Source

  서울 열린데이터 광장 - 서울시 개별공시지가 정보 (https://data.seoul.go.kr/openinf/fileview.jsp?infId=OA-1180)
  
  
* Definition

  개별공시지가는 표준지공시지가를 이용하여 산정한 개별토지의 단위면적당 가격이다.
  
  
* Preprocessing
  
  단위면적당 가격이기 때문에 같은 법정동 내에 여러 건의 토지 가격 정보가 포함되어 있었다.
  
  따라서 행정구와 법정동을 기준으로 평균을 내어 해당 구 또는 동의 지가를 계산하였다.
  
  
#### Explanatory Variable : 동별 대형마트, 특수학교, 대형병원, 유흥·단란주점, 숙박업소, 학교 개수

* Source

  서울 열린데이터 광장
  
* Preprocessing

  업체 주소 자료로부터 행정구역 정보(자치구명, 법정동명)를 추출하였다.
  
  주소 자료에 법정동명이 아닌 행정동명이 입력되어있는 경우 법정동명으로 변경 
  
  결측값는 네이버 주소검색 이용하여 채움
  
  !행정동이 아닌 법정동 정보 사용
  
  (법정동: 옛 지명 등에서 유래된 이름을 사용하여 법으로 정한 동으로 모든 정부 기관의 공무나 재산권 및 권리행사 등의 법률행위에 사용되는 동. 변동이 거의 없다.
  
  행정동: 행정 운영의 편의를 위하여 설정한 행정구역으로 도시의 확장, 인구의 이동 등 지역여건 변화와 주민 수의 증감에 따라 수시로 설치 또는 폐지할 수 있는 동)
  
  현재 서울특별시에는 총 25개 구, 467개의 법정동이 존재하며, 이름이 같은 동이 서로 다른 구에 있는 경우가 두 가지 존재하므로 (신사동: 강남구, 은평구 / 신정동: 양천구, 마포구) 같은 값으로 처리되지 않도록 구분하였다.


* Data Specificities
  
  유흥업소 : 유흥주점과 단란주점은 법적으로는 구분되어 있으나 ’유흥업소’라는 상위 카테고리로 묶음
  
  유흥업소, 대형마트, 숙박업소 : 폐업한 업소 제외
  
  대형마트 : 법적으로 매장 면적이 3000m^2 이상인 대형마트와 아울렛, 백화점
  
  대형마트 : 목록 안에 있더라도 식료품, 의류, 생활용품 종합 판매점이 아니면 삭제 (예: 세운상가 등 전자상가, GS슈퍼 등 슈퍼형 마트)
  
  대형마트 : 홈페이지의 점포 개수와 비교, 목록에 없으면 추가
  
  대형마트, 백화점 : 마트와 백화점이 붙어있는 경우 각각 따로 집계
  
  아울렛 : 아울렛 내의 식료품매장은 아울렛에 포함하여 하나로 집계
  
  !녹지 및 공원도 설명변수로 사용하려 하였으나 제외함. 면적, 용도, 관리 실태가 천차만별이기 때문


* Data Transformation 
  
  숙박업소, 유흥업소, 학교 : 동별 개수가 면적의 영향을 받을 것이므로 개수/면적, 1m^2 -> 1km^2당 개수로 변환
  
  대형마트, 대형병원, 특수학교 : 개수가 적고 좁은 구역에 집중되어 있는 경향이 있으므로 0 또는 1의 바이너리 변수로 변환
    
---

## 2. Library

#### used library list

* tree : Regression Tree Model
* rpart : Recursive Partitioning and Regression Trees
* class : k-Nearest Neighbour Classification, Regression
* kknn : Weighted k-Nearest Neighbors Classification, Regression
* ipred : Bagging Classification, Regression
* mboost : Generalized Linear Model By Likelihood Based Boosting
* randomForest : Random Forests for Classification and Regression
  
