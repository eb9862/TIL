# numpy.random 모듈

`numpy.random` 모듈은 난수 생성과 관련된 다양한 함수를 제공합니다

## 난수 생성 시드 설정

### [`numpy.random.seed`](https://numpy.org/doc/2.3/reference/random/generated/numpy.random.seed.html)

- 기능: 난수 생성기를 초기화하여 **재현 가능한 난수**를 생성할 수 있게 함
- 사용법
    ```python
    import numpy as np

    np.random.seed(42)  # 시드를 고정하여 동일한 난수 생성
    ```

## 균등분포 난수 생성

### [`numpy.random.rand`](https://numpy.org/doc/2.3/reference/random/generated/numpy.random.rand.html)

- 기능: 0~1 구간의 **균등분포** 난수를 생성
- 사용법
    ```python
    np.random.rand(3, 2)  # 3x2 행렬, 0~1 난수
    ```

### [`numpy.random.uniform`](https://numpy.org/doc/2.3/reference/random/generated/numpy.random.uniform.html)

- 기능: 지정한 구간 [low, high)의 균등분포 난수를 생성
- 사용법
    ```python
    np.random.uniform(low=0.0, high=10.0, size=(3, 2))
    ```

## 정규분포(가우시안) 난수 생성

### [`numpy.random.randn`](https://numpy.org/doc/2.3/reference/random/generated/numpy.random.randn.html)

- 기능: 표준 정규분포 (평균=0, 표준편차=1) 난수 생성
- 사용법
    ```python
    np.random.randn(3, 2)  # 3x2 행렬, N(0,1)
    ```

### [`numpy.random.normal`](https://numpy.org/doc/2.3/reference/random/generated/numpy.random.normal.html)

- 기능: 지정한 평균(mean)과 표준편차(std)의 정규분포 난수 생성
- 사용법
    ```python
    np.random.normal(loc=5.0, scale=2.0, size=(3, 2)) # loc: 평균, scale: 표준편차
    ```

## 배열 요소 섞기

### [`numpy.random.shuffle`](https://numpy.org/doc/2.3/reference/random/generated/numpy.random.shuffle.html)

- 기능: 배열의 요소를 제자리에서 랜덤하게 섞음
- 사용법 (원본 배열 직접 변경)
    ```python
    arr = np.array([1, 2, 3, 4, 5])
    np.random.shuffle(arr)
    # arr이 랜덤하게 섞임
    ```

### [`numpy.random.permutation`](https://numpy.org/doc/2.2/reference/random/generated/numpy.random.permutation.html)


- 기능: 배열의 요소를 랜덤하게 섞음
- 사용법 (`shuffle`과 다르게 원본을 유지하고 새로운 배열 반환)

```python
np.random.permutation(5) # [3 1 5 4 2]
np.random.permutation([1, 4, 9, 12, 15]) #[15,  1,  9,  4, 12]
```

<br>

> 작성 - `2025.09.12`<br>
> 마지막 수정 - `2025.09.15`