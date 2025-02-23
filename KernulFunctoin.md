# Kernul Function, 커널 함수

## 1. What is Kernul Function
- 커널 함수는 주로 기계 학습에서, 특히 SVM 과 같은 알고리즘에서 사용되는 개념이다.
- 커널 함수의 주요 역할은 데이터를 고차원 공간으로 매핑하여 선형적으로 구분할 수 있도록 하는 것이다.
- 이를 통해 복잡한 비선형 문제를 해결할 수 있다.
- 커널 함수는 기계 학습에서 핵심적인 기술로, 고차원 공간에서 데이터의 관계를 파악하고
  복잡한 문제를 효율적으로 해결하는데 크게 기여한다.

### 1-1. 커널 함수의 개념
#### 고차원 매핑
- 커널 함수는 입력 데이터를 저차원 공간에서 고차원 공간으로 매핑한다.
- 이 매핑은 명시적으로 수행될 필요가 없으며, 커널 트릭이라는 기법을 통해 암묵적으로 이뤄진다.

#### 커널 트릭
- 커널 트릭은 데이터를 고차원 공간으로 매핑하지 않고도 고차원 내적(inner product)을 계산할 수 있게 해주는 기법이다.
- 이를 통해 계산 비용을 크게 줄일 수 있다. SVM 에서는 데이터의 내적 연산만을 필요로 하기 때문에,
  커널 트릭을 사용하면 실제로 데이터를 고차원 공간으로 매핑하지 않고도 효과적으로 분류할 수 있다.
####
- 선형 커널 (Linear Kernul)
  - 정의:
  - 용도: 데이터가 이미 선형적으로 구분 가능한 경우에 사용    
- 다항 커널 (Polynomial Kernel)
  - 정의:
  - 용도: 데이터의 복잡한 다항식 경계면을 학습할 때 사용
- 가우시안 커널 (Radial Basis Function, RBF)
  - 정의:
  - 용도: 비선형 경계가 필요한 경우 널리 사용, RBF 커널은 데이터 포인트 주변에 가우시안 분포를 형성하여
    국소적인 영향을 준다.
- 시그모이드 커널 (Sigmoid Kernel)
  - 정의:
  - 용도: 인공 신경망의 활성 함수와 유사한 역할을 하며, 신경망 모델과의 연관성을 가진다.
  
### 1-2. 커널 함수의 활용
- 비선형 데이터 처리: 커널 함수를 사용하면 선형적으로 구분할 수 없는 데이터도 고차원 공간에서 선형적으로 구분할 수 있게 되어,
  복잡한 분류 문제를 해결할 수 있다.
- SVM 의 확장: 커널 함수는 SVM이 단순한 선형 분리기를 넘어 복잡한 분류 경계를 만들 수 있도록 확장시켜 준다.
