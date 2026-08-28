

#### **오차 최소화 목표**

- 함수 $f(x)$와 실제 값 $y$ 사이의 오차를 최소화하려면 다음과 같은 **오차 함수 $E(w)$** 를 최소화해야 합니다:
$$
E(w_1, w_2, ..., w_m) = \sum_{(x, y) \in \text{Data}} (y - f(x; w_1, w_2, ..., w_m))^2 
$$

### **오차가 최소가 되는 조건**

- $E(w)$가 최소값을 가지려면, 해당 지점에서의 **기울기(gradient)** 가 모든 방향에서 **0**이어야 합니다:
$$
\frac{\partial E}{\partial w_1} = 0, \quad \frac{\partial E}{\partial w_2} = 0, \quad \ldots, \quad \frac{\partial E}{\partial w_m} = 0
$$

→ 즉, 모든 변수에 대해 편미분한 값이 0이 되는 $w$를 찾아야 함

- 이 조건을 만족하는 $w$를 찾는 것이 바로 **오차 함수 최소화** 문제의 본질.
- 이 조건은 특히 **선형 회귀(linear regression)** 모델에 **정확하게 적용 가능**함.
- 하지만 **모든 머신러닝 모델에 적용 가능한 것은 아님**
    - 비선형 모델에서는 기울기=0이 항상 최솟값을 의미하지 않음 (예: 안장점 등)
- **해당 조건이 적용되지 않는 경우**에는 별도의 접근 방식이 필요

#### **해는 항상 존재하는가?**
- **Yes**, because the error function is **quadratic** (2차 함수 형태이기 때문)
$$
E(w_1, w_2, ..., w_m) = \sum_{(x, y) \in \text{Data}} \left(y - f(x; w_1, w_2, ..., w_m)\right)^2
$$
- 즉, **선형 회귀 모델**에서는 손실 함수가 항상 볼록(convex)하므로 최소값이 항상 존재

#### **더 복잡한 모델이면 해를 구할 수 있을까?**
- **No, we may not.**
    - 복잡한 비선형 모델의 경우 손실 함수가 **non-convex** 형태가 되며,
        - **여러 개의 로컬 최소값**
        - **안장점(saddle point)**
        - **해가 없거나 찾기 어려운 경우**가 발생할 수 있음

#### **그럼 못 푸는 경우엔 어떻게 해야 할까?**
- 일반적인 대응:
    - 수치적 최적화(gradient descent, momentum, Adam 등)
    - 정규화(regularization) 도입
    - 초기값 다양화
    - 모델 단순화 또는 구조 변경

### **해석적 해(Closed-form)로 최소값 찾기는 어렵거나 불가능**
- 많은 최적화 문제는 수학적으로 **해를 직접 구하기(open)** 어렵거나, 아예 **불가능(impossible)** 함

### **다변수 이차 함수 (Quadratic functions in many variables)**
- 여러 변수에 대한 이차 함수에서는 편미분 식으로 구성된 연립 방정식 시스템이 **ill-conditioned**일 수 있음
- 즉, 해가 **매우 민감하게 흔들리거나 수치적으로 안정적이지 않음**
$$
f(w, x, y, z) = 2w^2 + 3x^2 + 4y^2 + 5z^2 + 6wx + 7xy + 8yz + 9zw + 10w + 11x + 12y + 13z + 14
$$

### **Ill-conditioned 상태의 특징**
- **작은 오차**(측정 오류, 계산 오차)가 **결과에 큰 영향**을 미침
- 연립방정식을 풀 때 **안정적인 해(stable solution)를 구하기 어려움**

### **해석적 해(Closed Form)로 최소값 찾기란?**
- 대부분의 경우, **수식만으로 최소값을 정확히 구하는 것은 매우 어렵거나 불가능**
- 특히 **복잡하거나 비선형** 함수에서 두드러짐

### **Other Convex Functions**
- **전역 최소값(Global minimum)** 은 존재하지만  
    → **해석적 해(closed form solution)는 없음**
- 예: **로지스틱 회귀(Logistic Regression)**
    - 손실 함수가 시그모이드 함수 기반의 로그우도(Log-likelihood)로 구성됨
$$
E(\theta) = -\frac{1}{m} \sum_{i=1}^m \left[ y_i \log(h_\theta(x_i)) + (1 - y_i)\log(1 - h_\theta(x_i)) \right]
$$
$$
h_\theta(x) = \frac{1}{1 + e^{-\theta^T x}}
$$

### **Nonlinear Functions (비선형 함수)**
- 편미분(Partial derivative) 자체가 **비선형적**
- 예: 신경망에서의 활성 함수 합산, 비선형 결합 등
$$
f(x_1, x_2) = x_1 \cdot \sin(x_1, x_2) + x_2^2
$$
- 이런 함수는 해석적으로 풀 수 없기 때문에 경사하강법 등의 **수치적 최적화 방법**에 의존해야 함

### **해를 찾는 수치적 근사 방법들 (Approximate Methods)**
- 복잡한 함수에서는 해석적으로 해를 구하기 어렵기 때문에 **수치적으로 근사한 해를 찾는 기법** 사용
- 대표적인 방법들:
    - **Gradient Descent Method (경사 하강법)**
    - Newton Method
    - Gauss–Newton
    - 기타 고차 최적화 기법 등

### **함수의 종류에 따른 최적화 난이도**

**Convex 함수**
- 볼록 함수이며, **전역 최소값이 하나**
- 수렴이 비교적 안정적이며 단순한 경사 하강법으로도 잘 작동

![](https://i.imgur.com/U1Ed5a8.png)

**Non-Convex 함수**
- **다수의 국소 최소값(local minima)** 및 **전역 최소값(global minimum)** 존재
- 최적화 과정에서 로컬 최소점에 빠질 수 있어 더 어려움

![](https://i.imgur.com/pzLNYu3.png)

### Gradient는 **벡터(Vector)** 이다.
- **정의**: Gradient는 어떤 함수의 **모든 변수에 대한 편미분 값들의 모음**
- 함수가 한 지점에서 **얼마나, 어떤 방향으로 변화하는지**를 나타냄  
    → 즉, **변화 방향(direction)** 과 **변화 크기(magnitude)** 를 동시에 제공

**일반 수식 표현** :
$$
\nabla f(w) = \nabla f(w_1, w_2, \dots, w_m) =
\begin{bmatrix}
\frac{\partial f(w)}{\partial w_1} \\
\frac{\partial f(w)}{\partial w_2} \\
\vdots \\
\frac{\partial f(w)}{\partial w_m}
\end{bmatrix}
$$
- ∇ (nabla): **gradient 연산자**를 의미
- $f(w)$: 벡터 $w$에 대한 **스칼라 함수**
- $∇f(w)$ 각 변수 $w_i$​에 대한 **편미분 값들을 모아 만든 벡터**

### Gradient 정의
**Gradient**는 함수 fff의 모든 변수에 대한 **편미분(partial derivatives)** 을 벡터로 묶은 것이며,  
특정 지점 ppp에서의 변화 방향과 크기를 나타냅니다.

![](https://i.imgur.com/SBmls9Z.png)

### 시각적 의미
- 그림의 곡면은 함수 $f(w1​,w2​)$의 값 (출력)을 3차원으로 표현한 것
- 점 $(w1​,w2​)$에서의 함수의 값: $f(w1​,w2​)$
- 이 점에서의 편미분:
    - $\frac{∂f(w_{1},w_{2})}{∂w_{1}}$ ​: $w_1$​ 방향으로의 변화율
    - $\frac{∂f(w_{1},w_{2})}{∂w_{2}}$ ​: $w_2$​ 방향으로의 변화율
- 두 편미분을 벡터로 묶은 것이 바로 Gradient :
$$
\nabla f(w_1, w_2) =
\begin{bmatrix}
\frac{\partial f(w_1, w_2)}{\partial w_1} \\
\frac{\partial f(w_1, w_2)}{\partial w_2}
\end{bmatrix}
$$

### 목적:
**예측값 $f(x)$** 과 **실제값 $y$** 사이의 오차를 최소화하려면,  
다음과 같은 오차 함수를 최소화해야 합니다 :
$$
E(w_1, w_2, \dots, w_m) = \sum_{(x, y) \in \text{Data}} \left( y - f(x; w_1, w_2, \dots, w_m) \right)^2
$$

![](https://i.imgur.com/aAwiqxV.png)
- 세로축: $E(w)$ → 손실 함수 값 (오차)
- 가로축: $w$ → 파라미터 값
- 곡선: 손실 함수의 모양. 여러 개의 **최솟값(minimum)** 존재
- 노란 원 + 빨간 화살표: 각각의 **지역 최소값(local minimum)**
    - 이 중 하나가 선택되어 학습이 종료될 수 있음
- “Find out any of those minimums” →  즉, 여러 최소값 중 **어떤 하나라도 찾아내는 것이 현실적인 목표**

---

![](https://i.imgur.com/DN9LDt0.png)

## How to Update $w$?

###  **초기값 $w^0$ 선택**
- 최적화를 시작하기 위해 **임의의 초기값 $w^0$** 를 선택
- 이 점에서부터 기울기(gradient)를 따라 이동하며 오차 $E(w)$를 줄이는 방향으로 업데이트 수행

###  **업데이트 진행 방향**
- 그림의 파란 곡선은 오차 함수 E(w)E(w)E(w)
- 아래쪽으로 점점 이동하는 화살표는 다음을 나타냄:
    - **기울기 방향 반대**로 이동하며 오차가 점점 줄어듦
    - 마침내 **로컬 최소값 근처에서 수렴** (평평한 빨간선 위치)

###  **일반 수식 표현**
$$
w^{(t+1)} = w^{(t)} - \eta \cdot \nabla E(w^{(t)})
$$
- $η$ : 학습률 (learning rate)
- $∇E(w(t))$ : 현재 지점 $w^{(t)}$에서의 기울기

---



### 핵심 원리
**기울기(gradient)의 반대 방향으로 이동해야** 오차가 줄어든다.

![](https://i.imgur.com/xTg6FvB.png)
- 곡선: 오차 함수 $E(w)$
- $w^0$: 초기값
- 파란 점선: 기울기를 나타냄
- $w^1$: 기울기 방향의 반대로 이동하여 오차가 줄어든 새로운 위치

### 방향 결정 조건
- $slope>0$ : $w$를 **작아지게** 조정해야 오차 감소 ⇒ **왼쪽으로 이동**
- $slope<0$ : $w$를 **커지게** 조정해야 오차 감소 ⇒ **오른쪽으로 이동**
$$
\text{slope} = \frac{\Delta y}{\Delta x}
$$

---

![](https://i.imgur.com/DbYaZQM.png)

### 개념 정리
- $w$: 파라미터 벡터
- $Δw$: 업데이트 방향 벡터 (즉, gradient)
- 벡터 $w+Δw$는 **너무 큰 스텝**이 될 수 있음  
    → **오차 함수의 전역 최소값(global minimum)을 건너뛸 위험** 존재


### 해결 방법: 학습률 $η$ 도입

**학습률 없이 :**
$$
w_{\text{new}} = w + \Delta w
$$
- $w+Δw$는 **너무 큰 스텝**이 될 수 있음 

**학습률 적용 :**
$$
w_{\text{new}} = w + \eta \cdot \Delta w
$$
- 여기서 $η∈(0,1)$ 는 스텝 크기를 조절하는 **스케일 팩터**
- 너무 큰 $Δw$ 아닌 $ηΔw$ 로 조정해서 **학습 안정성 확보**

---

![](https://i.imgur.com/vusDttR.png)

### 핵심 개념
- **학습률 $η$** 는 파라미터 업데이트의 **스텝 크기**를 제어합니다.
- 너무 크게 업데이트하면 발산 위험, 너무 작게 하면 느리게 수렴
- 적절한 학습률을 사용하면 **최솟값(minimum)** 으로 안정적으로 접근 가능

### 수식 표현
**일반 수식 표현** :
$$
w^1 = w^0 - \eta \cdot \left. \frac{\partial E}{\partial w} \right|_{w = w^0}
$$
- $η$ : learning rate (학습률)
- $\frac{∂E}{∂w}$ ​: 현재 위치에서의 기울기
- 이 수식을 반복 적용하여 오차를 점점 줄임

---

![](https://i.imgur.com/rImRwsX.png)

### 핵심 원리
**오차 함수 $E(w)$** 의 기울기 $∇E(w)$ 가 0이 될 때까지 반복적으로 파라미터 $w$를 업데이트합니다.

### 반복 수식
**일반 수식 표현** :
$$
w^{(t+1)} = w^{(t)} - \eta \cdot \left. \frac{\partial F}{\partial w} \right|_{w = w^{(t)}}
$$
- $η$ : learning rate (학습률)
- $\frac{∂E}{∂w}$ ​: 현재 위치에서의 기울기
- $w^{(t)}$: t번째 스텝의 파라미터 값

### 종료 조건
아래 조건을 만족하면 종료 :
$$
\left\| \nabla E(w^{(t)}) \right\| \approx 0
$$
- 즉, 더 이상 내려갈 방향(기울기)이 없다면 수렴한 것으로 간주

---

## Gradient Descent (GD)

### 의사코드 (Pseudo code)
1. 초기값 w⁰을 무작위로 선택
2. 반복 수행:
     $w^{t+1} = w^t - η · (∂E/∂w) | _{w = w^t}$
3. 다음 조건 중 하나 만족 시 반복 종료

### 수식 표현
**일반 수식** :
$$
w^{t+1} = w^t - \eta \cdot \left. \frac{dE}{dw} \right|_{w = w^t}
$$
### 반복 종료 조건 (Stopping condition)
1. 파라미터의 변화가 매우 작을 때
2. 오차 함수 $f(w)$의 변화량이 거의 없을 때
3. 고정된 반복 횟수 도달  
    (예: 100번 또는 200번 반복)

## Gradient Descent (GD) for Multiple Parameters

### 목적
여러 개의 파라미터 $w0,w1,…$가 있는 경우, 각 파라미터에 대해 **개별적으로 기울기를 계산하고 동시에 업데이트**합니다.

### 수식 표현 (2개의 파라미터 예시)
**일반 수식** :
$$
w_0^{t+1} = w_0^t - \eta \cdot \left. \frac{\partial E}{\partial w_0} \right|_{w_0 = w_0^t, w_1 = w_1^t}
$$
$$
w_1^{t+1} = w_1^t - \eta \cdot \left. \frac{\partial E}{\partial w_1} \right|_{w_0 = w_0^t, w_1 = w_1^t}
$$

### 절차 요약 (의사코드 형식)
1. 초기 파라미터 값 $w_0^0, w_1^0$​을 랜덤으로 선택
2. 반복:
    - 각 파라미터별 기울기 계산
    - 동시에 각 파라미터를 업데이트
3. 종료 조건 만족 시 반복 종료

### 일반화된 벡터형 표현
복수의 파라미터 $w=[w_0,w_{1},...,w_m]$ 일 경우 :
$$
\mathbf{w}^{(t+1)} = \mathbf{w}^{(t)} - \eta \cdot \nabla E(\mathbf{w}^{(t)})
$$
