# 국내 관광산업의 비주류 지역 기반 소멸 위기 지역 선정 및 예방 정책 제안

## 📋 프로젝트 개요

본 프로젝트는 한국의 저출산으로 인한 인구감소와 대도시 인구 집중으로 지방의 소멸 위기에 대응하기 위해, 관광 데이터를 활용하여 관광객 소멸 위기 지역을 선정하고 예방 정책을 제안하는 것을 목적으로 합니다.

**작성자**: 이유진 (국민대학교)  
**제출일**: 2024.07.16  
**이메일**: iamlyj03@kookmin.ac.kr

## 🎯 연구 목적

- 지역별 관광객 수에 영향을 미치는 변수 분석
- 관광객 소멸 위기 지역 선정
- 지역 소멸 위기 예방을 위한 정책 제안

## 📊 사용 데이터

본 프로젝트는 2023년 기준으로 수집된 다음 데이터를 활용합니다. (수도권 제외)

1. **지역별 방문자 수**: 각 지역에서 발생한 방문자 수 기록
   - 출처: [한국관광공사 데이터랩](https://datalab.visitkorea.or.kr/datalab/portal/bda/getMetcoAna.do)

2. **지역별 관광지 검색건수**: 온라인 검색 활동을 통한 관심도 파악
   - 출처: [한국관광공사 데이터랩](https://datalab.visitkorea.or.kr/datalab/portal/bda/getDomInqCnt.do)

3. **모바일 인구 이동량**: 관광객의 출발지와 이동 동향 분석
   - 출처: [통계청 나우캐스트](https://data.kostat.go.kr/nowcast/main.do?initId=19)

4. **지역별 신용카드 지출액**: 관광 지출 패턴 분석
   - 출처: [한국관광공사 데이터랩](https://datalab.visitkorea.or.kr/datalab/portal/bda/getByLocgoCnsmAmt.do)

5. **지역별 숙박업소 개수**: 관광 인프라 수준 평가
   - 출처: [빅데이터 플랫폼](https://www.bigdata-culture.kr/bigdata/user/data_market/detail.do?id=81ae75f0-497f-11ea-8aab-85ba209ddf37)

6. **지역별 SNS 언급량**: 각 지역의 SNS 언급 정도 및 소비자 관심도
   - 출처: [한국관광공사 데이터랩](https://datalab.visitkorea.or.kr/datalab/portal/loc/getAreaDataForm.do#)

7. **관광명소 이름과 위치 데이터**: 지역별 관광 리소스 분석
   - 출처: [빅데이터 플랫폼](https://www.bigdata-culture.kr/bigdata/user/data_market/detail.do?id=1ec19f06-035e-49f3-8c5d-ff8d2d2829a7)

## 🔍 분석 방법론

### 1. 상관분석
- **목적**: 지역별 방문자 수와 상관관계가 있는 변수 판별
- **종속변수**: 방문자 수 (전체 방문자)
- **독립변수**: 검색 건수, SNS 언급량, 관외 이동량, 숙박업소 개수 등
- **결과**: 
  - 검색 건수, SNS 언급량, 관외 이동량 → 강한 양의 상관관계
  - 숙박업소 개수 → 약한 양의 상관관계

### 2. K-Means 클러스터링
- **피처**: 검색 건수, SNS 언급량, 관외 이동량, 숙박업소 개수
- **가중치**: 방문자 수에 2배 가중치 부여
- **클러스터 수**: 2개 (주류 지역 / 비주류 지역)
- **시각화**: GeoJSON 데이터를 이용한 지도 시각화

**주류 지역 예시**: 강릉시, 속초시, 원주시, 춘천시, 거제시, 양산시, 경주시, 제주시 등

### 3. 예측 모델 구축

#### 3.1 모델 비교
다음 3가지 회귀 기법을 비교 분석했습니다:

- **LSTM**: 시계열 데이터 예측
- **Random Forest**: 앙상블 기법
- **XGBoost**: 그래디언트 부스팅

#### 3.2 최종 모델: XGBoost (Grid Search)
- **성능 지표**:
  - MSE: 433,796,369,727.71
  - MAE: 407,997.67
  - R²: 0.9611
  - Accuracy: 96.11%

- **하이퍼파라미터 최적화**:
  - `n_estimators`: 100, 500, 1000
  - `max_depth`: 3, 5, 7
  - `learning_rate`: 0.01, 0.05, 0.1
  - `subsample`: 0.6, 0.8, 1.0

### 4. ARIMA 모델을 활용한 미래 예측
- 2025년도 각 변수값 예측
- XGBoost 모델을 통한 2025년도 지역별 방문자 수 예측
- 2023년, 2024년, 2025년 1월 기준 방문자 수 감소율 측정

## 📁 프로젝트 구조

```
Statistical-Data-Analysis-Project-main/
├── README.md
├── correlation.ipynb                          # 상관관계 분석
├── 상관관계 분석.ipynb                        # 상관관계 분석 (한글명)
├── 통계청_클러스터링.ipynb                    # K-Means 클러스터링
├── 통계청_클러스터링_시각화추가.ipynb          # 클러스터링 시각화
├── 통계청_Random_Forest.ipynb                 # Random Forest 모델
├── new_LSTM_1.ipynb                           # LSTM 모델 (개선 1)
├── re_LSTM_2.ipynb                            # LSTM 모델 (개선 2)
├── ML_XGBoost - 모델생성 및 예측.ipynb         # XGBoost 모델 생성 및 예측
├── ML_XGBoost - 2024년 비교.ipynb             # XGBoost 2024년 예측 비교
├── hotel_df.csv                               # 숙박업소 데이터
├── move_df.csv                                # 인구 이동량 데이터
├── hotels_predict.csv                         # 숙박업소 예측 데이터
├── move_predict.csv                           # 인구 이동량 예측 데이터
├── searches_predict.csv                       # 검색건수 예측 데이터
├── sns_predict.csv                           # SNS 언급량 예측 데이터
├── visitor_predict.csv                        # 방문자 수 예측 데이터
├── merged_predict.csv                         # 통합 예측 데이터
├── merged_predict2.csv                        # 통합 예측 데이터 (개선)
├── real_predict.csv                           # 실제 예측 결과
└── 국내 관광산업의 비주류 지역 기반 소멸 위기 지역 선정 및 예방 정책 제안 보고서.hwpx
```

## 🚀 설치 및 실행 방법

### 필수 라이브러리

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost tensorflow keras statsmodels folium geopandas
```

### 실행 순서

1. **데이터 전처리 및 상관분석**
   ```bash
   jupyter notebook correlation.ipynb
   # 또는
   jupyter notebook 상관관계\ 분석.ipynb
   ```

2. **클러스터링 분석**
   ```bash
   jupyter notebook 통계청_클러스터링.ipynb
   ```

3. **예측 모델 구축**
   ```bash
   # XGBoost 모델 (최종 모델)
   jupyter notebook ML_XGBoost\ -\ 모델생성\ 및\ 예측.ipynb
   
   # Random Forest 모델
   jupyter notebook 통계청_Random_Forest.ipynb
   
   # LSTM 모델
   jupyter notebook new_LSTM_1.ipynb
   ```

4. **예측 결과 비교**
   ```bash
   jupyter notebook ML_XGBoost\ -\ 2024년\ 비교.ipynb
   ```

## 📈 주요 결과

### 1. 모델 성능 비교

| 모델 | MSE | MAE | R² | Accuracy |
|------|-----|-----|----|----------|
| LSTM (개선 1) | 8,922,515,069,093.09 | 2,256,638.89 | 0.1622 | 15.58% |
| Random Forest | 515,798,428,050.43 | 437,062.69 | 0.9538 | 95.38% |
| **XGBoost (Grid Search)** | **433,796,369,727.71** | **407,997.67** | **0.9611** | **96.11%** |

### 2. 2024년 예측 정확도

월별 오차 평균값:
- 1월: 643,232.75
- 2월: 603,897.88
- 3월: 354,379.94
- 4월: 344,070.74

### 3. 소멸 위기 지역 선정

2025년 예측 결과를 바탕으로 방문자 수 감소율이 가장 높은 5개 지역:

1. **담양군**
2. **울릉군**
3. **군위군**
4. **장성군**
5. **공주시**

이 중 4개 지역은 클러스터링 분석에서 비주류 지역으로 선정되었습니다.

## 💡 정책 제안

### 기대효과
- 소멸 위기 지역 공표를 통한 위기의식 환기
- 지역별 맞춤형 관광 상품 개발 및 홍보 전략 수립
- 지역 경제 활성화를 통한 소멸 위기 극복

### 방향 제시
- 관계 부처 및 지자체에 소멸 위기 지역 공식 공표
- 지역 특성을 고려한 맞춤형 정책 시행 및 예산 지원
- 주민 대상 소멸 위기 심각성 알림 및 인구 유입 정책 제안

## 📚 참고문헌

1. 이선구, 유선종 (2024). XGBOOST 모형을 활용한 부동산 조각투자 가격 예측: 카사TE 물류센터를 중심으로. 6, 15-16

2. 오재영 외 3인 (2019). XGBoost 기법을 이용한 단기 전력 수요 예측 및 하이퍼파라미터 변화에 따른 영향 분석. 2-3

## 📝 라이선스

본 프로젝트는 2024년 통계데이터 활용대회 참가작입니다.

---

**Note**: 본 프로젝트는 수도권(서울, 경기, 인천)을 제외한 지역을 대상으로 분석을 진행했습니다.

