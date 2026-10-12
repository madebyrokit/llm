# uv Command Reference

Windows PowerShell 기준으로 자주 쓰는 `uv` 명령어를 정리했습니다. 이 프로젝트는 `pyproject.toml`을 사용하는 Python 프로젝트입니다.

## 설치 및 확인

```powershell
uv --version                 # uv 버전 확인
uv self update               # uv 업데이트
```

uv가 설치되지 않았다면 공식 설치 방법을 사용하세요. 설치 후 새 터미널을 열고 `uv --version`으로 확인합니다.

## 프로젝트 시작 및 실행

```powershell
uv init my-project           # 새 프로젝트 생성
uv sync                      # pyproject.toml 기준으로 의존성 설치/동기화
uv run python main.py        # 프로젝트 환경에서 Python 파일 실행
uv run <명령어>              # 프로젝트 환경에서 명령 실행
uv run                      # 프로젝트의 기본 실행 대상 실행(설정된 경우)
```

`uv run`과 `uv sync`는 필요한 경우 프로젝트 가상환경을 자동으로 준비합니다. 별도로 가상환경을 활성화하지 않고도 사용할 수 있습니다.

## 패키지 관리

```powershell
uv add requests              # 런타임 의존성 추가
uv add --dev pytest          # 개발용 의존성 추가
uv remove requests           # 의존성 제거
uv lock                      # uv.lock 잠금 파일 갱신
uv sync                      # 잠금 파일에 맞춰 환경 동기화
uv tree                      # 의존성 트리 보기
```

패키지를 일회성으로 실행할 때:

```powershell
uvx ruff check .             # 도구를 별도 환경에서 실행(uv tool run과 동일)
uv tool install ruff         # CLI 도구 설치
uv tool list                 # 설치된 CLI 도구 확인
uv tool uninstall ruff       # CLI 도구 제거
```

## Python 버전 및 가상환경

```powershell
uv python list               # 사용 가능한 Python 버전 보기
uv python install 3.12       # Python 3.12 설치
uv python pin 3.12           # 프로젝트 Python 버전 고정(.python-version)
uv venv                      # .venv 가상환경 생성
uv venv --python 3.12        # 지정한 Python으로 가상환경 생성
```

현재 프로젝트의 `.venv`를 직접 활성화하려면 PowerShell에서:

```powershell
.\.venv\Scripts\Activate.ps1
```

스크립트 실행 정책 때문에 막히면 현재 PowerShell 프로세스에만 임시 허용을 설정한 뒤 다시 실행합니다:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
.\.venv\Scripts\Activate.ps1
```

가상환경을 비활성화하려면:

```powershell
deactivate
```

## pip 호환 명령

기존 `pip` 대신 uv를 통해 설치/실행할 수 있습니다.

```powershell
uv pip install requests    # 현재 활성 환경에 설치
uv pip list                # 설치 패키지 목록
uv pip freeze              # 고정된 패키지 목록
uv pip uninstall requests  # 패키지 제거
```

프로젝트의 의존성을 관리할 때는 `uv pip install`로만 설치하기보다 `uv add`를 사용해 `pyproject.toml`과 `uv.lock`도 함께 갱신하는 것을 권장합니다.

## 이 프로젝트에서 가장 자주 쓸 명령

프로젝트 폴더(`C:\Users\wins\Downloads\temp`)에서:

```powershell
uv sync                     # 프로젝트 의존성 설치
uv run python --version     # uv 환경의 Python 버전 확인
uv run temp                 # pyproject.toml에 등록된 temp 스크립트 실행
uv add <패키지명>           # 패키지 추가
uv add --dev <패키지명>     # 개발 의존성 추가
```

## 참고

- `pyproject.toml`: 프로젝트 설정 및 의존성 목록
- `uv.lock`: 재현 가능한 의존성 버전 잠금 파일(uv가 관리)
- `.python-version`: 프로젝트에서 사용할 Python 버전
- `.venv`: 프로젝트 가상환경 폴더
