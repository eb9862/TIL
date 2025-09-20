# RNN (Recurrent Neural Network)

## 개요

RNN(Recurrent Neural Network)은 순차적(sequence)인 데이터 처리에 특화된 신경망 구조입니다

시간(t) 또는 순서가 있는 입력 데이터(예: 자연어, 시계열 데이터)를 다룰 때, 이전 입력/상태(history)가 이후의 출력에 영향을 주도록 설계되어 있습니다

<img src="https://i.ibb.co/354kcBJz/image.png" alt="image" border="0">

## 배경 및 맥락

- 전통적인 피드포워드 신경망(feedforward neural network)은 입력 → 출력만 고려하고, 입력 순서나 시간적 연관성을 반영할 수 없음

- 하지만 언어 모델, 음성 인식, 시계열 예측 등의 문제에서는 “과거의 정보”가 중요합니다
  예: 문장에서 앞 단어들이 뒤 단어의 의미를 결정함

- RNN은 이런 과거의 정보를 내부 상태(hidden state)에 저장하고 순차적으로 업데이트하면서, 시간적/순차적 특징을 학습할 수 있게 해줍니다

## 주요 개념 및 구성요소

| 구성 요소 | 설명 |
|--------|------|
| 입력 $x_t$ | 시퀀스의 각 시점 $t$에서의 입력 벡터 |
| 숨겨진 상태 $h_t$ | 과거 입력들을 요약한 내부 메모리 역할을 하는 벡터 |
| 출력 $y_t$ | 시점 $t$에서의 예측값 또는 결과 |
| 파라미터 $W_{ih}, W_{hh}, W_{ho}, b_h, b_o$ | 입력→은닉, 은닉→은닉, 은닉→출력 가중치 및 편향 등 |

## 동작 원리 (과정)  

1. **초기화**  
   - 초기 숨겨진 상태 $h_0$를 설정 (보통 0 벡터 혹은 학습 가능한 초기값)

2. **순전파(Forward Pass)**  
   - 각 시점 \(t\)마다:  
     - $h_t = f(W_{ih} x_t + W_{hh} h_{t-1} + b_h)$  
      - $y_t = g(W_{ho} h_t + b_o)$
    - 여기서 $f$, $g$는 활성화 함수(예: tanh, ReLU, softmax 등)

3. **역전파(Backpropagation Through Time, BPTT)**  
   - 손실(loss)을 기준으로 시간축을 따라 파라미터들에 대한 그래디언트를 계산하고 업데이트
   - 긴 시퀀스에서는 그래디언트 소실(vanishing) 또는 폭발(exploding) 문제가 발생할 수 있습니다 

4. **문제와 한계**  
   - 장기 의존성(long-term dependency)을 잘 학습 못함 → LSTM, GRU 같은 변형 등장
   - 계산 비용과 메모리 사용량 증가
   - 병렬 처리 어려움 (순차적 특성 때문) 

## 예시 코드

아래는 PyTorch의 `nn.RNN` 모듈을 사용한 간단한 예시로

임의의 시퀀스를 입력으로 받아, 은닉 상태를 출력하는 과정을 보여줍니다

```python
import torch
import torch.nn as nn

# 하이퍼파라미터
input_size = 5     # 입력 벡터 차원
hidden_size = 10   # 은닉 상태 차원
num_layers = 1     # RNN 레이어 수
seq_length = 7     # 시퀀스 길이
batch_size = 3     # 배치 크기

# RNN 모델 정의
rnn = nn.RNN(input_size, hidden_size, num_layers, batch_first=True)

# 임의 입력 (batch_size, seq_length, input_size)
x = torch.randn(batch_size, seq_length, input_size)

# 초기 은닉 상태 (num_layers, batch_size, hidden_size)
h0 = torch.zeros(num_layers, batch_size, hidden_size)

# 순전파 실행
out, hn = rnn(x, h0)

print("입력 크기:", x.shape)       # (3, 7, 5)
print("출력 크기:", out.shape)     # (3, 7, 10) → 모든 시점의 은닉 상태
print("마지막 은닉 상태 크기:", hn.shape)  # (1, 3, 10)
```

---
<br>

## LSTM (Long Short-Term Memory) 

<img src="https://i.ibb.co/h1BDTqHP/image.png" alt="image" border="0">

### 등장 배경  

단순 RNN은 **장기 의존성(long-term dependency)** 문제를 잘 처리하지 못합니다

- 시퀀스가 길어질수록 초반 정보가 역전파 과정에서 소실(vanishing gradient)되거나 폭발(exploding gradient)하는 문제가 발생

- LSTM은 **게이트 구조(gating mechanism)** 를 추가하여, 어떤 정보를 유지할지/버릴지 선택적으로 조절할 수 있게 설계된 RNN 변형

### 추가된 핵심 구성요소  

| 구성 요소 | 역할 |
|-----------|------|
| **Cell state** ($C_t$) | 시퀀스 전반에 걸쳐 정보를 장기적으로 보존하는 통로. RNN의 hidden state보다 더 직접적인 정보 흐름을 제공함 |
| **Forget gate** ($f_t$) | 과거 정보 중에서 어떤 것을 잊을지 결정 |
| **Input gate** ($i_t$) | 현재 입력에서 어떤 새로운 정보를 기억할지 결정 |
| **Candidate cell state** ($\tilde{C}_t$) | 새로운 후보 정보(현재 입력을 tanh 변환) |
| **Output gate** ($o_t$) | 현재 cell state에서 어떤 정보를 출력(hidden state)으로 보낼지 결정 |


### 예시 코드

아래 예시는 PyTorch의 `nn.LSTM` 모듈을 사용하여 간단한 시퀀스 데이터를 처리하는 방법입니다

```python
import torch
import torch.nn as nn

# 입력 차원, 은닉 차원, LSTM 레이어 개수
input_size = 10    # 입력 벡터 크기
hidden_size = 20   # hidden state 크기
num_layers = 2     # LSTM layer 수

# batch_first=True → 입력 텐서 형태: (batch, seq_len, input_size)
lstm = nn.LSTM(input_size, hidden_size, num_layers, batch_first=True)

# 예시 입력 데이터: 배치 크기=5, 시퀀스 길이=7, 입력 차원=10
x = torch.randn(5, 7, 10)

# 초기 hidden state, cell state (모두 0으로 초기화)
h0 = torch.zeros(num_layers, 5, hidden_size)
c0 = torch.zeros(num_layers, 5, hidden_size)

# 순전파(forward pass)
output, (hn, cn) = lstm(x, (h0, c0))

print("output shape:", output.shape)  # (5, 7, 20)
print("hn shape:", hn.shape)          # (2, 5, 20) → (num_layers, batch, hidden_size)
print("cn shape:", cn.shape)          # (2, 5, 20)
```

### 직관적 이해  
- **Cell state = 정보 고속도로**
  - 정보가 전혀 바뀌지 않고 그대로만 흐르게 하는 부분
  - State가 꽤 오래 경과하더라도 Gradient가 잘 전파됨
- **게이트 = 조절 밸브**: 어떤 정보를 남기고, 잊고, 내보낼지 제어

### 장점  
- RNN의 장기 의존성 문제 완화 → 긴 시퀀스도 학습 가능  
- 기계 번역, 음성 인식, 텍스트 생성 등에서 우수한 성능  

### 한계  
- 구조가 복잡하고, 계산량이 커서 학습 비용 증가  
- 최근에는 더 단순한 **GRU**나 더 강력한 **Transformer**가 대체되는 경우 많음  

## 참고자료

- [CS230 - 딥러닝: 순환신경망(RNN)](https://stanford.edu/~shervine/l/ko/teaching/cs-230/cheatsheet-recurrent-neural-networks)
- [한땀한땀 딥러닝 컴퓨터 비전 백과사전 - LSTM](https://wikidocs.net/152773)

<br>

> 작성 - `2025.09.17`<br>
> 마지막 수정 - `2025.09.17`
