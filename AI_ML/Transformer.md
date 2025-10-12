# Transformer

## 개요
Transformer는 2017년 구글 브레인 팀이 발표한 논문 `Attention Is All You Need`에서 소개된 모델로, 
이전까지 주로 사용되던 RNN(Recurrent Neural Network)이나 CNN(Convolutional Neural Network)을 사용하지 않고,  
**Attention 메커니즘만으로 시퀀스 데이터를 처리**하는 혁신적인 구조입니다

이후 BERT, GPT, T5 등 수많은 파생 모델의 기반이 되었고, 현재는 NLP뿐 아니라 Vision, Speech, 멀티모달 영역까지 확장되어 있습니다

## Transformer의 핵심 아이디어

- **순차 연산 제거**: RNN처럼 순차적으로 데이터를 처리하지 않고 병렬 연산이 가능 → 학습 속도 대폭 향상
- **Self-Attention**: 입력 토큰들이 서로 어떤 관계가 있는지 직접 학습
- **장기 의존성(Long-range dependency)** 문제 해결: 문장이 길어져도 앞뒤 단어 간의 관계를 잘 학습할 수 있음

## 전체 구조

Transformer는 **Encoder-Decoder 구조**로 이루어져 있습니다

- Encoder Stack을 통과하여 입력 문장에 대한 문맥 정보가 완벽하게 압축된 문맥 표현(context representation)이 생성됩니다
- 이는 Decoder Stack에 전달되어 모델이 문장의 끝을 의미하는 토큰을 생성할 때까지 계속해서 반복됩니다

```
Input → [Encoder Stack] → 중간 표현 z → [Decoder Stack] → Output
```

<img src="https://i.ibb.co/62Gskz9/image.png" alt="image" border="0">

## Encoder 이해하기

- N=6개의 동일한 레이어로 구성
- 각 레이어는 두 부분으로 이루어집니다
  1. **Multi-Head Self-Attention**: 입력 문장 내 단어들 간의 관계 학습
  2. **Feed-Forward Network (FFN)**: 각 위치별 비선형 변환
- 각 부분 뒤에는
  1. **Residual Connection**: 원래 입력을 더해 정보 손실 방지
  2. **Layer Normalization**: 학습 안정화

### Self-Attention

- 각 단어(토큰)를 Query(Q), Key(K), Value(V)로 변환
- 유사도(QK^T)를 계산해 **해당 단어가 문맥에서 다른 단어를 얼마나 "참조"해야 하는지** 결정
- Value를 가중합해 새로운 표현 생성
- 문장 안에서 각 단어가 자기 자신과 다른 단어들을 어떻게 바라볼지를 학습하는 과정

<img src="https://i.ibb.co/cKbMC8mG/image.png" alt="image" border="0"></a>

- $\sqrt{d_k}$: 차원에 따라 값이 커지는 문제를 방지하기 위한 스케일링
- Softmax: 유사도를 확률로 변환

### Multi-Head Attention

- Attention을 여러 번(head) 나누어 병렬로 수행
- 각 head는 서로 다른 관점을 학습 (예: 의미적 관계, 문법적 관계)
- 모든 head를 이어붙여 다시 projection

<img src="https://i.ibb.co/KjrhCm8R/2025-09-25-9-57-34.png" alt="2025-09-25-9-57-34" border="0">

### FFN (Feed Forward Network)
- Self-Attention이 토큰 간 관계를 통합한 뒤, FFN은 각 위치의 표현을 개별적으로 비선형 변환하여 표현력을 확장
- 각 위치별로 독립적으로 2층 MLP 적용:

$FFN(x) = max(0, xW_1 + b_1)W_2 + b_2$

## Decoder 이해하기

- N=6개의 동일한 레이어로 구성
- Decoder는 Encoder와 유사하지만 **2가지 차이점**이 있습니다
  1. **Masked Multi-Head Self-Attention**: 미래 단어를 가리지 않고 현재까지만 참조 (언어 모델링 특성 반영)
  2. **Encoder-Decoder Attention**: Encoder의 출력과 Decoder의 입력을 연결
- 역시 Residual Connection + LayerNorm 적용

### Masked Multi-Head Self-Attention

- 디코더가 다음 단어를 예측할 때 미래 단어를 참조하지 못하도록 마스크를 적용
- attention score 계산 시 아직 생성되지 않은 단어의 위치를 `-∞`로 채워 softmax 결과가 0이 되도록 만듦
- 이 구조 덕분에 모델은 “현재 시점까지의 단어”만 보고 다음 단어를 생성

### Encoder-Decoder Attention (Cross-Attention)

- 디코더의 Query(Q)가 인코더의 출력에서 생성된 Key(K)와 Value(V)를 참고
- 즉, 디코더는 인코더의 문맥 정보를 참조하면서 다음 단어를 생성
- 예를 들어 번역 모델이라면, 입력 문장의 각 단어가 현재 생성 중인 단어에 얼마나 관련이 있는지를 학습

## 참고자료

- [Attention is all you need](https://arxiv.org/abs/1706.03762)

<br>

> 작성 - `2025.09.25`<br>
> 마지막 수정 - `2025.10.12`