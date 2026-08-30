
## 초록

장문 생성에서 나타나는 언어 혼입, 반복, 스타일 드리프트는 “특정 언어를 금지/강제”하는 규칙만으로 해결되기보다, 생성 과정에서 **언어군을 포함한 문맥 상태(context state)**가 안정적으로 유지·갱신되지 못해 발생하는 현상으로 볼 수 있다. 본 논문은 Transformer 계열(Decoder-only, Encoder-Decoder, Encoder-only) 전반에 적용 가능한 **지속 상태 토큰(persistent state token)** 프레임워크 _StateSlot_을 제안한다. StateSlot은 (i) 상태 갱신에 사용되는 정보를 **상태 슬롯이 강하게 참조한 토큰 Top-$m$만 선택**해 정제하고, (ii) 후보 상태와 기존 상태의 **코사인 거리**로부터 **overwrite 게이트**를 구성해 보수적 갱신과 빠른 전환을 통합하며, (iii) **초기화 민감도 파라미터 $\rho$**로 “상태 유지/대체” 수준을 연속적으로 제어한다. 또한 본 논문은 고정 길이 윈도우/세그먼트 운용 없이, **각 답변(출력) 종료 시 1회 업데이트(per-output update)**만 수행하는 설정을 정식화하고, 해당 설정의 특성(턴 내부 즉시 복구 제약)을 반영한 **2-Turn 회복 중심 평가 프로토콜**을 제시한다.

---

## 1. 서론

Transformer 기반 모델은 긴 입력·긴 생성에서 문맥 유지가 핵심 난제이며, 이를 위해 희소 어텐션과 전역 토큰(Longformer), 세그먼트 간 recurrence(Transformer-XL), 메모리 토큰을 추가하는 구조(RMT) 등이 제안되어 왔다. 또한 Prefix-Tuning은 “가상 토큰(prefix)”을 학습해 이후 토큰이 지속적으로 조건을 참고하도록 한다.

하지만 “언어 혼입/반복/드리프트”를 단순 제약(금지 토큰, 언어 필터링)으로 처리하면, 실제로 유지되어야 하는 목표가 “언어”만이 아니라 **언어군을 포함한 문맥 상태(톤/형식/논점/역할)**라는 점을 놓치기 쉽다. 본 논문은 이를 **쓰기(write) 제어 문제**로 재정의한다. 즉, (a) 생성 내내 참고할 상태 슬롯을 제공하되, (b) 상태가 무엇을 받아들일지(선택적 쓰기), (c) 언제 전환할지(적응적 초기화)를 구조적으로 설계한다.

### 기여(Contributions)

- **(C1) Turn-level StateSlot 정식화:** 고정 길이 윈도우 없이 **답변 종료 시 1회(per-output) 업데이트**만 수행하는 상태 슬롯 구조를 Transformer 계열 전반에 공통 적용 가능하도록 정의한다.
    
- **(C2) 규칙 기반 선택적 쓰기:** 상태 갱신 입력을 **STATE$\rightarrow$token self-attention Top-$m$** 으로 고정하여, 상태 오염(잡음/일시 혼입)과 불필요한 전환을 억제한다.
    
- **(C3) 거리-조건 overwrite 게이트:** 코사인 거리와 초기화 민감도 $\rho$로 “보수 유지 $\leftrightarrow$ 빠른 대체”를 통합하는 업데이트를 제시하고, **턴 내부 즉시 복구 한계**를 평가 프로토콜(2-Turn 회복)로 정면 반영한다.
    

---

## 2. 관련 연구(방어적 정리)

### 2.1 전역 토큰·희소 어텐션 기반 장문 처리

Longformer는 로컬 윈도우 어텐션과 전역 어텐션을 결합해 긴 시퀀스를 효율적으로 처리하며, 일부 위치를 전역 토큰으로 지정한다. 이는 “허브 슬롯이 전역 정보를 매개”한다는 점에서 동기적 유사성이 있으나, 본 연구는 attention 패턴 설계가 아니라 **생성 중 문맥 상태 붕괴를 쓰기 제어로 다루는 것**이 목표다.

### 2.2 메모리 토큰/recurrence 계열(Transformer-XL, RMT)

Transformer-XL은 segment-level recurrence로 장기 의존을 확장한다. RMT는 특수 메모리 토큰을 추가해 정보 교환을 가능하게 한다.  
본 연구는 “메모리/상태 토큰”이라는 큰 틀은 공유하지만, (i) **업데이트를 ‘턴 종료 1회’로 고정**하고, (ii) **STATE$\rightarrow$token Top-$m$** 으로 쓰기 입력을 규칙적으로 제한하며, (iii) **거리-조건 overwrite 게이트($\rho$)** 로 전환을 정의한다는 점을 분리된 설계 변수로 명확히 한다.

### 2.3 조건 주입(가상 토큰) 계열(Prefix-Tuning)

Prefix-Tuning은 연속 prefix 벡터를 학습해 이후 토큰이 이를 “virtual tokens”처럼 참고하게 한다. StateSlot도 “항상 참조되는 슬롯”을 둔다는 점은 유사하지만, 본 연구의 상태는 학습된 고정 프롬프트가 아니라 **대화/출력에 따라 갱신되는 상태**이며, 그 갱신을 **선택적 쓰기 + 거리-게이트 전환**으로 제어한다.

### 2.4 “무엇을 저장할지”를 학습하는 선택적 메모리(BudgetMem)

BudgetMem은 장문 처리 비용을 줄이기 위해 “무엇을 기억할지”를 학습된 정책으로 선택하는 selective memory policy를 제안한다. 본 연구도 “선택적 저장” 문제의식은 공유하지만, (i) 비용/예산 최적화가 아니라 **문맥(언어 포함) 일관성 붕괴**가 목표이며, (ii) 선택 규칙을 정책 학습이 아니라 **STATE self-attention Top-$m$ 규칙으로 고정**해 오염 억제를 전면화한다.

### 2.5 게이트/캐시 갱신을 포함한 Transformer 변형(FoX, Cached Transformers)

FoX는 어텐션에 forget gate를 포함하는 방식으로 장문 성능을 개선하는 방향을 제시한다. Cached Transformers는 differentiable memory cache를 recurrent gating으로 업데이트한다.  
이들과 달리 본 연구의 게이트는 attention 자체를 변형하거나 캐시를 확장하는 목적이 아니라, **문맥 상태 요약 벡터의 overwrite 강도**를 결정하는 **거리-조건 스칼라 게이트**이며, 업데이트가 **턴 종료 1회**라는 점을 명시한다.

### 2.6 문서 단위 MT의 메모리 업데이트(attention 기반)

문서 레벨 MT에서도 문장 상태를 메모리에 반영하는 update attention 모듈이 제안되어 왔다. 본 연구는 (i) 업데이트 스케줄을 문장/윈도우 단위가 아니라 **답변 종료 1회**로 고정하고, (ii) 업데이트 입력을 **STATE$\rightarrow$token Top-$m$** 으로 제한하며, (iii) **거리-게이트($\rho$)** 로 전환을 정식화한다는 점에서 설계 초점이 다르다.

---

## 3. 방법: StateSlot (per-output update)

### 3.1 Persistent State Slot: 입력 인터페이스 확장

- hidden size: $d$
    
- 입력 토큰 길이: $L$
    
- 토큰 임베딩 함수: $E[\cdot]$
    
- 입력 토큰 시퀀스: $x$
    
- 이전 상태 벡터: $s_{\text{prev}} \in \mathbb{R}^{d}$
    

기존 입력 임베딩은 다음과 같다.

$$  
X = E[x] \in \mathbb{R}^{L \times d}  
$$

StateSlot은 입력 맨 앞에 상태 슬롯을 1개 추가해 입력을 구성한다(행(row) 방향으로 스택).

$$  
\tilde{X} = [, s_{\text{prev}} ; ; ; X ,] \in \mathbb{R}^{(L+1) \times d}  
$$

여기서 $[a ; B]$는 벡터 $a$를 첫 행으로 두고, 그 아래에 행렬 $B$를 붙인 스택 연산을 의미한다.

---

### 3.2 Selective Write: STATE$\rightarrow$token attention Top-$m$ (재현성 위해 고정)

한 턴 처리 후 마지막 레이어 $N$에서 헤드별 self-attention을 $A^{(N,h)}$라 하자($h=1,\dots,H$). 인덱스 $0$이 상태 슬롯, $1..T$가 토큰 위치이다.

영향도(importance)는 **마지막 레이어, 헤드 평균의 STATE$\rightarrow$token attention**으로 정의한다.

$$  
a_i = \frac{1}{H}\sum_{h=1}^{H} A^{(N,h)}[0, i] \quad \text{for } i=1,\dots,T  
$$

Top-$m$ 선택:

$$  
I = \text{TopM}({a_i}_{i=1}^{T}, m)  
$$

선택된 토큰의 마지막 레이어 hidden을 모은 집합:

$$  
C = {, h_i^{(N)} \mid i \in I ,}  
$$

후보 상태(평균 풀링):

$$  
\hat{s} = \frac{1}{m}\sum_{h \in C} h  
$$

---

### 3.3 Adaptive Reset: 코사인 거리 기반 overwrite 게이트($\rho$)

코사인 거리:

$$  
d = 1 - \cos(s_{\text{prev}}, \hat{s})  
$$

overwrite 게이트(여기서 $\alpha$는 기울기, $\rho$는 초기화 민감도):

$$  
g = \sigma\big(\alpha(d - \rho)\big)  
$$

업데이트:

$$  
s_{\text{new}} = (1-g)s_{\text{prev}} + g\hat{s}  
$$

---

### 3.4 단순형(어블레이션): EMA

EMA는 $g$를 상수로 둔 특수 사례:

$$  
s_{\text{new}} = (1-\eta)s_{\text{prev}} + \eta \hat{s}  
$$

---

### 3.5 턴 단위 알고리즘

턴 $k$에서:

1. 입력에 상태 슬롯 주입: $[s_{k-1}] + x_k$
    
2. 답변 $y_k$ 생성
    
3. 마지막 레이어 attention으로 ${a_i}$ 계산, Top-$m$ 선택
    
4. $\hat{s}_k$ 계산(평균 풀링)
    
5. $d_k$, $g_k$ 계산 후 $s_k$ 업데이트
    
6. 다음 턴에 $s_k$ 사용
    

---

## 4. 평가 프로토콜: 2-Turn 회복 중심(턴 내부 한계 반영)

턴 단위 업데이트는 같은 턴 내부에서 상태를 즉시 재작성해 복구하기 어렵다. 따라서 평가는 (A) Turn-1에서 붕괴가 얼마나 발생하는지와 (B) Turn-2에서 회복/유지가 가능한지를 함께 측정해야 한다.

### 4.1 2-Turn Consistency Benchmark 구성

- Turn-1(Stress Turn): 길고 복잡한 답변을 요구하며 교란을 삽입
    
- Turn-2(Recovery Turn): 동일 언어/톤/형식으로 Turn-1의 연장/요약/정리/검증을 요구
    

Turn-1 교란 유형:

1. 오염(Contamination): URL/코드/영문 약어 인용 후 설명 지속 요구
    
2. 반복 유도(Loop trigger): 유사 문장 변주/목록 확장 등 반복 폭주 유도
    
3. 전환(Shift): Turn-1 후반에 완전 다른 주제/작업 지시 삽입
    

### 4.2 지표

Turn-1 내부(붕괴 관찰)

- Drift onset position: 언어 스크립트 비율 급증, 반복 $n$-gram 비율 급증 등으로 최초 붕괴 위치
    
- Loop severity: 반복 비율 및 반복 구간 길이
    

Turn-2(회복/유지 효과)

- Recovery success rate: 목표 언어/톤/형식 유지 성공률
    
- Recovery length: Turn-2에서 안정화까지 필요한 토큰 수
    
- State shift adequacy: $d_k$, $g_k$, $\cos(s_{k-1}, s_k)$ 분포로
    
    - 오염/반복에서 $g_k$ 과대 상승 억제
        
    - 전환에서 $g_k$ 충분히 상승
        

### 4.3 어블레이션

- Base: 상태 슬롯 없음
    
- Main: Top-$m$ + 거리-게이트($\rho$)
    
- Simple: Top-$m$ + EMA
    
- 스윕: $m \in {8,16,32}$, $\rho$ 여러 값, $\alpha$ 소수 값 고정
    

---

## 5. 논의

StateSlot은 기존 전역 토큰/메모리 토큰 연구와 구성 요소가 일부 겹치지만, 본 논문은 (i) 턴 종료 1회 업데이트라는 스케줄을 1급 설계로 두고, (ii) STATE$\rightarrow$token Top-$m$ 규칙으로 쓰기 입력을 고정해 오염 억제를 전면화하며, (iii) 거리-조건 overwrite 게이트($\rho$)로 전환을 정의한다는 점에서 주장 단위를 명확히 분리한다. 또한 턴 단위 업데이트의 제약(턴 내부 즉시 복구 어려움)을 숨기지 않고, 이를 2-Turn 회복 평가로 직접 측정하도록 설계한다.

---

## 6. 결론

본 논문은 Transformer 계열 전반에 적용 가능한 StateSlot을 제안했다. StateSlot은 지속 상태 슬롯을 입력에 추가하고, 상태 업데이트를 (a) STATE self-attention 기반 Top-$m$ 선택적 쓰기와 (b) 코사인 거리 기반 overwrite 게이트($\rho$)로 구성해 문맥(언어 포함) 일관성 붕괴를 “쓰기 제어” 관점에서 다룬다. 또한 고정 윈도우 없이 답변 종료 시 1회 업데이트만 수행하는 설정을 정식화하고, 그 한계를 반영한 2-Turn 회복 중심 평가 프로토콜을 제시했다.

---

## 참고문헌 (URL 유지)

- Beltagy, I., Peters, M. E., Cohan, A. _Longformer: The Long-Document Transformer_. 2020.  
    [https://arxiv.org/abs/2004.05150](https://arxiv.org/abs/2004.05150)
    
- Dai, Z., Yang, Z., Yang, Y., et al. _Transformer-XL: Attentive Language Models Beyond a Fixed-Length Context_. 2019.  
    [https://arxiv.org/abs/1901.02860](https://arxiv.org/abs/1901.02860)  
    (ACL Anthology PDF) [https://aclanthology.org/P19-1285.pdf](https://aclanthology.org/P19-1285.pdf)
    
- Bulatov, A., Kuratov, Y., Burtsev, M. S. _Recurrent Memory Transformer_. 2022.  
    [https://arxiv.org/abs/2207.06881](https://arxiv.org/abs/2207.06881)
    
- Li, X. L., Liang, P. _Prefix-Tuning: Optimizing Continuous Prompts for Generation_. 2021.  
    [https://arxiv.org/abs/2101.00190](https://arxiv.org/abs/2101.00190)  
    (ACL Anthology) [https://aclanthology.org/2021.acl-long.353/](https://aclanthology.org/2021.acl-long.353/)
    
- Alla, C. V. K., Gaddam, H. N., Kommi, M. _BudgetMem: Learning Selective Memory Policies for Cost-Efficient Long-Context Processing in Language Models_. 2025.  
    [https://arxiv.org/abs/2511.04919](https://arxiv.org/abs/2511.04919)
    
- Lin, Z., Nikishin, E., He, X. O., Courville, A. _Forgetting Transformer: Softmax Attention with a Forget Gate_. 2025.  
    [https://arxiv.org/abs/2503.02130](https://arxiv.org/abs/2503.02130)
    
- Zhang, Z., Shao, W., Ge, Y., et al. _Cached Transformers: Improving Transformers with Differentiable Memory Cache_. 2023.  
    [https://arxiv.org/abs/2312.12742](https://arxiv.org/abs/2312.12742)
    
- Feng, Y., et al. _Transformer with Recurrent Memory for Document-Level Machine Translation_. Findings of NAACL 2022.  
    [https://aclanthology.org/2022.findings-naacl.105.pdf](https://aclanthology.org/2022.findings-naacl.105.pdf)  
    (arXiv) [https://arxiv.org/abs/2205.01546](https://arxiv.org/abs/2205.01546)
    

---
