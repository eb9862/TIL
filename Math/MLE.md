# 최대가능도추정법 (Maximum Likelihood Estimation, MLE)

## 개요

- **최대가능도추정법(MLE)**은 통계적 모형에서 **모수(parameter)**를 추정하는 대표적인 방법입니다
- 관측된 데이터가 주어졌을 때, 그 데이터가 가장 "그럴듯하게(likely)" 나올 수 있도록 만드는 모수 값을 찾습니다
- 즉, **데이터를 가장 잘 설명하는 모수를 선택**하는 방법

## 기본 개념

### 확률모형

- 확률변수 $X$가 분포 $p(x|\theta)$를 따른다고 가정 ($\theta$: 모수)
- 관측 데이터: $x_1, x_2, \dots, x_n$

### 가능도 함수 (Likelihood Function)

- 관측된 데이터가 주어졌을 때, 모수 $\theta$에 대한 "가능도"를 정의합니다

  - $
L(\theta; x_1, \dots, x_n) = \prod_{i=1}^n p(x_i | \theta)
$

- 로그가능도(Log-Likelihood): 계산 편의를 위해 로그를 취합니다 (곱 -> 합)

  - $
\ell(\theta) = \log L(\theta) = \sum_{i=1}^n \log p(x_i | \theta)
$

## 최대가능도추정 (MLE)

### 정의

- MLE는 가능도 함수 $L(\theta)$를 최대로 만드는 $\theta$를 찾는 방법입니다

  - $
\hat{\theta}_{MLE} = \arg\max_{\theta} L(\theta; x_1, \dots, x_n)
$

- 보통 로그가능도를 사용하여 최적화합니다

  - $
\hat{\theta}_{MLE} = \arg\max_{\theta} \ell(\theta)
$

<br>

> 참고자료 - [공돌이의 수학정리노트 (Angelo's Math Notes)](https://angeloyeo.github.io/2020/07/17/MLE.html)<br>
> 2025.09.10 작성