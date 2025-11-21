# 파이썬 가상환경 설정 (Python Virtual Environments)

파이썬 개발에서는 프로젝트마다 서로 다른 패키지 버전과 Python 버전을 사용하는 경우가 자주 발생합니다.
이를 적절히 관리하지 않으면 다음과 같은 문제가 생길 수 있습니다.

- 패키지 버전 충돌
- 시스템 Python 오염
- 프로젝트 간 의존성 혼재
- 재현 가능한 개발환경(reproducible environment) 구축 어려움

이를 해결하는 표준적인 방법이 바로 **가상환경**(Virtual Environment)입니다.
가상환경은 각 프로젝트가 독립적인 실행 환경과 패키지 집합을 가질 수 있도록 해주는 격리된 공간입니다.

## 가상환경이란?

가상환경은 다음 2가지 핵심을 제공합니다.

1. 파이썬 인터프리터의 독립 복제본
   - 시스템 Python과 분리된 실행 파일(executable)을 갖습니다.

2. 독립적인 `site-packages` 디렉토리
   - 프로젝트 전용 패키지를 설치하고 관리할 수 있습니다.

예를 들어, 한 프로젝트에서 `Django==4.2`를 사용하면서 다른 프로젝트에서는 `Flask==3.0`을 사용할 수 있습니다.

## 가상환경을 구축하는 방법

Python 3.3 이상에서는 표준 라이브러리 `venv` 모듈이 기본 제공됩니다.
따라서 **가장 일반적이고 권장되는 방식**은 venv 기반 가상환경 구축입니다.

### 가상환경 생성

```shell
# .venv라는 이름의 가상환경 디렉토리 생성
python -m venv .venv
```

### 가상환경 활성화 (activate)

#### macOS / Linux
```shell
source .venv/bin/activate
```

- 활성화되면 프롬프트에 환경 이름이 표시됩니다.

```shell
(.venv) $
```

### 가상환경 비활성화 (Deactivate)

```shell
deactivate
```

### 패키지 설치하기

가상환경이 활성화된 상태에서 설치된 패키지는
시스템 전체가 아닌 해당 가상환경 내부에만 적용됩니다.

```shell
pip install requests  # 특정 패키지 설치
pip list              # 패키지 목록 확인
```

## venv 디렉토리 구조

가상환경은 실제로 프로젝트 내부에 하나의 디렉토리로 존재합니다.
Unix 기반 시스템(macOS/Linux)을 기준으로 구조는 다음과 같습니다.

```
.venv/
├── bin/               # Python 실행 파일 & pip 명령어 존재
│   ├── python
│   ├── pip
│   ├── activate    # 가상환경 활성화 스크립트
│   └── ...       
│
├── lib/
│   └── python3.x/
│       └── site-packages/   # 설치된 패키지들이 저장되는 공간
│
├── pyvenv.cfg         # 가상환경 설정 파일 (기준 python 경로 등)
└── include/           # C 확장 모듈 헤더 파일 (optional)
```

| 요소                                          | 설명                              |
| ------------------------------------------- | ------------------------------- |
| `bin/python`                                | 이 가상환경이 사용하는 Python 실행 파일       |
| `bin/pip`                                   | 이 가상환경 전용 pip                   |
| `activate`, `activate.fish`, `Activate.ps1` | OS별 activate 스크립트               |
| `lib/python3.x/site-packages/`              | 패키지 설치 경로                       |
| `pyvenv.cfg`                                | venv 메타데이터 (base interpreter 등) |

## 패키지 버전 고정 및 재현 가능한 환경 만들기

### 의존성 고정 파일 생성

```shell
pip freeze > requirements.txt
```

### 동일 환경 재생성
```shell
pip install -r requirements.txt
```