# `torchvision.transforms`

딥러닝에서 이미지 모델의 성능은 데이터의 다양성과 품질에 크게 좌우됩니다. 하지만 실제 데이터셋은 한정적이고, 원본 이미지만으로는 모델이 일반화하기 어렵습니다.  

이를 해결하기 위해 PyTorch는 `torchvision.transforms` 모듈을 제공하여 **이미지 전처리와 데이터 증강**을 쉽게 적용할 수 있습니다.

#### 주요 목적

- 이미지 전처리 (크기 조정, 정규화, 채널 변환 등)
- 데이터 증강 (랜덤 회전, 색상 변화, 뒤집기 등)
- PIL 이미지, Tensor 이미지, 또는 이미지 배치(batch of tensors)를 다룰 수 있게 지원
- `functional` 변환을 통해 더 세부적 조작 가능함

➡️ train dataset의 다양성을 확보하여 reat data distribution과 비슷한 데이터를 만듦

## 주요 Transform 클래스 / 기능

아래는 자주 쓰이는 변환(transform) 클래스들의 주요 기능과 옵션입니다

| 변환 이름 | 기능 요약 | 주요 파라미터 / 주의점 |
|---|---|---|
| `Resize` | 이미지 크기를 조정함 | `size` (정수 혹은 `(h, w)`), `interpolation` 모드 지정 가능|
| `CenterCrop` | 이미지 중앙을 기준으로 crop | `size` 지정; 이미지가 작을 경우 패딩(padding) 후 crop |
| `RandomResizedCrop` | 무작위 위치 + 비율(aspect ratio)로 잘라낸 후 크기 조정 | `size`, `scale`, `ratio`, `interpolation` 등 설정 가능 |
| `RandomHorizontalFlip` / `RandomVerticalFlip` | 확률적으로 좌우 혹은 상하 반전 | `p` (확률) 지정 |
| `RandomRotation` | 특정 각도 범위 내에서 무작위 회전 | `degrees`, `interpolation`, `expand`, `center`, `fill` 등 주의사항 있음 |
| `ColorJitter` | 밝기(brightness), 대비(contrast), 채도(saturation), 색조(hue) 무작위 변화 | 각 요소의 변화 범위 지정 가능 |
| `Grayscale` / `RandomGrayscale` | 회색조 변환; 무작위 또는 고정 | 출력 채널 수(`num_output_channels`) 조정 가능 |
| `GaussianBlur` | 이미지 블러링 | `kernel_size`, `sigma` 범위 지정 가능 |
| `Normalize` | 텐서 이미지 정규화 (평균/표준편차 기반) | 입력은 Tensor이어야 함; PIL 이미지에서는 지원하지 않음 |
| `RandomErasing` | 이미지 내 임의 영역을 지우거나 대체값 적용 | `p`, `scale`, `ratio`, `value` 설정 가능 |
| 변환간 합성/변형/보조 기능 | `Compose`, `Lambda`, `TenCrop`, `Pad` 등 | `Lambda` 사용 시 TorchScript 비호환 가능; `TenCrop`은 여러 개 이미지 반환함에 유의 |

### 예제 코드

```python
import torch
from torchvision import transforms
from PIL import Image

transform_pipeline = transforms.Compose([
    transforms.Resize((256, 256)),
    transforms.RandomHorizontalFlip(p=0.5),
    transforms.ColorJitter(brightness=0.2, contrast=0.2, saturation=0.2),
    transforms.RandomRotation(degrees=15),
    transforms.ToTensor(),
    transforms.Normalize(mean=(0.485, 0.456, 0.406),
                         std=(0.229, 0.224, 0.225))
])

img = Image.open('path/to/image.jpg')
tensor_img = transform_pipeline(img)
```

### 결과 예시

<img src="https://i.ibb.co/Y7wwDdzm/image.png" alt="image" border="0">

## `transforms.functional`

위에서 살펴본 방식은 한 번 정의하여 반복적으로 사용이 가능한 장점은 있지만, 직접적으로 이미지를 다룰 때 매번 선언하는 것이 번거로울 수 있습니다

### 예제 1: 클래스 기반 vs functional

```python
from torchvision import transforms
import torchvision.transforms.functional as F
from PIL import Image

# 동일한 작업을 클래스 기반으로
transform = transforms.Resize((256, 256))
img = Image.open("path/to/image.jpg")
out1 = transform(img)

# functional API로
img = Image.open("path/to/image.jpg")
out2 = F.resize(img, size=(256, 256))
```

### 예제 2: 랜덤 대신 직접 파라미터 제어

```python
from PIL import Image
import torchvision.transforms.functional as F

img = Image.open("path/to/image.jpg")

# 클래스: 무작위 회전
random_rot = transforms.RandomRotation(degrees=30)
out1 = random_rot(img)

# functional: 동일한 각도를 직접 지정 (재현 가능)
angle = 15
out2 = F.rotate(img, angle=angle)
```

## `v2`의 확장 기능

최신 torchvision.transforms.v2 모듈은 이미지뿐 아니라 비디오, 마스크, 바운딩 박스, 키포인트까지 일관성 있게 다룰 수 있습니다 (학습 성능을 크게 높이는 고급 증강 기법도 포함)

### [`CutMix`](https://docs.pytorch.org/vision/main/generated/torchvision.transforms.v2.CutMix.html)

일반적인 증강 기법은 한 이미지만 변형하기 때문에, 모델이 이미지 일부만 보고 학습하는 데 취약할 수 있습니다.

**CutMix는 두 이미지를 잘라 붙여 새로운 샘플을 생성**하여, 모델이 다양한 지역적 특성을 학습하고 과적합을 줄이는 데 도움을 줍니다.

- 입력: `(images, targets)` 튜플  
- 출력: `(mixed_images, mixed_targets)` 튜플 (라벨도 섞여 soft-label 형태가 됨)

```python
import torchvision.transforms.v2 as v2

cutmix = v2.CutMix(num_classes=10, alpha=1.0)
out_img, out_target = cutmix(images, targets)
```

<img src="https://i.ibb.co/zhTpmMSp/image.png" alt="image" border="0">

### [`MixUp`](https://docs.pytorch.org/vision/main/generated/torchvision.transforms.v2.MixUp.html)

일반적인 증강은 공간적 변형(회전, 잘라내기 등)이 많습니다. 하지만 이런 방식만으로는 클래스 간 경계를 부드럽게 학습하기 어렵습니다

MixUp은 두 이미지를 픽셀 단위로 선형 혼합하여, 모델이 더 유연하게 클래스 간 구분을 학습하도록 돕습니다

- 입력: (images, targets) (배치 단위)
- 출력: (mixed_images, mixed_targets) (soft-label 형태)

```python
mixup = v2.MixUp(num_classes=10, alpha=0.4)
out_img, out_target = mixup(images, targets)
```

<img src="https://i.ibb.co/SDLPdj7n/image.png" alt="image" border="0">

### [`RandAugment`](https://docs.pytorch.org/vision/main/generated/torchvision.transforms.v2.RandAugment.html)

데이터 증강에는 수많은 기법이 있지만, 어떤 조합이 최적인지는 실험적으로 찾아야 하는 경우가 많습니다

RandAugment는 미리 정의된 여러 변환 중 무작위로 N개를 선택해 동일한 강도로 적용함으로써, 하이퍼파라미터 탐색 부담을 줄이고 안정적인 성능을 제공합니다

- 입력: (images) (배치 혹은 단일 이미지)
- 출력: (augmented_images)

```python
randaug = v2.RandAugment(num_ops=2, magnitude=9)
augmented = randaug(images)
```

<img src="https://i.ibb.co/Y4F78P6f/image.png" alt="image" border="0">

## 핵심 요약

1. `torchvision.transforms`는 **전처리와 증강을 위한 표준 도구**입니다
2. `Transform` 클래스는 파이프라인 구성에 적합하고,`functional` API는 세밀한 제어와 어노테이션 처리에 적합합니다
3. v2는 CutMix, MixUp, RandAugment 같은 **고급 증강 기법**을 제공하여 모델 일반화 성능을 높일 수 있습니다

## 참고자료

- [torchvision.transforms - PyTorch docs
](https://docs.pytorch.org/vision/0.9/transforms.html)
- [torchvision을 사용한 이미지 변환 - 파이토치 핸즈온 랩 소개](https://hands-on.pytorch.kr/object-detection/torchvision-basic-transforms.html)

<br>

> 작성 - `2025.09.24`<br>
> 마지막 수정 - `2025.09.24`
