# 버전 관리 시스템 (Versioning)

소프트웨어 개발에서 버전 관리는 프로젝트의 변경 사항을 추적하고, 협업을 용이하게 하며, 릴리스를 관리하는 데 필수적인 프로세스입니다. 다양한 버전 관리 전략이 있으며, 각기 다른 목적과 장단점을 가집니다.

이 문서에서는 대표적인 버전 관리 방식인 Semantic Versioning, Hash Versioning, Calendar Versioning에 대해 살펴보고 마지막으로 Python에서의 버전 및 환경 관리 방식도 함께 알아봅니다.

---

## 1. Semantic Versioning (SemVer)

Semantic Versioning(의미론적 버전 관리)은 가장 널리 사용되는 버전 관리 방식으로, 버전 번호에 명확한 의미를 부여하여 변경 사항의 성격을 쉽게 파악할 수 있도록 합니다. 버전은 **Major.Minor.Patch** 세 부분으로 구성됩니다.

**구조: `Major.Minor.Patch` (예: `2.1.4`)**

- **Major**: 하위 호환성이 보장되지 않는 큰 변경(Breaking Change)이 있을 때 올립니다. API가 변경되거나, 기존 기능이 삭제되는 등 이전 버전과 호환되지 않는 업데이트가 포함됩니다.
  
- **Minor**: 하위 호환성이 보장되면서 새로운 기능이 추가될 때 올립니다. 기존 API를 사용하는 코드는 수정 없이 새로운 버전을 사용할 수 있습니다.

- **Patch**: 하위 호환성이 보장되는 버그 수정 및 작은 변경 사항이 있을 때 올립니다. 새로운 기능 추가 없이 기존 기능의 문제를 해결하는 경우에 해당합니다.

### 장점
- 버전 번호만으로 변경의 영향 범위를 예측할 수 있습니다.
- 의존성 관리(Dependency Management)가 용이해집니다. (예: `^2.1.4`는 `2.x` 버전 내에서 최신 버전을 사용하도록 허용)
- 개발자와 사용자 간의 명확한 소통 기준이 됩니다.

### 단점
- 0.x.x 버전(초기 개발 단계)에서는 Major 버전 변경이 잦을 수 있어 의미가 퇴색될 수 있습니다.
- 어떤 변경이 "Breaking Change"인지에 대한 판단이 주관적일 수 있습니다.

---

## 2. Hash Versioning

Hash Versioning(해시 버전 관리)은 주로 파일 기반의 애셋(Asset)이나 빌드 결과물에 사용되는 방식으로, 파일의 내용 자체를 해시(Hash)하여 버전으로 사용합니다. 주로 웹 개발에서 CSS, JavaScript 파일의 캐시 버스팅(Cache Busting)을 위해 사용됩니다.

**구조: `[filename].[hash].[extension]` (예: `main.d2a8e7f.css`)**

- **Hash**: 파일의 내용이 변경될 때마다 새로운 해시값이 생성됩니다. MD5, SHA-1, SHA-256 등의 해시 알고리즘이 사용됩니다.

파일 내용이 단 1비트라도 변경되면 해시값이 완전히 달라지므로, 사용자는 항상 최신 버전의 파일을 다운로드하게 됩니다. 반면, 내용이 동일한 파일은 해시값도 동일하여 브라우저 캐시를 효율적으로 활용할 수 있습니다.

### 장점
- 파일 내용의 무결성을 보장합니다.
- 캐시 관리가 매우 효율적입니다. 변경되지 않은 파일은 캐시를 사용하고, 변경된 파일만 새로 다운로드합니다.
- 버전 충돌의 여지가 없습니다.

### 단점
- 버전 번호가 길고 사람이 기억하기 어렵습니다.
- 전체 릴리스의 버전을 표현하기보다는 개별 파일의 버전을 관리하는 데 적합합니다.

---

## 3. Calendar Versioning (CalVer)

Calendar Versioning(달력 버전 관리)은 버전 번호에 날짜 정보를 포함하여 릴리스 시점을 명확하게 나타내는 방식입니다. 주로 Ubuntu, Twisted 등과 같이 정기적인 릴리스 주기를 가진 프로젝트에서 선호됩니다.

**구조: `YYYY.MM.DD`, `YY.MM.Patch`, `YYYY.Minor` 등 다양한 조합이 가능합니다.**

- **YYYY/YY**: 릴리스 연도
- **MM**: 릴리스 월
- **DD**: 릴리스 일
- **Minor/Patch**: 같은 날짜에 여러 릴리스가 있을 경우 사용

예를 들어, Ubuntu는 `YY.MM` 형식(예: `22.04`)을 사용하여 매년 4월과 10월에 릴리스되는 버전을 표기합니다.

### 장점
- 버전 번호만으로 언제 릴리스되었는지 쉽게 알 수 있습니다.
- 시간 기반의 릴리스 정책을 가진 프로젝트에 매우 적합합니다.
- 사용자에게 지원 기간이나 업데이트 주기를 예측하는 데 도움을 줍니다.

### 단점
- 버전 번호가 변경 사항의 성격(Breaking Change 등)을 직접적으로 알려주지 않습니다.
- 하루에 여러 번 릴리스가 필요한 경우 버전 관리가 복잡해질 수 있습니다.

---

## 4. Python 환경 및 버전 관리

Python에서는 **언어 버전**(Python 3.9, 3.10 등)과 **패키지 버전**(requests 2.31.0 등)이 모두 중요합니다.
이를 관리하기 위해 다양한 도구와 표준이 존재하며, 아래에서는 Python 환경을 구성하는 방법부터 패키지 버전 관리까지 순서대로 살펴봅니다.

### Python 버전 및 환경 관리 도구

프로젝트마다 다른 Python 버전을 사용할 수 있기 때문에, Python 자체를 효율적으로 관리하는 도구가 필요합니다. 대표적으로 pyenv, virtualenv, venv, conda 등이 있습니다.

이 중 `pyenv`는 여러 Python 버전을 설치하고 프로젝트별로 구분해 사용할 수 있게 해주는 가장 널리 쓰이는 도구입니다.

#### pyenv 설치 (macOS)
```shell
brew install pyenv
```

#### shell 설정 파일(~/.zshrc 등)에 다음을 추가
```shell
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
```

#### Python 버전 설치
```shell
pyenv install 3.11.6
```

---

### Python 패키지 및 프로젝트 버전 관리

- **`setup.py`**: `setuptools` 라이브러리를 사용해 패키지 메타데이터(이름, 버전, 의존성 등)를 정의하는 파일입니다. 전통적인 방식이며, 이제는 `pyproject.toml`로 대체되는 추세라고 합니다.
    ```python
    from setuptools import setup, find_packages

    setup(
        name='my-awesome-package',
        version='0.1.0',  # 여기서 패키지 버전 지정
        packages=find_packages(),
        install_requires=[
            'requests>=2.20.0',
        ],
        # 기타 메타데이터...
    )
    ```

- **`pyproject.toml`**: 파이썬 빌드 시스템 및 프로젝트 메타데이터를 정의하는 최신 표준 파일입니다 (PEP 518, PEP 621). `[project]` 섹션에서 패키지 정보를 설정합니다.
    ```toml
    [project]
    name = "my-awesome-package"
    version = "0.1.0" # 여기서 패키지 버전 지정
    description = "My awesome Python package"
    requires-python = ">=3.7"
    dependencies = [
        "requests>=2.20.0",
    ]

    [build-system]
    requires = ["setuptools>=61.0"]
    build-backend = "setuptools.build_meta"
    ```

- **`setuptools`**: 파이썬 패키지를 빌드하고 배포하는 핵심 라이브러리입니다. `setup.py` 또는 `pyproject.toml`에 정의된 `version` 정보를 활용하여 패키징을 수행합니다.

- **`pip`**: 파이썬 패키지 설치 및 관리의 표준 도구입니다. `pip install my-package==1.2.3`과 같이 특정 버전의 패키지를 정확히 지정하여 설치할 수 있습니다.

---

### 파이썬 프로젝트의 버전 관리 전략

대부분의 파이썬 프로젝트는 **Semantic Versioning (SemVer)** 규칙을 따릅니다:

- **0.y.z**: 개발 초기, API 변경 빈번
- **1.0.0** 이후: 안정 단계, 의미론적 버전(SemVer) 준수
- **Major** 변경 → Breaking Change
- **Minor** → 기능 추가
- **Patch** → 버그 수정

### 버전 자동화 도구

`setuptools_scm`, `bump2version` 등의 도구를 활용하면 Git 태그나 커밋 메시지를 기반으로 자동으로 버전을 관리하거나 쉽게 업데이트할 수 있습니다. 이는 수동 작업의 오류를 줄이고 CI/CD 파이프라인과 연동하여 릴리스 과정을 효율화하는 데 도움을 줍니다.
