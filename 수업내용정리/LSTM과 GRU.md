
## RNN의 Long Term Dependency (장기 의존성)

### 반복 구조 핵심 이해

- 입력 시퀀스: $x1,x2,…,x_{t}t​$
    
- 은닉 상태: $h1,h2,…,h_{t}$
    

### 설명 요약

1. **$x_1 \sim x_{t-1}$​** → $h_{t-1}$​**에 인코딩됨
    
    - 과거 입력들의 정보가 은닉 상태 $h_{t-1}$​에 압축됨
        
    - 즉, 지금까지 본 내용 전체가 벡터 하나로 요약
        
2. **$h_{t-1}$​**는 과거 정보를 담고 있음
    
    - 모델이 필요로 하는 중요한 특징(예: 단어, 문맥 등)이 이 벡터 안에 담겨야 함
        
3. **$h_{t}$​**는 현재 입력 $x_t$​를 처리하는 데 필요한 문맥 정보
    
    - 결국 $x_t$​를 해석할 때 $h_{t-1}$​의 정보가 필요
        

### 하단 그림 설명

- 시계열 데이터 흐름 구조
    
    - 입력 시퀀스: $x_1, x_2, x_3, x_4, x_5$​
        
    - RNN 셀: $h_1, h_2, h_3, h_4, h_5$
        
    - 출력: $o_5$​ (최종 예측 결과)
        
    - 좌측부터 우측으로 시간 순서대로 전달됨

![](https://i.imgur.com/N6DSbRt.png)

## 문제점: **Vanishing Gradient (기울기 소실)**

- 시퀀스가 길어질수록 $h_{t-1}$​에 오래된 정보는 점점 희미해짐
    
- 멀리 있는 정보(예: 문장 맨 앞 단어)는 예측에 영향을 거의 주지 못함
    

## 그래서 등장한 것: **LSTM, GRU**

- 긴 시퀀스에서 중요한 정보를 오래 **기억**하고, 불필요한 정보는 **잊는** 능력 탑재
    
- 이 슬라이드는 **LSTM과 GRU가 왜 필요한지**를 설명하는 **인트로(도입)** 역할

---

### Recurrent Neural Networks (1980년대 중반)

**Long Term Dependency 문제 설명**

#### 1. 문제 요약

- RNN은 시간 흐름에 따라 동일한 가중치(W, U)를 공유하면서 순차적으로 정보를 전달함.
    
- 이때 가중치 W를 반복해서 곱하게 되어 다음과 같은 문제가 발생함.
    

#### 2. Vanishing / Exploding Gradient 문제

- 수식:
    $$
    o_5 = W^4 U X \quad \text{예: } W = 0.5 \Rightarrow (0.5)^4 = 0.0625
    $$
    
- 과거 입력 x1x_1x1​에 대한 영향력이 시간 경과에 따라 급격히 감소하거나 증가함.
    
- 일반적인 현상:
    
    - W < 1: 지수적으로 감소 → **Vanishing Gradient** (기울기 소실)
        
    - W > 1: 지수적으로 증가 → **Exploding Gradient** (기울기 폭발)
        

![](https://i.imgur.com/Skms6Qh.png)

#### 3. 시사점

- RNN은 본질적으로 **긴 시퀀스 학습이 어려움**.
    
- 일반적으로 **10단계 이상**의 과거 정보를 효과적으로 반영하기 어려움.
    
- 이러한 한계를 극복하기 위해 **LSTM**, **GRU**와 같은 구조가 제안됨.

---

### Long Short-Term Memory (LSTM, 1999)

#### 1. 장기 의존성(Long-Term Dependency)을 학습할 수 있는 능력

- **기존 RNN의 한계**: 과거의 정보가 시간이 지남에 따라 사라짐 (vanishing gradient).
    
- **LSTM은 이를 해결하기 위한 구조**를 도입함.
    

#### 2. 메모리 셀 (Memory Cell)

- LSTM은 정보를 보존하거나 잊는 **셀 상태(cell state)**를 도입하여, 장기 기억이 가능하도록 함.
    
- 이 메모리 셀을 통해 **1000단계를 초과**하는 긴 시퀀스에서도 정보를 보존하며 학습 가능.
    

#### 3. 게이트 구조 (Gate Units)

- 과거 정보에 대한 접근을 **선택적으로 허용하거나 차단**하는 구조가 있음.
    
- 주요 게이트:
    
    - **입력 게이트 (Input Gate)**: 새로운 정보를 셀 상태에 얼마나 반영할지 결정.
        
    - **망각 게이트 (Forget Gate)**: 셀 상태에서 어떤 정보를 잊을지 결정.
        
    - **출력 게이트 (Output Gate)**: 셀 상태로부터 어떤 정보를 출력할지 결정.
        
    - (여기서 self gate는 self-loop된 연결을 의미하는 설명일 수도 있음)

---

## Long Short-Term Memory (LSTM) – Basic Structure

### 핵심 구성 요소

![](https://i.imgur.com/ZpxEJnO.png)

#### 1. 입력

- $x_t$​: 현재 입력 데이터
    
- $h_{t-1}$​: 이전 시점의 은닉 상태 (short-term memory)
    
- $c_{t-1}$​: 이전 시점의 셀 상태 (long-term memory)
    

#### 2. 내부 처리 흐름

- 파란색 박스 내부는 **LSTM 셀 내부의 계산 구조**
    
- 오렌지색 선은 **완전 연결 (fully connected) 신경망 계층**을 의미
    

##### 셀 상태 업데이트:

1. **이전 셀 상태 $c_{t-1}$​** → 직접 전달되며, 일부 정보는 유지됨 (long-term memory 역할).
    
2. **입력 게이트 $g_t$​**: 현재 시점의 입력 $x_t$​, 이전 은닉 상태 $h_{t-1}$​를 받아 **신규 정보 생성**.
    
3. 이 신규 정보는 $g_t$​와 곱해져서, **현재 셀 상태 $c_t$​**에 더해짐:
    $$c_t = c_{t-1} + g_t$$
    
    - 이 구조로 인해, 셀 상태는 덧셈 연산 기반이므로 **기울기 소실(vanishing gradient)**을 줄일 수 있음.
        
4. **셀 상태 $c_t$​**는 그대로 전달되거나, **$tanh$**를 거쳐 출력 $h_t$​ 계산에 사용됨.
    

##### 출력:

- 최종 은닉 상태 $h_t$​: 셀 상태를 $tanh$를 통해 변환한 후 출력 게이트와 결합되어 생성됨.
    
- 최종 출력 $y_t$​: RNN 외부로 전달되는 출력

---

## LSTM 구조 요약

### 주요 구성 요소

- **i** : input gate
    
- **f** : forget gate
    
- **o** : output gate
    
- **g** : self-recurrent (candidate cell state)
    
- $c_t$ : internal memory (셀 상태)
    
- $h_t$ : hidden state (은닉 상태)
    
- $y_t$ : final output (최종 출력)
    

### 동작 과정 (수식 기반)

- $i_t = \sigma(x_t U^i + h_{t-1} W^i)$  
    → 입력 게이트: 현재 입력 $x_t$와 이전 은닉 상태 $h_{t-1}$을 통해 입력 정보 비중 결정
    
- $f_t = \sigma(x_t U^f + h_{t-1} W^f)$  
    → 망각 게이트: 과거 셀 상태 $c_{t-1}$을 얼마나 유지할지 결정
    
- $o_t = \sigma(x_t U^o + h_{t-1} W^o)$  
    → 출력 게이트: 현재 은닉 상태 $h_t$를 출력에 얼마나 반영할지 결정
    
- $g_t = \tanh(x_t U^g + h_{t-1} W^g)$  
    → 후보 셀 상태: 새로운 정보 후보 계산 (셀 상태에 추가될 정보)
    
- $c_t = c_{t-1} \cdot f_t + g_t \cdot i_t$  
    → 새로운 셀 상태 $c_t$는 과거 셀 상태와 새로운 정보 후보의 가중합
    
- $h_t = \tanh(c_t) \cdot o_t$  
    → 은닉 상태 $h_t$는 현재 셀 상태 $c_t$를 tanh 처리 후 출력 게이트를 곱함
    
- $y_t = \text{softmax}(V h_t)$  
    → 최종 출력 $y_t$는 은닉 상태를 softmax 함수로 처리하여 계산

### 도식 설명

- 파란색 박스는 각 게이트 연산 (input, forget, output, candidate)
    
- 갈색 원은 수학 연산 (곱셈, 덧셈)
    
- 회색 선은 정보의 흐름
    
- 화살표: 데이터 흐름 방향
    
- 위쪽 흘러가는 경로: 셀 상태 ctc_tct​ 전달
    
- 오른쪽 하단으로 흘러가는 경로: 은닉 상태 $h_t$​와 최종 출력 $y_t$
    

### 요점

- **게이트 구조 덕분에** 과거 정보의 유실을 방지하고, 필요 없는 정보는 제거하며, **장기 의존성 문제**를 해결할 수 있음.
    
- **기울기 소실 문제(vanishing gradient)**를 완화하며, RNN보다 훨씬 안정적인 학습이 가능함.

---

## Gated Recurrent Units (GRU)

### ▪︎ 설명

- GRU는 LSTM보다 구조가 간단함.
    
- LSTM은 3개의 gate를 사용하지만, GRU는 **2개의 gate만 사용**함 (update gate, reset gate).
    
- gate의 개수 외에 원리적으로는 큰 차이가 없음.
    


![](https://i.imgur.com/8tFPPOf.png)

### ▪︎ GRU 구조 수식

- **Reset gate**  
    $r_t = \sigma(W_{xr} x_t + W_{hr} h_{t-1} + b_r)$
    
- **Update gate**  
    $z_t = \sigma(W_{xz} x_t + W_{hz} h_{t-1} + b_z)$
    
- **Candidate hidden state**  
    $\tilde{h}_t = \tanh(W_{xh} x_t + W_{hh}(r_t \cdot h_{t-1}) + b_h)$
    
- **최종 hidden state**  
    $h_t = z_t \cdot h_{t-1} + (1 - z_t) \cdot \tilde{h}_t$
    


### ▪︎ 해석

- $r_t$: reset gate는 과거 상태 $h_{t-1}$을 얼마나 **무시**할지 조절함. (0에 가까우면 과거를 무시)
    
- $z_t$: update gate는 이전 상태 $h_{t-1}$과 새로운 후보 상태 $\tilde{h}_t$ 중 어떤 것을 **더 반영할지** 조절함.
    
- $\tilde{h}_t$: 현재 입력과 일부 과거 정보를 활용해 계산한 **새로운 상태 후보**.
    
- $h_t$: 최종 hidden state는 이전 hidden state와 새로운 후보 사이의 **선형 보간값(linear interpolation)** 으로 계산됨.

