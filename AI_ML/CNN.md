# CNN (Convolutional Neural Network)

## 개요

Convolutional Neural Network(CNN)는 이미지, 영상, 시계열 등 **격자형 데이터(grid-like data)**를 처리하도록 설계된 딥러닝 모델입니다

CNN의 핵심은 **공간적 계층 구조(spatial hierarchy)**를 학습하고, **지역 패턴(local features)**을 효율적으로 추출하는 데 있습니다

## CNN 주요 레이어

<img src="https://i.ibb.co/chz5F6x1/image.png" alt="image" border="0">

### Convolutional Layer

- 입력 이미지에서 **지역적 특징(feature)** 추출
- 커널(kernel)을 이용해 입력 이미지와 **합성곱 연산(convolution)** 수행
- 필터/커널을 입력 행렬의 가능한 모든 위치로 이동<br>
  입력 이미지의 필터 크기 패치와 필터 간의 요소별 곱셈을 수행 후 합산

<img src="https://i.ibb.co/5X5xsxXS/image.png" alt="image" border="0">

<img src="https://i.ibb.co/xtqxjQBh/1-wvw-T9-Ll-Hc-Dfex-Iiv7f-WWx-A.gif" alt="1-wvw-T9-Ll-Hc-Dfex-Iiv7f-WWx-A" border="0">

```python
import torch.nn as nn

conv_layer = nn.Conv2d(in_channels=3, out_channels=16, kernel_size=3, stride=1, padding=1)
```

### Pooling layer

- Convolution 후 얻은 feature map을 작게 줄이면서 중요한 정보만 남기는 레이어
- 위치 변화(translation)에 강건성을 높이고, 계산량도 줄임
- 종류
  - Max Pooling: 영역 내 가장 큰 값 선택 → 가장 중요한 특징 유지
  - Average Pooling: 영역 내 평균값 → 부드러운 정보 유지
- modern CNN 구조에는 잘 사용되지 않음

<a href="https://ibb.co/6cyb8b9k"><img src="https://i.ibb.co/wZdC7CGf/image.png" alt="image" border="0"></a>

```python
pool_layer = nn.MaxPool2d(kernel_size=2, stride=2)
```

### Batch Normalization Layer (배치 정규화)

- 각 레이어 입력의 평균과 분산을 정규화(normalization)
- 학습을 안정화하고 속도를 높임
- 과정
  1. 배치 단위 평균 $μ$, 분산 $σ$ 계산
  2. 각 값 $x$를 정규화: $(x - μ) / sqrt(σ^2 + ε)$
  3. 학습 가능한 스케일 $γ$와 시프트 $β$ 적용

```python
bn_layer = nn.BatchNorm2d(num_features=16)
```

### Fully Connected Layer

- Convolution + Pooling으로 추출된 feature를 1차원 벡터로 펼쳐 최종 분류
- 모든 뉴런이 이전 층의 모든 출력과 연결
- 전통적인 MLP 구조와 동일

```python
fc_layer = nn.Linear(in_features=32*8*8, out_features=10)
```

### Global Average Pooling (GAP)

- Feature map 전체를 채널별 평균으로 압축
- 1x1xC 형태 → FC 레이어 대비 파라미터 적음, 과적합 방지
- 이미지 분류 마지막 단계에 자주 사용

```python
gap_layer = nn.AdaptiveAvgPool2d(1)  # HxW -> 1x1
```

## PyTorch CNN 예제

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class SimpleCNN(nn.Module):
    def __init__(self, num_classes=10):
        super(SimpleCNN, self).__init__()
        # Conv Layer 1
        self.conv1 = nn.Conv2d(3, 16, kernel_size=3, stride=1, padding=1)
        self.bn1 = nn.BatchNorm2d(16)

        # Conv Layer 2
        self.conv2 = nn.Conv2d(16, 32, kernel_size=3, stride=1, padding=1)
        self.bn2 = nn.BatchNorm2d(32)
        
        # Pooling
        self.pool = nn.MaxPool2d(2, 2)
        
        # Fully connected
        self.fc = nn.Linear(32*8*8, num_classes)

    def forward(self, x):
        # Conv Block 1
        x = self.conv1(x)
        x = self.bn1(x)
        x = F.relu(x)
        x = self.pool(x)

        # Conv Block 2
        x = self.conv2(x)
        x = self.bn2(x)
        x = F.relu(x)
        x = self.pool(x)

        x = x.view(x.size(0), -1)  # flatten
        x = self.fc(x) # Fully Connected Layer
        return x

model = SimpleCNN()
print(model)
```

- 입력: (batch_size, 3, 32, 32)
- 출력: (batch_size, num_classes)

## CNN의 시각적 이해

```scss
Input Image (32x32x3)
        │
   Conv(3x3,16) + ReLU
        │
   MaxPool(2x2)
        │
   Conv(3x3,32) + ReLU
        │
   MaxPool(2x2)
        │
Flatten (32*8*8)
        │
Fully Connected → Output (10 classes)
```

- Conv → Feature Map
- Pooling → 공간 축소, 위치 강건성
- FC → 최종 분류

## 참고자료

- [Convolutional neural network란? | 꼭 알아야 할 3가지 사항 - MATLAB & Simulink](https://kr.mathworks.com/discovery/convolutional-neural-network.html)
- [Deep Learning Bible - 2. Classification](https://wikidocs.net/164365)
- [Convolutional Neural Network — An Informal Introduction (Part-1)](https://medium.com/analytics-vidhya/convolutional-neural-network-an-informal-intro-part-1-db9fca86a750)

<br>

> 작성 - `2025.09.25`<br>
> 마지막 수정 - `2025.09.25`