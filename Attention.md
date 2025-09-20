# Attention

## 개요

Attention Mechanism은 딥러닝 모델이 입력(input)의 여러 부분 중에서 '지금 중요한 것'에 더 가중치를 두어 처리할 수 있게 만드는 구조입니다

말하자면, 전체 입력을 동일하게 보는 대신, 입력의 일부를 선택적으로 더 주목(attend)함으로써 더 효과적으로 정보를 활용 가능하게 합니다

## 배경 및 필요성

- 순서(sequence) 또는 구조(structure)가 있는 데이터를 다룰 때, 모든 정보가 동등하게 중요한 건 아닙니다. 문장 번역, 요약, 질의응답 등에서는 특히 어떤 단어들이 중요할지가 문맥(context)에 따라 달라집니다

- 기존의 Encoder-Decoder + RNN/LSTM 구조에서는 입력 전체를 하나의 고정 길이 벡터(context vector)로 요약한 뒤 디코더(decoder)에 넘기는 방식이 많았습니다. 그런데 입력이 길어지면 초반 정보가 압축 과정에서 희석되어 버리는 문제가 발생합니다

- 이런 정보 손실, 장기 의존성(long-term dependency) 문제를 해결하고, 입력의 유용한 부분을 동적으로 참조할 수 있게 합니다

## 핵심 개념 및 구성요소

| 구성 요소 | 역할 |
|------------|------|
| Query (쿼리) | “내가 지금 어떤 정보가 필요한가?”를 나타내는 벡터. 디코더의 현재 상태나, 또는 어떤 특정 위치의 표현일 수 있음 |
| Key (키) | 입력의 각 요소(element)가 어떤 특성을 가지고 있는지 표현하는 벡터. 쿼리와 비교되어 얼마나 관련 있는지(score)를 매기는 대상 |
| Value (값) | 입력의 각 요소가 실제로 갖고 있는 정보. 최종적으로 attention 가중치가 곱해져서 출력(context)으로 합산됨 |
| Alignment / Score 함수 | Query-Key 간 유사도를 계산하는 함수. dot product, additive (feed-forward net), scaled dot-product 등이 있음 |
| Softmax / 가중치(weight) | Score들을 정규화해서 “얼마나 주목(attend)”해야 할지를 결정함. 값들은 0~1 사이이고 전체 합 = 1 |
| Context vector (문맥 벡터) | Value 벡터들에 attention 가중치를 곱한 후 합한 결과. 디코더의 출력 예측 등에 쓰임 |

## 수식

1. 유사도 계산 (score 함수):
   - Dot-product: $e_{t,i} = q_t^\top h_i$
   - Additive: $e_{t,i} = v^\top \tanh(W_q q_t + W_h h_i)$
2. 정규화 (가중치):
   $$
   \alpha_{t,i} = \frac{\exp(e_{t,i})}{\sum_j \exp(e_{t,j})}
   $$
3. 문맥 벡터 계산:
   $$
   c_t = \sum_i \alpha_{t,i} \, h_i
   $$

## Self-Attention

### 개념
- 쿼리, 키, 값을 **같은 시퀀스에서 생성**하는 Attention
- 입력 시퀀스의 각 단어가 **동일 시퀀스 내 다른 단어들과 관계를 학습**합니다
- 긴 의존성(Long-term dependency)을 효율적으로 포착 가능

### 수식 (Scaled Dot-Product Attention)

$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{Q K^\top}{\sqrt{d_k}}\right)V$

- $Q = X W_Q$, $K = X W_K$, $V = X W_V$  
- $X$: 입력 시퀀스 임베딩  
- $d_k$: Key 벡터 차원 (스케일링 안정화 목적)

## Multi-Head Attention

### 아이디어

- 단일 Attention만으로는 다양한 관계를 포착하기 어려움
- 여러 개의 서로 다른 **헤드(head)** 를 두어 병렬로 Attention 수행 후 결과를 결합

### 과정

1. 입력 $X$에 대해 $h$개의 쿼리/키/값 프로젝션 생성  
   $Q_i = X W_Q^i$, $K_i = X W_K^i$, $V_i = X W_V^i$

2. 각 헤드에서 Attention 계산  
   $Z_i = \text{Attention}(Q_i, K_i, V_i)$

3. 모든 헤드 결과를 연결(concatenate) 후 선형 변환  
   $Z = [Z_1; Z_2; \dots; Z_h] W_O$


## 장점 및 한계  

| 장점 | 한계 / 고려사항 |
|------|------------------|
| 입력의 중요한 부분에 집중할 수 있어 성능 향상 (특히 긴 문장, 복잡한 문맥) 가능함 | 계산 비용(computation cost) 및 메모리 사용량이 올라감. 특히 시퀀스 길이가 길면 attention 행렬 크기가 커짐 |
| 병렬 처리 가능성 증대 (특히 Transformer 기반) → 학습 속도 개선 가능 | “Attention weights = 설명(explanation)” 이라는 해석이 항상 옳은 건 아님. 가중치가 주목도를 보여주는 지표이긴 하지만, 모델의 전체 내부 작용을 설명해 주는 건 아니야 |
| 장기 의존성(long-term dependency)을 더 잘 학습 가능함 | 입력 길이 증가 시 계산 복잡도가 제곱(quadratic) 수준으로 커질 수 있음 (예: self-attention). 최적화 필요 |

## 활용 예시  

- 기계 번역 (Machine Translation) — 입력 문장의 각 단어들이 번역될 때 어느 단어들이 중요한지 attend 함

- 문서 요약(Text Summarization) / 질의응답(Question Answering) — 핵심 문장이나 단어 강조  

- 언어 모델링 및 대화 모델 — 문맥(context)의 더 먼 부분까지 참조 가능

- 비전(Vision) / 이미지 처리 — 이미지 내 중요한 영역(attend region) 강조 (예: 이미지 캡션, 비전 트랜스포머)

## 요약  

Attention은 딥러닝에서 입력의 여러 부분 중 중요한 것에 집중함으로써 더 효율적이고 유연한 학습 및 추론을 가능하게 해 주는 메커니즘입니다

단순한 Seq2Seq 구조의 정보 병목 문제를 완화하고, Transformer 등 현대 언어 및 비전 모델들의 핵심 요소로 자리잡았지. 다만 계산 자원, 해석 가능성(explainability) 등에서는 여전히 고려해야 할 점들이 있습니다

## 참고자료

- [3Blue1Brown Youtube - 그 이름도 유명한 어텐션](https://www.youtube.com/watch?v=_Z3rXeJahMs)

<br>

> 작성 - `2025.09.17`<br>
> 마지막 수정 - `2025.09.17`
