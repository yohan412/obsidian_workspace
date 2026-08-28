
## 급부상하는 LLM

### 초기 접근 방식의 한계

- **1950~60년대**
    
    - Symbolic(기호) 추론과 Rule 기반 시스템에 주로 의존
        
    - 매우 취약하고 능력에 한계가 많음
        
- **1980년대**
    
    - Rule‑기반 프로그래밍으로는 인간 지능의 다양성과 견고함을 재현 불가를 인식
        
        - (Expert System: Prolog, LISP)
            
    - 기계학습 기반 접근 등장 → 컴퓨터 비전·음성 인식 분야에서 일정 진전
        
    - **그러나** AGI(인공 일반 지능) 달성 목표는 요원함
        

### 딥러닝의 부상

- **2000년대 초반**
    
    - 딥러닝 신경망(deep learning neural network) 등장 (Geoffrey Hinton 등, 2006년 제안)
        
- **2010년대**
    
    - 충분한 데이터와 컴퓨팅 파워를 갖춘 심층 신경망을 활용
        
    - 이미지 분류·물체 인식, 컴퓨터 비전·음성 인식 등에서 놀라운 성과 달성
        

#### 남은 과제 및 새로운 학습 기법

- **지도 학습의 한계**
    
    - 레이블이 달린 대규모 데이터셋 확보가 어려움
        
- **자가 지도 학습(Self‑Supervised Learning)**
    
    - 레이블 없이도 데이터 자체에서 특성을 학습
        
    - 튼튼한 표현(embedding) 모델 구축 가능
        
- **Auto‑Regressive 학습**
    
    - 텍스트 시퀀스에서 “다음 단어 예측”을 통해 언어 모델 훈련

### 신경 언어 모델링의 발전

- **Neural Language Modeling** 기법의 등장  
    자연어 처리(NLP)에 딥러닝을 활용할 수 있는 토대 마련
    

#### 2013년: word2vec

- Google에서 제안
    
- **비지도 학습** 방식으로 레이블 없이 대량 텍스트 데이터를 학습
    
- Shallow Neural Network를 이용한 효율적 단어 임베딩 학습
    
- 단점: 문맥 의존성 부족 (각 단어별로 고정된 벡터)
    

#### 2018년: ELMo

- University of Washington에서 제안
    
- **LSTM** 기반으로 양방향 문맥(context)을 고려한 단어 표현 학습
    
- 문맥 따라 변하는 임베딩 제공
    

#### 2018년 후반: BERT

- Google 제안 (“Attention Is All You Need” 이론 기반)
    
- **Bidirectional Encoder Representations from Transformers**
    
- **Masking 기반 언어 모델링**
    
    - 입력 문장에서 일부 토큰(masked token)을 가리고, 이를 예측하도록 학습
        
- 사전 학습(Pre‑training) + 파인튜닝(Fine‑tuning) 방식 도입
    
- 다양한 NLP 태스크에서 획기적 성능 향상
    

#### 2020년 이후: 초거대 언어 모델(LLM)의 등장

- **GPT‑3 (OpenAI, 1750억 파라미터)**
    
    - Zero‑/Few‑Shot 학습 능력
        
    - 모델 크기의 중요성 부각
        
- 이후 수천억 ~ 조 단위 파라미터 LLM 등장
    
    - 예: PaLM (Google, 2022년 5400억 파라미터)

#### PLM에서 LLM으로의 전환: 질적 변화

- **LLM의 새로운 능력**
    
    - Few‑shot 학습, 시퀀스 연결, 지시사항 수행 등 소형 모델에서 불가능했던 기능을 수행
        
- **임계점 기반 성능 폭발**
    
    - 모델 규모가 일정 임계값을 넘으면 갑자기 나타나는 비선형적 성능 향상
        
    - 단순한 점진적 확장으로 설명되지 않는 ‘임계 현상’
        
- **AI 패러다임의 전환**
    
    - 작은 문제 해결용 특화 시스템에서 ▶ 다재다능한 범용(General‑Purpose) 모델로 이동
        
- **파인튜닝 없이 범용성 발휘**
    
    - 별도 파인튜닝 없이도 다양한 문제를 놀랍도록 해결하는 범용 AI 시스템으로 기능

#### 생성형 AI와 LLM의 차이

- **생성형 AI**
    
    - 텍스트, 이미지, 비디오 등 다양한 유형의 콘텐츠를 생성하는 AI 시스템을 포괄하는 더 넓은 개념
        
- **LLM (Large Language Model)**
    
    - 자연 언어 데이터를 처리하고 이해하도록 설계된 특정 범위의 딥러닝 모델
        
- **현재**
    
    - 두 개념 간의 실질적 차이는 거의 없음
        
- **관계**
    
    - 생성형 AI 애플리케이션은 LLM을 핵심으로 하는 통합 애플리케이션
        

#### 생성형 AI 애플리케이션 유형

- 대화형 에이전트 및 챗봇
    
- 코드 완성형 프로그래밍 어시스턴트
    
- 언어 번역
    
- 텍스트 요약 및 생성
---

# 생성형 AI 애플리케이션 3계층

스택의 세 가지 주요 계층:

1. 인프라 계층
    
2. 모델 계층
    
3. 애플리케이션 계층

## 1. 인프라 계층

생성형 AI 애플리케이션을 개발·학습·배포하기 위해 반드시 갖춰야 할 기본 데이터·컴퓨팅·도구 자원을 다룹니다.

### 1.1 데이터 저장 및 관리

- **훈련 데이터**
    
    - Petabyte 단위로 저장·관리·접근이 효율적이어야 함
        
    - 분산 파일 시스템(예: Amazon S3, Google Cloud Storage) 활용
        
- **데이터 레이크 (Data Lake)**
    
    - 로그·이미지·텍스트 등 이질적 소스의 원천 데이터를 중앙에 집중 저장
        
    - 수집 → 정리 → 처리 → 분석 파이프라인 구성
        
- **견고한 데이터 관리 플랫폼**
    
    - 대규모 학습 데이터로부터 안정적 이점을 얻기 위해 필수
        
    - 버전 관리·메타데이터 관리·접근 제어 기능 포함
        

### 1.2 벡터 데이터베이스

- **임베딩 저장·검색**
    
    - LLM이 생성한 단어·문서 임베딩을 숫자 벡터로 저장
        
- **주요 제품**
    
    - Pinecone, Weaviate, Milvus, Chroma 등
        
- **기능**
    
    - 수십억 개 벡터 색인(Indexing) 및 효율적 유사도 검색(Cosine similarity)
        
    - RAG(Retrieval‑Augmented Generation) 등 외부 벡터 DB와 연동
        

### 1.3 컴퓨팅 인프라

- **대규모 분산 연산 자원**
    
    - 수천~만 대 GPU/TPU 클러스터 또는 클라우드 인스턴스
        
    - 예: GPT‑3 학습 시 10,000개 이상의 GPU, 총 3,640 petaflop/s·days
        
- **인프라 고려 사항**
    
    - 네트워크 대역폭, 스토리지 I/O, 전력 및 냉각 리소스
        
    - 온프레미스 vs. 클라우드 선택(비용·유연성·운영 편의성)

## 2. 모델 계층

### 2‑1. 올바른 LLM 선택

- **모델 기능**:
    
    - Open vs. Closed, 제공하는 고유 기능 고려
        
- **계산 효율성**:
    
    - 선택한 LLM을 학습·배포할 때 필요한 컴퓨팅 자원 판단
        
- **문제 적합성**:
    
    - 해결하려는 과제와 도메인에 적합한 모델 판단 (예: 창의적 텍스트 생성용 GPT vs. T5 계열)
        

### 2‑2. LLM 파인튜닝

1. **전이 학습(Transfer Learning)**
    
    - 한 작업에서 학습한 지식을 다른 작업에 재사용
        
2. **추가 데이터 셋 활용**
    
    - 특화된 도메인·태스크를 위한 소규모 레이블 데이터로 미세 조정
        
3. **성능 향상**
    
    - 파인튜닝을 통해 대화 시스템, 검색·QA 등 특정 영역에서 성능 개선
        
4. **치명적 망각 방지**
    
    - Gradient Clipping, 특정 레이어만 선택적 파인튜닝으로 이전 지식 유지
        

### 2‑3. LLM 통합

1. **데이터 변환(Data Wrangling)**
    
    - 애플리케이션 코드의 입력(텍스트·그 외 포맷)을 토크나이저/임베더블로 변환
        
2. **모델 아키텍처 연결**
    
    - 입력 토큰에 self‑attention 등 각 요소 적용
        
    - 다양한 구조(예: Mixture of Experts) 활용
        
3. **확장 확산(Scalable Deployment)**
    
    - SGoME(Sparsely Gated Mixture of Experts) 같은 기술로 여러 전문가 모델 병렬화

## 3. 애플리케이션 계층

### 3‑1. 애플리케이션 개발 프레임워크

- **LangChain** 등 LLM API 중심 프레임워크
    
    - 여러 구성 요소(chain)를 연결해 최종 사용자 워크플로우 구성
        
    - 1. 입력 템플릿 생성 → 2) LLM에 전달 → 3) 결과 파싱·처리 → 출력
            

### 3‑2. 자율 에이전트(Agents)

- **일반 에이전트**
    
    - 계산기, 파일 시스템, 웹 API 등 외부 도구(tool) 사용
        
- **자율 에이전트(Autonomous Agent)**
    
    - 목표 이해 → 계획 수립 → 단계별 실행 → 외부 도구·환경과 상호작용
        
    - 예: AutoGPT, AgentGPT
        

### 3‑3. 애플리케이션 구축 & 운영

- **프레임워크 인터페이스** → LLM 기능 통합
    
- **비즈니스 로직** → 사용자 요청 처리, 후처리
    
- **운영 관리**
    
    - 인증·권한 관리, 보안, 모니터링, 개인정보 보호 등

---

## Flask의 Backend Server 역할

1. **RESTful API 서버**
    
    - Flask는 가볍고 유연한 Python 웹 프레임워크
        
    - AI 모델을 외부와 연결하는 HTTP API 서버로 적합
        
2. **모델 Serving Interface**
    
    - HuggingFace, OpenAI 모델 등을 Python에서 실행하고 Flask가 HTTP 요청을 수신하여 응답
        
    - 모델 호출/파싱/포맷팅 처리
        
3. **Frontend 연동**
    
    - React 등 클라이언트에서 사용자 입력을 받아 Flask로 전달 → 모델 응답 반환
        
4. **세션 및 상태 관리**
    
    - 챗봇 기반 서비스에서 간단한 세션 유지 기능 수행 가능
        

---

## Flask와 LangChain의 관계

- **Flask**: 웹 요청 처리 및 LangChain 호출을 위한 인터페이스
    
- **LangChain**: LLM 워크플로우 관리 (Prompt Chain, Tools, Memory 등)
    
- **LLM**: 실제 답변 생성 (OpenAI, HuggingFace 등)
    
- **RAG(Vector DB)**: 벡터 검색 기반 외부 지식 활용
    

### 구성도

css

복사편집

`[웹 UI client] → [Flask] → [LangChain Agent] → [LLM or Tools]                                           ↘︎ [RAG(Vector DB)]`

### Flask 한계

- 대규모 API 설계 및 관리 어려움 (라우팅, 타입 검증, 테스트 부재 등)
    
- 다수 개발자 협업에 적합한 아키텍처 부재
    
- 확장성과 미들웨어 통합이 어렵고 구조화가 미흡
    

---

## Flask와 Spring의 관계

- **Spring**: 백엔드 인프라 책임 (인증, 보안, DB, 마이크로서비스 등)
    
- **Flask**: AI 모델 서빙 (Python 기반 모델 실행)
    
- Flask와 Spring은 HTTP/gRPC 등으로 연결
    

### 구성도

css

복사편집

`[웹 UI client] → [Spring] → [Flask] → [LangChain Agent] → [LLM or Tools]                                                     ↘︎ [RAG(Vector DB)]`

### Spring의 역할

- 인증: OAuth2, JWT 기반 인증/인가
    
- DB 연동: MyBatis, JPA
    
- 메시징: Kafka, RabbitMQ 등
    
- MSA 환경 구성: Spring Cloud, Gateway 등
    
- Python 모델 실행은 Flask에 위임