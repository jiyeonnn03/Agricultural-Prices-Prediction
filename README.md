# 농산물 가격 예측 모델 성능 비교

## Model
- XGBoost
- LightGBM
- RNN
- LSTM

## 분석 Flow
### 1. 데이터 수집
  -  나라지표, 기상청, 공공데이터포털, 농넷 제공 데이터 병합
    ![image](https://user-images.githubusercontent.com/49242144/163016959-7e922175-cbab-4a95-b1f4-79cc6a522809.png)

### 2. 데이터 전처리
  - 이상치 제거(하루 전/후 가격 차이가 매우 큰 데이터를 이상치라 판단하여 제거)
  - 결측값 평균 대체(주말/공휴일 등 거래가 없는 날, 하루 전/후 평균 가격으로 대체)
  - 정규화(MinMaxScaler)
  - 데이터 분할(window size = 7)
  - 최종 데이터 구조
    ![image](https://user-images.githubusercontent.com/49242144/163014755-eb5d1a28-f594-4c3d-815a-1a847ff273cc.png)
      
### 3. 모델 학습
  - 하이퍼 파라미터 설정([LSTM 네트워크를 활용한 농산물 가격 예측 모델(2018.11)](https://scienceon.kisti.re.kr/commons/util/originalView.do?cn=JAKO201809469053682&oCn=JAKO201809469053682&dbt=JAKO&journal=NJOU00292001) 참고)
  ![image](https://user-images.githubusercontent.com/49242144/163014898-10f17967-2f6e-44ac-b030-9af21a935fb0.png)

### 4. 교차 검증
  - 시계열 데이터 속성을 고려해 Time Series Split 사용   
  
    ![image](https://user-images.githubusercontent.com/49242144/163015444-6512d900-5012-4241-a422-8b886458fc51.png)

### 5. 결과 해석
  - Fold별 가격 예측 결과
    ![image](https://user-images.githubusercontent.com/49242144/163015520-e67ef5b7-4a41-48a0-a58c-ac2008f4e1af.png)

  - 모델 성능 비교
    ![image](https://user-images.githubusercontent.com/49242144/163015631-0c154790-1868-4c81-aa36-d4e845ce32e7.png)

  - 모델 성능 순위
    ![image](https://user-images.githubusercontent.com/49242144/163015692-c676be5f-5e2d-4512-bbd3-93dc2717ae4e.png)

  - 모델 성능 비교 결과 RNN > LGBM > XGB > LSTM 순으로 성능이 우수함을 확인
  - XGBoost 변수 중요도 비교 결과, 유류 가격과 물가가 가격에 미치는 영향이 컸음

### 6. 한계점 및 후속 연구
  - 전체 데이터의 수가 적어 변동 요인 추세 변동 , 계절 변동 , 순환 변동 , 불규칙 변동 ) 파악 어려움
    -> 더 많은 양의 데이터셋을 수집 해 변동 요인 파악이 가능할 것으로 보임
  - 하루 사이 가격 변동 이 크게는 몇천원까지 발생하기도 함
    -> 가격을 1000 원 단위로 라벨링하여 회귀 문제로 접근하지 않고 분류 문제로 접근해보는 것도 흥미로운 결과를 도출해낼 수 있을 것으로 보임
  - 딥러닝 모델에서 가격에 영향을 미치는 주요인 파악의 어려움
    -> 모델에서 변수 중요도를 제공하지 않는 경우, XAI 모델을 통해 각 변수의 영향력 정도를 파악할 수 있을 것으로 보임
  - 앙상블 을 통한 더 나은 모델
    -> 성능이 좋은 부스팅 모델 + 딥러닝 모델을 앙상블 하여 새로운 결과를 도출
