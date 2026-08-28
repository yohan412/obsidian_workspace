
## What is Generalization?

 **일반화란?**
- 단순히 훈련 데이터에 잘 맞는 모델을 만드는 것에서 그치지 않고,
- **보지 못한 새로운 데이터(unseen data)에 대해서도 정확한 예측**을 할 수 있도록 모델을 구성하는 것


## 오버피팅과 일반화 사이의 관계

![](https://i.imgur.com/yAifv8y.png)

### 그래프 해석:
- **X축:** 모델의 복잡도(Model complexity)
- **Y축:** 오류(Error)

### 곡선 설명:
- **Training error:** 모델 복잡도가 높아질수록 줄어듦
- **Test error:** 처음엔 감소하다가 복잡도가 너무 높아지면 오히려 **증가** (오버피팅)
- ✔️ **Optimal Point:** 테스트 에러가 최소가 되는 지점 → **일반화가 가장 잘된 모델**

## 복잡도와 일반화의 트레이드오프

> "모델이 너무 복잡하면 훈련 데이터는 잘 맞추지만, 일반화 성능은 떨어짐"
- 복잡도가 높을수록 **더 많은 파라미터**를 가지며, **비선형 관계도 잘 표현**하지만,
- 그만큼 **오버피팅 위험**도 증가함

## How to Achieve Generalization?

### 목표

- **테스트 데이터에 대한 정확한 예측**(accurate predictions for test data)을 통해 **좋은 일반화 성능**을 얻는 것

### 주의점

- 훈련 데이터에서 **오차 함수(error function)** 를 최소화하는 파라미터를 선택하는 것이 항상 최선은 아님  
    → 과적합(overfitting) 가능성 존재


## 핵심 전략

### **진짜 패턴(true regularities)** 은 모델링하고 **잡음(noise)** 은 무시해야 함

- 그러나 현실적으로는 **진짜 규칙과 우연한 패턴(accidental)** 을 구별하는 것이 어렵다.


## **Increasing the Size of Data (데이터 양 증가)**


> “When the model complexity is fixed, the overfitting problem decreases as the amount of data increases.”

- **모델 복잡도(model complexity)가 고정된 상태에서**,  
    **데이터의 양이 많아질수록 과적합(overfitting) 문제는 줄어든다.**
- 즉, 충분한 데이터를 확보하면 모델이 훈련 데이터에만 특화되지 않고 **일반화 성능이 향상**됨.

> “It can be incomplete or inaccurate to assess model complexity solely based on the number of parameters.”

- **파라미터 수만으로 모델 복잡도를 평가하는 것은 불완전하거나 부정확할 수 있음**
- 복잡도는:
    - 파라미터 수뿐만 아니라,
    - **비선형성**, **표현력**, **모델 구조** 등도 함께 고려해야 함

> “For a given model complexity, the overfitting problem becomes less severe as the data size increases.”

- **모델 복잡도는 동일**하게 유지하더라도,
- **데이터의 수 (N)** 를 늘리면 과적합이 완화됨

---

![](https://i.imgur.com/Lfmc5pa.png)
#### 두 모델 모두 9차 다항식(9th-order polynomial)을 사용

- 왼쪽: N = 15 (적은 데이터)
    - 데이터 포인트가 적고, 학습 곡선(보라색)이 실제 패턴(녹색)을 따라가지 못하고 **진동적으로 과적합**되어 있음
- 오른쪽: N = 100 (많은 데이터)
    - 데이터가 많아짐에 따라 학습 곡선이 더 매끄럽게 **실제 분포를 근사**하게 됨
    - 과적합이 줄어들고 일반화가 개선됨

### 목적: 모델 복잡도에 패널티를 부여하여 최적 파라미터 찾기

#### 일반 수식 표현 :
$$
\hat{\theta} = \arg\min_{\theta} \left( \frac{1}{n} \sum_{i=1}^{n} L(f(x^{(i)}), y^{(i)}) + \lambda \Omega(\theta) \right)
$$
- **$L(θ)$** : 예측값과 실제값 사이의 **오차 (loss)**
- **$Ω(θ)$** : 모델 복잡도를 측정하는 함수
- **$λ$** : 정규화 강도 (**regularization strength**)

### 정규화 함수 예시

#### Ridge (L2 정규화)
$$
\Omega(\theta) = \sum_j \theta_j^2
$$
- **희소성(sparsity)를 유도하지 않음**:
    - 파라미터들을 0으로 만들지 않고, 대신 **작지만 0이 아닌 값**을 유지
- **관련된 피처들에 가중치를 고르게 분산**시켜 **강건함(robustness)** 증가
- **해석적 해(Analytic solution)가 존재**:
    - 선형대수 기법으로 쉽게 계산 가능
    - 계산 효율이 좋음

#### Lasso (L1 정규화)
$$
\Omega(\theta) = \sum_j |\theta_j|
$$
- **희소성(sparsity)** 유도:  
	- $a_i = 0$이 되도록 유도하여 **불필요한 피처 제거**
- **가장 정보가 많은 피처**만 선택하게 됨 → **변수 선택 기능 내장**
- **해석적 해(analytic solution)가 없음**:  
	- $x=0$에서 **절댓값 함수는 미분 불가능**하기 때문에, **수치적 최적화(numerical methods)**로 해를 구함

#### How should we define $Ω(θ)$?
- 문제의 특성과 목적에 따라 **Ridge (L2)** 또는 **Lasso (L1)** 선택
- 예: 변수 선택이 중요하면 Lasso, 수치 안정성 원하면 Ridge
#### How should we define $λ$?
- 교차 검증(Cross-validation) 등으로 **최적의 정규화 강도** 탐색

### 핵심 요약

- 정규화는 **오버피팅 방지**의 핵심 기법
- 모델이 복잡해질수록 $Ω(θ)$를 통해 **패널티 부여**
- Ridge는 **가중치를 작게 유지**, Lasso는 **불필요한 변수 제거**
- 결과적으로 **단순하고 일반화 가능한 모델**을 만드는 데 기여함

## Regularization: 모델 단순화를 통한 일반화 성능 향상

>  **"While the prediction error may slightly increase, simplifying the model helps improve generalization to unseen data."**

### 목적

- 모델의 복잡도를 줄여 **과적합을 방지**
- 일반화(generalization)를 향상
- 규제(regularization)를 통해 불필요한 가중치를 억제함

### 잔차제곱합 (RSS: Residual Sum of Squares)
$$
RSS(w) = \sum_{i=1}^{n} (y_i - \hat{y}_i)^2 = \sum_{i=1}^{n} (y_i - (w^T x_i + b))^2
$$

![](https://i.imgur.com/5M8zCAy.png)
- 등고선은 RSS 값이 동일한 영역을 나타냄
- 각 제약 조건 (L1, L2, Elastic Net)의 영역과 RSS 등고선이 **최초로 접하는 지점**이 최적의 파라미터가 됨
- L1은 다이아몬드 형태의 제약, L2는 원형, Elastic Net은 그 중간

### 핵심 요약

|항목|Lasso (L1)|Ridge (L2)|Elastic Net|
|---|---|---|---|
|**희소성 유도**|✅|❌|✅ (부분)|
|**해석적 해 존재**|❌|✅|❌|
|**특징 선택 기능**|강함|약함|중간|
|**계산 복잡도**|중|낮음|중|
|**권장 사용 상황**|불필요한 피처 많을 때|피처 간 상관관계 존재 시|혼합 특성 필요 시|

![](https://i.imgur.com/ur6BxQx.png)
#### $λ=0$ : **Overfitting (과적합)**
- 훈련 오차(E_train)는 매우 낮음
- 테스트 오차(E_test)는 매우 높음
- 모델이 훈련 데이터에만 최적화됨

#### $λ→∞$ : **Underfitting (과소적합)**
- 너무 큰 패널티 → 대부분의 가중치가 0으로 수렴
- 모델이 충분히 학습하지 못함 → 훈련 및 테스트 성능 모두 저조

#### 중간값 $λ$ : **Optimal Generalization (일반화 최적점)**
- 훈련 오차와 테스트 오차가 균형을 이룸
- 오버피팅 방지 + 충분한 표현력 확보