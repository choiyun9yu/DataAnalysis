# Anomaly Detection, Novelty Detection, Outlier Detection
뉘앙스 차이 (학계에서 용어가 통일되지 않았음)
- Anomaly Detection 비정상 탐지
- Novelty Detection 신기한 것 탐지
- Outlier Detection 이상치 탐지

## 1. 학습시 비정상 데이터 사용 유무에 따른 분류 

### 1-1. Supervised Anomaly Detection
- 주어진 학습 데이터 셋에 정상 sample, 비정상 sample 그리고 label이 모두 존재하는 경우 이 방식을 사용한다.
- 지도 학습 방식이기 때문에 Supervised Anomaly Detection 이라고 한다.
- 적용 분야에 따라 정상 데이터 보다 비정상 데이터가 현저히 적은 경우 Class-Imbalance 문제가 있다.

#### 장점
- 다른 방법 대비 정확도가 높다. 

#### 단점
- Class-Imbalance 문제를 해결해야한다.
- 비정상 sample 취득하는데 시간과 비용이 많이 든다.

### 1-2. Semi-supervised (One-Class) Anomaly Detection
- Class-Imbalance 문제에 따른 Supervised Anomaly Detection의 대안 중 하나이다.
- 정상 sample 을 둘러싸는 discriminative boundary를 설정하고, 이 바운더리를 최대한 좁혀서 boundary 밖에 있는 sample을 모두 비정상으로 간주한다.

![image](https://github.com/choiyun9yu/DataAnalysis/assets/110392046/8694535f-49fe-4a0f-ab87-abb7d82e90de)

#### 장점
- 정상 smaple 만 있어도 학습이 가능하다.

#### 단점
- Supervised Anomaly Detection 방벙론 대비 영/불 판정 정확도가 떨어진다.
- 어떤 것이 정상인지에 대한 정의로 정상 sample에 대한 label이 필요하다.

### 1-3. Unsupervised Anomaly Detection
- 대부분의 데이터가 정상 smaple이라는 가정을하고 label 취득 없이 학습한다.
- 단순하게는 주어진 데이터에 대해 주성분 분석(PCA)으로 차원을 축소하고 복원하는 과정에서 비정상 sample을 검출한다.
- Neural Network 기분으로는 autoencoder 기반의 방법론이 주로 사용된다.
  - autoencoder 는 입력을 code 혹은 latent variable로 압축하는 encodeing과 이를 다시 원본과 가깝게 복원하는 decoding 과정으로 진행된다.
  - autoencoder 는 원본 데이터와 출력 데이터를 비교하여 오차가 작은 방향으로 학습 한다.
  - 이렇게 학습한 autoencoder 에 정상 sample을 넣으면 정상 sample 처럼 복원하기 대문에 input과 output의 차이가 거의 발생하지 않는다.
  - 반면 비정상 smaple을 넣으면 정상 sample에 가깝게 복원하기 때문에 input과 output의 차이가 도드라기자 발생하므로 비정상 sample을 검출할 수 있게 된다.

#### 장점
- labeling 과정이 필요하지 않다.

#### 단점
- hyper-parameter 에 따라 전반적인 복원 성능이 크게 영향을 받아 양/불 판정 정확도가 불안정하다.
- autoencoder 에 넣어주는 input 과 output 의 차이를 어떻게 정의할 것인지, 어떤 방식으로 dirrerence map 을 계산할 것인지, 어떤 loss function 을 사용해 학습할 지 등에 따라 성능 차이가 크게 달라질 수 있다. 
![image](https://github.com/choiyun9yu/DataAnalysis/assets/110392046/a35faf11-1cb9-47a6-84b7-fe2b57b188b4)

<br>

## 2. Anomaly Detection Method

### 2-1. Classifcation Based Method, 분류 기반
- sample 들을 정상과 비정상으로 labeling 할 수 있는 경우
- 정상 sample 과 비정상 sample 을 분류기가 학습하고 정상, 비정상 여부를 판단한다.
- 또는 분류기가 각 정상 class와 나머지를 구분하도록 학습하고 정상 class 에 포함되지 않는 데이터를 비정상으로 판단한다. 
- MLP, SVM, Decision rule based model

### 2-2. Nearest Neighbor Based Method
- 정상 sample 들이 어떤 근방에 밀집되어 있고, 비정상 sample 들은 근방에서 멀리 떨어져 있는 경우
- 각 객체 사이의 거리를 측정해서 '이상 score' 를 만든다.
- KNN, LOF(Local Outlier Factor)score

### 2-3. Clustering Based Method, 군집화 기반
#### 이상값 가정 1
- 정상 sample 은 하나 또는 몇 개의 군집에 모여있고 비정상 smaple은 군집에 속하지 않는 경우
- 데이터에서 군집을 찾아낸 후 제거한 뒤 남아 있는 데이터를 이상 값으로 처리한다.
- SNN 군집화, DBSAN, ROCK

#### 이상값 가정 2
- 군집의 중심(centroid) 중 가장 가까운 것과의 거리가 짧으면 정상값, 길면 이상 값으로 정의한 경우
- 군집화를 하고 데이터가 포함된 군집의 중심과 데이터 개체 사이의 거리를 '이상 socre'로 두고 계산한다.
- K-menas, EM 알고리즘

#### 이상값 가정 3
- 정상값은 크거나 조밀한 군집에, 이상 값은 작거나 sparce 한 군집에 속하는 경우
- 데이터 개체가 속한 군집의 크기나 밀도가 "이상" 여부를 판단한다.
- CBLOF(cluster-based local outlier factor)


### 2-4. Latent Variable Based Method
- (어떤 경우에 사용하는 게 좋은지)
- (사용 방법)
- PCA, Autoencoder 

### 2-5. Statistics Based Method, 통계학적 기반
- 통계적 기법에서 anomaly 는 대부분의 데이터가 가지는 확률 분포와 부분적으로 또는 완전히 동떨어졌다고 여기는 값이다.
- 주어진 자료로 모형을 적합한 뒤 통계척 추론을 통해 새로운 데이터가 그 모형을 따르는지를 판단한다.
- 검정 통계량을 바탕으로 테스트 데이터가 해당 모형으로 부터 생성되었을 확률이 낮은 값을 이상값으로 본다.

#### 모수적 기법
- 테스트 대상 데이터가 추정된 분포에서 생성되었다는 것을 귀무가설로 한다.
- 이때 가설 검정에 사용한 검정 통계량을 '이상 score'로 활용할 수 있다.
- 정규 모형 기반
  - 데이터가 정규모형에서 생성된 것으로 가정하고, 최대우도추정량을 사용한다.
  - 각 데이터와 추정된 평균값 사이의 거리가 '이상 score'가 되고, 이상 score의 경계를 정해서 이상값 여부를 판정한다.
  - 거리의 정의와 경계를 구하는 방법: 상자그림, Grubbs 검정, Mahalanobis 거리, Student t 검정, Hotelling's t 검정, 카이제곱 검정
- 회귀 모형 기반
  - 시계열 데이터에 적용하며, 데이터의 회귀모형을 적합한 뒤에 테스트 데이터와 회귀모형 간의 잔차로 '이상 score'를 구한다.
  - Robust 회귀, ARIMA 모형
- 혼잡 모형 기반
  - 데이터에 적용할 분포를 혼합하여 사용한다.
  - 정상값과 이상값에 각각 다른 분포를 적용하는 방법과 정상값에만 혼합 분포를 적용하는 방법이 있다.
  
#### 비모수적 기법 
- 데이터가 특정 모형을 따른다는 가정을 하지 않는다.
- 비모수적 기법은 실제로 데이터가 특정 분포를 따른 다는 가정이 성립하지 않을 때가 많기 때문에 현실적인 접근이 용이한 이점이 있다.
- 히스토그램 기반
- 커널 함수 기반

<br>

## 3. Anomaly Detection 적용 사례

### 3-1. Cyber-Intrusion Detection
- 컴퓨터 시스템 상에 침입을 탐지하는 사례이다.
- 주로 시계열 데이터를 다루면 RAM, file system, log file 등 일련의 시계열 데이터에 대해 이상치를 검추랗여 침입을 탐지한다.

### 3-2. Fraud Detection
- 보험, 신용, 금융 관련 데이터에서 불법 행위를 검출하는 사례이다.
- 주로 표로 나타내는 tabular 데이터를 다룬다.

### 3-3. Malware Detection
- Malware(악성코드)를 검출해내는 사례이다.
- Classification과 Clustering이 주로 사용된다.

### 3-4. Medical Anomaly Detection
- 의료 영상, 뇌파 기록 드으이 의학 데이터에 대한 이상치 탐지 사례이다.
- 주로 신호 데이터와 이미지 데이터를 다룬다.
- X-ray, CT, MRI, PET 등 다양한 장비로부터 취득된 이미지를 다루기 때문에 난이도가 높다.

### 3-5. Social Networks Anomaly Detection
- Social Network 상의 이상치들을 검출하는 사례이다.
- 주로 Text 데이터를 다루며 Text를 통해 스팸 메일, 비매너 이용자. 허위 정보 유포자 등을 검출한다.

### 3-6. Log Anomaly Detection
- 시스템이 기록한 log를 보고 실패 원인을 추적하는 사례이다.
- 주로 Text 데이터를 다루며 parttern matching 기반의 단순한 방법을 사용해 해결할 수 있지만 failure message가 새로운 것이 계속 추가, 제외되는 경우 딥러닝 기반 방법론을 사용하는 것이 효과적이다.

### 3-7. IoT Big-Data Anomaly Detection
- 사물 인터넷에 주로 사용되는 장치, 센서들로부터 생성된 데이터에 대해 이상치를 탐지하는 사례이다.
- 주로 시계열 데이터를 다루며 여러 장치들이 복합적으로 구성이 되어 있기 때문에 난이도가 높다.

### 3-8. Industrial Anomaly Detection
- 산업 속 제조업 데이터에 대한 이상치를 탐지하는 사례이다.
- 각종 제조업 도메인 이미지 데이터에 대한 외관 검사, 장비로부터 측정된 시계열 데이터를 기반으로 한 고장 예측 등 다양한 적용 사례가 있다.
- 외관상에 발생하는 결함과, 장비의 고장 등의 비정상적인 smaple이 굉장히 적은 수로 발생하지만 정확하게 예측하지 못하면 큰 손실을 유발하기 때문에 난이도가 높다.

### 3-9. Video Surveillance
- 비디오 영상에서 이상한 행동이 발생하는 것을 모니터링하는 사례이다.
- 주로 CCTV를 이용한 사례가 주를 이루며, 보행로에 자전거, 차량 등이 출현하는 비정상 sample, 지하철역에서 넘어짐, 싸움 등이 발생하는 비상정 sample 등 다양한 종류의 비정상 케이스가 존재한다.
