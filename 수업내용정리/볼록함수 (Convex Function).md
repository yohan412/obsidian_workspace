
## What is a Convex Function?

### 정의

함수 $f$가 **convex** 하다는 것은 다음 조건을 만족하는 것을 의미합니다 :
$$
f(tx + (1 - t)x') \leq t f(x) + (1 - t) f(x') \quad \text{for any } x, x' \in X, \; t \in [0,1]
$$
- 좌변: **두 점 $x$, $x^′$** 사이의 **보간된 입력값에서의 실제 함수값**
- 우변: 그 두 점에서의 함수값을 선형 조합한 **선분 위의 점**


### 특수한 경우 $t = \frac{1}{2}$

$$
f\left( \frac{1}{2}x + \frac{1}{2}x' \right) \leq \frac{1}{2}f(x) + \frac{1}{2}f(x')
$$

![](https://i.imgur.com/Uahh5fu.png)

### 시각적 해설

- 파란 곡선: 실제 함수 $f(x)$
- 빨간 직선: $tf(x1)+(1−t)f(x2)$ → 두 점을 이은 직선
- 결과: **볼록 함수는 항상 선분 아래에 있음**

### 왜 중요한가?

- **Convex function**이면:
    - **해가 유일함 (global minimum 보장)**
    - **Gradient Descent 등 수치해법으로 해를 쉽게 구할 수 있음**
- 따라서 실제 최적화 문제에서 해당 함수가 convex인지 판단하는 것이 중요

| 항목     | 내용                               |
| ------ | -------------------------------- |
| 조건     | $f(tx+(1−t)x′)≤tf(x)+(1−t)f(x′)$ |
| 시각적 의미 | 함수가 두 점을 잇는 직선보다 아래에 위치          |
| 중요성    | 최적화에서 해의 존재성과 수렴 보장에 결정적         |

### 수학적 정의

함수 $f:X→R$가 **convex** 하려면 다음 조건을 만족해야 합니다 :
$$
f(tx + (1 - t)x') \leq t f(x) + (1 - t) f(x') \quad \text{for any } x, x' \in X, \; t \in [0, 1]
$$

![](https://i.imgur.com/w1udPV8.png)

|항목|Convex 함수|Non-convex 함수|
|---|---|---|
|정의 만족|O|✕|
|곡선과 직선의 위치|곡선이 선분 아래|곡선이 선분 위|
|최적화 보장|Global minimum 존재|Local minimum 가능성|

---

## Examples of Convex Functions

### 1. **Quadratic Function (이차 함수)**

$$
f(x) = x^2
$$
- 이차 함수는 전 범위에서 곡선이 아래로 오목하므로 볼록(convex)

### 2. **Exponential Function (지수 함수)**

$$
f(x) = 2^x
$$
- 모든 $x$에 대해 2차 미분이 양수 → convex

### 3. **Negative Logarithmic Function (음의 로그 함수)**

$$
f(x) = -\ln x
$$
- 로그 함수 자체는 **concave**
- 부호를 반대로 바꾸면 convex 함수가 됨

![](https://i.imgur.com/eVZ5NCD.png)

| 함수              | 수식    | Convex 여부   |
| --------------- | ----- | ----------- |
| $f(x) = x^2$    | 이차 함수 | ✅           |
| $f(x) = 2^x$    | 지수 함수 | ✅           |
| $f(x) = \ln x$  | 로그 함수 | ❌ (concave) |
| $f(x) = -\ln x$ | 음의 로그 | ✅           |

---

![](https://i.imgur.com/KQebkPc.png)
- 파란 곡선: convex 함수
- 붉은 점: $∇f(w^∗)=0$ 인 지점 → global minimum
- 오직 하나의 최저점만 존재

### 수학적 조건

$$
\nabla f(w^*) = 0 \quad \Longleftrightarrow \quad w^* \text{ is a global minimum}
$$
- $∇f(w^∗)=0$ : gradient가 0이면 경사하강법이 멈추는 지점
- Convex 함수에서는 이 지점이 반드시 **전역 최소값(Global Minimum)**

![](https://i.imgur.com/tXXiaeC.png)
기울기(gradient)가 0이라도,
- **Global minimum**
- **Local minimum**
- **Saddle point**  
    중 어떤 지점일지 확신할 수 없습니다.

### Stationary Points란?
- $∇f(w)=0$ 인 지점들
- 하지만 비볼록 함수에서는 이런 지점들이 최적해가 아닐 수도 있음

### Neural Network와의 관계

Neural Network의 Loss Function은
- 고차원(high-dimensional)
- 비선형(nonlinear)
- 비볼록(non-convex)  
    이기 때문에 많은 경우 **stationary point에 수렴**할 수밖에 없습니다.