# Word2Vec

## 개요

Word2Vec은 단어를 고정 길이의 실수 벡터 공간에 임베딩(embedding)하는 기법입니다

이 벡터 표현을 통해 단어 사이의 유사성, 의미 관계 등을 정량적으로 다룰 수 있으며, 자연어 처리(NLP) 응용에 널리 사용됩니다

## 배경 및 필요성

전통적으로는 단어를 원-핫 인코딩(one-hot encoding)으로 표현했다. 하지만 이 방식은 다음과 같은 한계를 갖습니다
- 차원이 매우 크고 희소(sparse)하다
- 단어 간 유사성을 전혀 반영하지 못한다 (예: "king"과 "queen"은 벡터 상에서 완전히 독립적이다)

Word2Vec은 이런 문제를 해결하기 위해 단어를 **의미 공간(semantic space)** 안의 좌표로 배치합니다

이 좌표에서는 비슷한 의미의 단어가 가까이 위치하고, 관계가 있는 단어는 특정 방향성을 공유합니다

## 기본 구조 및 원리  

Word2Vec에는 크게 두 가지 아키텍처가 있다:

1. **CBOW (Continuous Bag of Words)**  
2. **Skip-gram**

각 구조와 학습 방식은 다음과 같습니다

### CBOW 방식

- 주어진 **문맥 단어들(context words)** 을 입력으로 받아 **중앙 단어(target word)** 를 예측하는 방식
- 예: 문장 “the **cat** sits on the mat”에서
  - 입력: ["the", "sits", "on", "the"]
  - 출력: "cat"
- 문맥 → 중앙 단어 예측  
- 장점: 학습이 빠르고, 자주 등장하는 단어 학습에 유리 

### Skip-gram 방식

- **중앙 단어**를 입력으로 받아 문맥 단어들을 예측하는 방식  
- 예: 문장 “the **cat** sits on the mat”에서  
  - 입력: "cat"
  - 출력: ["the", "sits", "on", "the"]
- 중앙 단어 → 문맥 예측  
- 장점: 희소 단어(low-frequency word)에 대해 더 잘 일반화

<img src="https://i.ibb.co/BVgk9htg/image.png" alt="image" border="0">

## 학습 방식과 최적화  

Word2Vec은 확률적 모델을 사용합니다.
즉, “주어진 단어가 문맥 속에 등장할 확률”을 계산하여 벡터를 학습합니다.
하지만 모든 단어를 대상으로 확률을 계산하면 비용이 너무 큽니다

이를 해결하기 위해 두 가지 최적화 기법이 제안되었습니다

- **Negative Sampling**  
  - 정답 단어(positive) 외에 몇 개의 무작위 단어(negative)를 뽑아 구분하는 방식  
  - 학습 속도를 크게 개선합니다

- **Hierarchical Softmax**
  - 단어를 트리 구조로 표현하고, 루트에서 리프까지의 경로를 따라가며 확률을 계산
  - 자주 등장하는 단어일수록 경로가 짧아 계산이 효율적입니다

## 수학적 직관  

Word2Vec의 목표는 다음과 같습니다

- 단어 $w$ 를 벡터 $v_w$ 로 표현
- “문맥 단어들이 주어졌을 때, 특정 단어가 등장할 확률”을 높이도록 학습

예를 들어 Skip-gram에서는,

$P(context \mid word) = \frac{\exp(v_{word}^\top v_{context})}{\sum_{w'} \exp(v_{word}^\top v_{w'})}$

하지만 실제로는 이 복잡한 식을 Negative Sampling 같은 근사 방법으로 단순화하여 학습합니다

## 한계  

Word2Vec은 뛰어난 아이디어였지만 몇 가지 단점이 있습니다

- 문맥 순서 반영 불가: CBOW/Skip-gram 모두 단어 순서를 무시
- 다의어 문제: "bank" (강둑 vs 은행) 같은 단어를 하나의 벡터로만 표현
- OOV(out-of-vocabulary): 학습 시 보지 못한 단어는 벡터를 만들 수 없음
- 충분한 양의 텍스트 말뭉치가 있어야 함

## 발전된 방법들

Word2Vec 이후, 여러 한계를 극복하기 위한 기법들이 등장했습니다

- **GloVe**: 전역 단어 통계(global co-occurrence)를 반영
- **FastText**: 단어를 subword(부분 단어) 단위로 쪼개어 OOV 문제 해결
- **ELMo, BERT 등**: 문맥에 따라 단어 벡터가 달라지는 **문맥적 임베딩(Contextual Embedding)**

## 요약

- Word2Vec은 단어를 연속적인 벡터로 변환하는 방법입니다
- CBOW와 Skip-gram 두 가지 구조가 있으며, Negative Sampling 같은 기법으로 학습 효율을 높일 수 있습니다
- 단어 간 의미 관계를 벡터 연산으로 다룰 수 있어 NLP에 큰 기여를 했습니다
- 그러나 순서, 다의어, OOV 문제 등 한계가 있어 후속 연구들이 발전해왔습니다

## 참고자료

- [딥 러닝을 이용한 자연어 처리 입문 - Word2Vec](https://wikidocs.net/22660)
- [Efficient Estimation of Word Representations in Vector Space](https://arxiv.org/abs/1301.3781)

<br>

> 작성 - `2025.09.29`<br>
> 마지막 수정 - `2025.09.29`