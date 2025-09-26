## GitHub Copilot으로 레거시 프로젝트 업그레이드하기

레거시(오래된) Python 애플리케이션을 최신 버전의 Python으로 업그레이드하는 과정을 GitHub Copilot의 도움을 받아 진행해봅시.

> [!NOTE]
> 이 저장소는 **GitHub Copilot**의 다양한 기능(예: **Copilot Chat**, **인라인 채팅**)을 소개하기 위한 것입니다. 아래 단계별 가이드에는 필요한 작업의 일반적인 설명이 포함되어 있으며, Copilot Chat 또는 인라인 채팅을 통해 필요한 명령을 생성할 수 있습니다.
>
> 각 단계(해당되는 경우)에는 Copilot 제안의 정확성을 검증할 수 있는 `Cheatsheet`도 포함되어 있습니다.
>
> 💡 다양한 프롬프트를 시도해보고 결과의 정확도가 어떻게 달라지는지 확인해보세요. 예를 들어 인라인 채팅을 사용할 때, 추가 프롬프트를 통해 응답을 세부적으로 조정할 수 있습니다.

## 워크숍 기능

이 워크숍에서는 Python 2.5를 사용했던 레거시 Python 애플리케이션을 다룹니다. 주요 특징은 다음과 같습니다:

1. 모든 의존성이 미리 설치되어 있지만, 레거시 애플리케이션에서는 오래된 Python 버전을 사용합니다.
2. 애플리케이션은 Sqlite3와 오래된 Python 문법을 사용하며, 이를 최신 문법으로 업데이트할 수 있습니다.
3. 레거시 애플리케이션에는 기능 테스트에 사용할 수 있는 문서가 포함되어 있습니다.
4. 유닛 테스트와 기능 테스트가 모두 레거시 코드에 포함되어 있습니다.

### 1. 에이전트로 프로젝트 탐색하기

`@workspace` 에이전트를 사용하여 프로젝트의 동작 방식을 설명받으세요.

- GitHub Copilot Chat을 열고 프롬프트 앞에 `@workspace`를 붙여 질문하세요.
- 예시: "이 프로젝트는 어떻게 의존성을 설치하나요?"

### 2. 레거시 및 포팅 문제 파악하기

Python 애플리케이션의 테스트를 실행해보세요. 사전 설치된 `pytest`를 사용하여 결과를 확인합니다.

- 예시: "@workspace 이 코드를 최신 Python으로 포팅할 때 발생할 수 있는 문제는 무엇인가요?"
- 가상 환경을 만들어 의존성을 설치해보세요.

> [!NOTE]
> 왜 문제가 발생할 수 있나요? 레거시 Python은 더 이상 지원되지 않는 `distribute`를 사용하므로, 의존성 설치가 실패할 수 있습니다. Copilot에게 다른 설치 방법을 제안받으세요.

### 3. 모든 코드를 upgraded 디렉터리로 복사하기

코드를 포팅하려면 모든 코드를 `upgraded` 디렉터리로 복사해야 합니다. 이곳에서 작업을 진행합니다.

- 기능 테스트를 사용해 집중할 기능 범위를 결정하세요.

### 4. 프로젝트 설치하기

가상 환경을 만들고 프로젝트를 설치하세요. `python setup.py develop` 명령을 실행하고 오류를 분석하세요.

- Copilot Chat에 오류를 붙여넣고 원인을 질문하세요.
- `setup.py` 파일을 컨텍스트로 추가하세요.
- Copilot의 제안대로 오류를 수정하고 설치를 완료하세요.

### 5. 테스트 실행하기

`pytest` 프레임워크와 명령줄 도구가 설치되어 있습니다. 유닛 테스트를 실행하고 오류를 확인하세요.

- 오류 출력을 Copilot에게 질문하세요. `#terminalSelection`을 사용해 선택한 출력을 붙여넣으세요.
- Copilot의 답변을 검토하고 추가 질문을 하세요.
- 코드를 바로 수정하지 마세요.

> [!NOTE]
> 오류가 즉시 이해되지 않을 수 있습니다. 레거시 Python 버전 사용을 염두에 두고 추가 질문을 하세요.

### 6. try/except 오류 수정하기

기능 테스트를 사용해 집중할 기능 범위를 결정하세요. 이번 단계에서는 try/except 오류를 수정합니다.

- Copilot에게 실제 문제를 질문하세요. 오래된 Python 버전을 언급하세요.
- 모든 try/except 오류를 수정하고 테스트가 통과할 때까지 반복하세요.

> [!NOTE]
> 예외 외에도 다른 오류가 있을 수 있습니다. 모든 try/except를 우선적으로 수정하세요.

### 7. ConfigParser 문제 해결하기

다음 단계는 `ConfigParser`가 사용 불가한 문제를 해결하는 것입니다. Copilot에게 해결 전략을 질문하세요.

- 예시: "이 프로젝트는 Python 2.5로 작성되었는데 Python 3.9에서 다음과 같은 오류가 발생합니다:" (테스트 출력 포함)
- 간단한 수정일 수 있습니다. 선택한 방법을 바로 적용하고 검증하세요.
- 테스트를 실행해 변경 사항이 올바른지 확인하세요.

[!NOTE]
> 모든 테스트가 통과해야 합니다. 통과하지 않으면 Copilot에게 추가 질문을 하세요.

### 8. 테스트를 Pytest로 포팅하기

Pytest는 가장 강력한 Python 테스트 프레임워크입니다. 레거시 코드는 `unittest`를 사용하므로, 모든 테스트를 `pytest`로 포팅하세요.

- 테스트 파일에서 `@workspace` 에이전트로 포팅 방법을 질문하세요.
- 클래스 대신 함수형 테스트를 작성하세요.
- 하나의 테스트를 작성한 후 Copilot에게 다음 테스트 생성을 요청하세요.

> [!NOTE]
> Pytest는 `unittest` 테스트도 실행할 수 있지만, 더 강력하고 유연하므로 포팅하는 것이 좋습니다.

### 9. 최신 패키징 사용하기

레거시 코드는 더 이상 지원되지 않는 `distribute`와 `setuptools`를 사용합니다. 최신 표준인 `pyproject.toml`을 사용해 패키징하세요.

- Copilot에게 `pyproject.toml` 파일 생성 방법을 질문하세요. `setup.py` 파일을 컨텍스트로 추가하세요.
- 새 `pyproject.toml` 파일을 만들고 Copilot의 제안대로 내용을 채우세요.
- `pip install .` 명령으로 프로젝트를 설치하세요.

> [!NOTE]
> Python 패키징은 여전히 복잡한 주제입니다. Copilot의 제안이 항상 정확하지 않을 수 있으니, 내용을 검증하고 설치를 테스트하세요.

### 10. GitHub Actions로 자동화하기

레거시 프로젝트에는 자동화가 없습니다. GitHub Actions를 사용해 테스트 및 검증을 자동화하세요.

- `@workspace` 에이전트로 GitHub Actions 워크플로우 생성 방법을 질문하세요.
- 필요한 파일과 디렉터리를 만들어 워크플로우를 작성하세요.
- 변경 사항을 푸시해 GitHub Actions가 동작하는지 확인하세요.

> [!NOTE]
> 자동화 구축은 시간이 오래 걸릴 수 있습니다. Copilot의 도움을 받아 오류와 문제를 해결하며 진행하세요.

## :books: 참고 자료

필수는 아니지만, 워크숍에서 다루는 일부 기능은 아래 Microsoft Learning 모듈에서 확인할 수 있습니다:

- [GitHub Codespaces로 코드 작성하기](https://learn.microsoft.com/training/modules/code-with-github-codespaces/)
- [고급 GitHub Copilot 기능 사용하기](https://learn.microsoft.com/training/modules/advanced-github-copilot/)

## 기여하기

이 프로젝트는 기여와 제안을 환영합니다. 대부분의 기여에는 Contributor License Agreement(CLA)에 동의해야 합니다. 자세한 내용은 https://cla.opensource.microsoft.com 을 참고하세요.

풀 리퀘스트를 제출하면 CLA 봇이 자동으로 CLA 필요 여부를 판단하고 안내합니다. 안내에 따라 진행하면 됩니다.

이 프로젝트는 [Microsoft 오픈 소스 행동 강령](https://opensource.microsoft.com/codeofconduct/)을 채택했습니다. 자세한 내용은 [행동 강령 FAQ](https://opensource.microsoft.com/codeofconduct/faq/)를 참고하거나 [opencode@microsoft.com](mailto:opencode@microsoft.com)으로 문의하세요.
