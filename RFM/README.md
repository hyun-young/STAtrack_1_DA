# RFM Segmentation

## 프로젝트 개요

이 프로젝트는 이커머스 고객 데이터를 기반으로 RFM(Recency, Frequency, Monetary) 분석을 수행하여 고객을 다양한 세그먼트로 분류한다. 실제 구매 이력 데이터를 SQL과 Pandas로 전처리하고, RFM 지표 산출, 점수화, 세그먼트 분류, 시각화, 기존 등급과의 비교까지 일련의 분석 과정을 수행한다.

## 데이터 추출 및 전처리

- Google BigQuery에서 회원, 주문, 주문상세, 상품 테이블을 JOIN하여 데이터프레임으로 추출한다.
- 주요 컬럼: user_id, order_date, quantity, price_per_unit, discount, user_type 등
- 데이터 위치: `code/rfm_segmentation.ipynb`에서 직접 쿼리 및 전처리 수행

## RFM 지표 산출

- **Recency**: 기준일(2024-05-19)로부터 고객별 마지막 주문일까지의 일수로 산출한다.
- **Frequency**: 고객별 주문 건수로 산출한다.
- **Monetary**: (단가-할인)×수량을 주문별로 계산 후, 고객별 총 구매금액을 산출한다.

## RFM 점수화 및 세그먼트 분류

- 각 지표를 사분위수(q=4)로 나누어 점수(1~4점)로 변환한다. Recency는 작을수록, Frequency/Monetary는 클수록 높은 점수를 부여한다.
- RFM 점수의 합(RFM_Score)으로 1차 세그먼트(Bronze, Silver, Gold, Platinum) 분류한다.
- R, F, M 조합에 따라 8개 세그먼트(VIP, Core, Frequent, Past Vip, At_Risk, Newbie, Inactive, General)로 2차 분류한다.
- 데이터가 적을 경우, 2차 세그먼트를 5개 그룹(최우수고객, 핵심고객, 잠재고객, 이탈위험, 비활성, 일반)으로 통합한다.

## 시각화 및 분석

- 각 세그먼트별 RFM 지표의 평균, 표준편차, 고객수 등 통계 요약을 산출한다.
- Plotly 3D 산점도로 R, F, M의 관계와 세그먼트별 분포를 시각화한다.
- 기존 등급(user_type)과 RFM 기반 등급의 교차표, 히트맵, 정확도(accuracy) 등 비교 분석을 수행한다.

## 주요 결과 및 결론

- RFM 기반 세그먼트와 기존 등급 간 일치율이 낮아, 기존 등급이 실제 구매 패턴을 충분히 반영하지 못함을 확인한다.
- 최종적으로 5개 그룹(최우수고객, 핵심고객, 잠재고객, 이탈위험, 비활성, 일반)으로 고객을 분류한다.
- 각 그룹별로 맞춤형 마케팅 전략 수립이 가능함을 시사한다.

## 필요 패키지

- pandas, numpy, scikit-learn, plotly, google-cloud-bigquery, pydata-google-auth 등

## 실행 방법

1. `requirements.txt`로 패키지 설치
2. `code/rfm_segmentation.ipynb` 실행
