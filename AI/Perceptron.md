# 퍼셉트론 (Perceptron)

## 개요

퍼셉트론은 인공 신경망의 가장 기본적인 형태로, 1957년 Frank Rosenblatt에 의해 제안되었습니다

이는 이진 분류 문제를 해결하기 위한 선형 분류기로, 단일 계층의 인공 뉴런으로 구성됩니다

## 구조

퍼셉트론은 다음과 같은 구성 요소로 이루어져 있습니다:

- **입력층 (Input Layer)**: 입력 벡터
  - $\mathbf{x} = [x_1, x_2, \dots, x_n]$
- **가중치 벡터 (Weight Vector)**: 각 입력에 대한 가중치
  - $\mathbf{w} = [w_1, w_2, \dots, w_n]$
- **편향 (Bias)**: 결정 경계를 조정하는 추가 파라미터 ($b$)
- **활성화 함수 (Activation Function)**: 입력의 가중합에 기반하여 출력을 결정하는 함수

<img src="https://i.ibb.co/HpCkwnHp/image.png" alt="image" border="0">

## 동작 원리

퍼셉트론은 다음과 같은 방식으로 동작합니다

1. **가중합 계산**: 입력 벡터와 가중치 벡터의 내적에 편향을 더합니다
   
   $z = \mathbf{w} \cdot \mathbf{x} + b$

2. **활성화 함수 적용**: 계산된 가중합 $z$에 Heaviside 계단 함수를 적용하여 출력을 결정합니다
   
   $y = \begin{cases}
   1 & \text{if } z \geq 0 \\
   0 & \text{if } z < 0
   \end{cases}$

   이는 이진 분류를 위한 결정 기준을 제공합니다

   <img src="https://i.ibb.co/1tHtmg4d/image.png" alt="image" border="0">

## 한계점

- **선형 분리 가능성**: 퍼셉트론은 선형적으로 분리 가능한 데이터에 대해서만 작동합니다. `XOR` 문제와 같은 비선형 데이터셋에서는 수렴하지 않습니다.

- **다중 클래스 분류**: 기본 퍼셉트론은 이진 분류에만 적용 가능하며, 다중 클래스 분류를 위해서는 추가적인 구조가 필요합니다.

## 확장: 다층 퍼셉트론 (MLP)

퍼셉트론의 한계를 극복하기 위해 여러 개의 퍼셉트론을 층으로 쌓은 다층 퍼셉트론(Multi-Layer Perceptron, MLP)이 개발되었습니다. MLP는 은닉층을 추가하여 비선형 분류 문제를 해결할 수 있으며, 역전파 알고리즘을 통해 학습됩니다.

<img src="https://i.ibb.co/JXFhqc4/image.png" alt="image" border="0">

## 참고 자료

- [Wikipedia: Perceptron](https://en.wikipedia.org/wiki/Perceptron)
- [GeeksforGeeks](https://www.geeksforgeeks.org/machine-learning/what-is-perceptron-the-simplest-artificial-neural-network/)
- [딥 러닝을 이용한 자연어 처리 입문](https://wikidocs.net/24958)

<br>

> 작성 - `2025.09.16`<br>
> 마지막 수정 - `2025.09.16`