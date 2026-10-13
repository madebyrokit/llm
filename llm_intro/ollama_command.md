# Ollama Command Reference

Windows PowerShell 기준으로 자주 쓰는 `ollama` 명령어를 정리했습니다. Ollama는 로컬 PC에서 LLM을 내려받아 실행할 수 있게 해 주는 도구입니다.

## 설치 및 확인

```powershell
ollama --version             # Ollama 버전 확인
ollama help                  # 전체 명령어 도움말
ollama <명령어> --help        # 특정 명령어 도움말
```

Ollama가 설치되지 않았다면 공식 사이트(https://ollama.com)에서 Windows 설치 파일을 받아 설치하세요. 설치 후 새 터미널을 열고 `ollama --version`으로 확인합니다.

## 서버 실행

```powershell
ollama serve                 # Ollama 서버 실행(기본 주소: http://localhost:11434)
ollama ps                    # 현재 메모리에 올라가 있는 모델 확인
ollama stop <모델명>          # 실행 중인 모델을 메모리에서 내리기
```

Windows에서는 설치 후 Ollama 앱이 백그라운드(트레이)에서 서버를 자동으로 실행합니다. 이미 실행 중일 때 `ollama serve`를 실행하면 포트 사용 중 오류가 날 수 있으니, 그 경우에는 별도로 실행하지 않아도 됩니다.

## 모델 내려받기 및 관리

```powershell
ollama pull llama3.2         # 모델 내려받기
ollama pull qwen2.5:7b       # 태그(크기/버전)를 지정해 내려받기
ollama list                  # 내려받은 모델 목록
ollama show llama3.2         # 모델 정보(파라미터, 컨텍스트 길이 등) 확인
ollama rm llama3.2           # 모델 삭제
ollama cp llama3.2 my-llama  # 모델을 다른 이름으로 복사
```

모델 이름 뒤의 `:7b`, `:latest` 같은 부분은 태그입니다. 태그를 생략하면 `latest`가 사용됩니다. 사용 가능한 모델은 https://ollama.com/library 에서 확인할 수 있습니다. 모델 크기가 클수록 더 많은 메모리(RAM/VRAM)가 필요합니다.

## 모델 실행(대화)

```powershell
ollama run llama3.2                       # 대화형으로 모델 실행(없으면 자동으로 내려받음)
ollama run llama3.2 "한 문장으로 자기소개해줘"  # 프롬프트 한 번만 실행하고 종료
ollama run llama3.2 --verbose             # 응답 후 토큰 수, 속도 등 통계 표시
```

대화형 모드 안에서 쓸 수 있는 명령:

```text
/?                 # 도움말
/show info         # 현재 모델 정보
/set system <내용>  # 시스템 프롬프트 설정
/clear             # 대화 기록 초기화
/bye               # 종료
```

여러 줄 입력은 `"""`로 시작하고 `"""`로 끝냅니다.

## Modelfile로 나만의 모델 만들기

`Modelfile` 예시:

```text
FROM llama3.2
PARAMETER temperature 0.7
SYSTEM "너는 친절한 한국어 Python 튜터야."
```

```powershell
ollama create my-tutor -f Modelfile   # Modelfile로 새 모델 생성
ollama run my-tutor                   # 생성한 모델 실행
ollama show my-tutor --modelfile      # 모델의 Modelfile 내용 확인
```

## REST API 호출

Ollama 서버는 `http://localhost:11434`에서 API를 제공합니다. PowerShell에서는 `curl` 대신 `Invoke-RestMethod`를 쓰는 것이 편합니다.

```powershell
# 텍스트 생성
Invoke-RestMethod -Uri http://localhost:11434/api/generate -Method Post -ContentType "application/json" -Body '{"model":"llama3.2","prompt":"하늘은 왜 파란가요?","stream":false}'

# 채팅 형식
Invoke-RestMethod -Uri http://localhost:11434/api/chat -Method Post -ContentType "application/json" -Body '{"model":"llama3.2","messages":[{"role":"user","content":"안녕?"}],"stream":false}'

# 설치된 모델 목록
Invoke-RestMethod -Uri http://localhost:11434/api/tags
```

OpenAI 호환 엔드포인트(`http://localhost:11434/v1`)도 제공하므로, OpenAI SDK에서 `base_url`만 바꿔 사용할 수도 있습니다.

## Python에서 사용하기(uv 프로젝트)

```powershell
uv add ollama                # Ollama Python 라이브러리 추가
```

```python
import ollama

response = ollama.chat(
    model="llama3.2",
    messages=[{"role": "user", "content": "안녕?"}],
)
print(response["message"]["content"])
```

```powershell
uv run python main.py        # 프로젝트 환경에서 실행
```

## 자주 쓰는 환경 변수

```powershell
$env:OLLAMA_HOST = "0.0.0.0:11434"      # 서버 주소/포트 변경(현재 터미널에만 적용)
$env:OLLAMA_MODELS = "D:\ollama\models" # 모델 저장 경로 변경
```

영구적으로 적용하려면 Windows "시스템 환경 변수 편집"에서 설정한 뒤 Ollama 앱을 재시작합니다. 기본 모델 저장 경로는 `C:\Users\<사용자>\.ollama\models`입니다.

## 일반적인 작업 흐름

```powershell
ollama --version
ollama pull llama3.2
ollama list
ollama run llama3.2
ollama ps
ollama stop llama3.2
```

## 참고

- `ollama list`: 내려받은 모델 목록
- `ollama ps`: 메모리에 올라간(실행 중인) 모델
- `ollama run <모델>`: 모델과 대화
- `Modelfile`: 기본 모델, 파라미터, 시스템 프롬프트를 정의하는 파일
- `http://localhost:11434`: Ollama API 기본 주소
