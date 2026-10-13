# Git Command Reference

Windows PowerShell 기준으로 자주 쓰는 `git` 명령어를 정리했습니다.

## 설정 및 확인

```powershell
git --version                                      # Git 버전 확인
git config --global user.name "Your Name"          # 커밋 작성자 이름 설정
git config --global user.email "you@example.com"   # 커밋 작성자 이메일 설정
git config --global --list                         # 전역 설정 확인
```

## 저장소 시작 및 상태 확인

```powershell
git init                    # 현재 폴더를 Git 저장소로 초기화
git clone <저장소 URL>       # 원격 저장소 복제
git status                  # 작업 트리와 스테이징 상태 확인
git status --short          # 상태를 간단히 표시
git diff                    # 스테이징 전 변경 내용 확인
git diff --staged           # 스테이징된 변경 내용 확인
```

## 변경 사항 저장

```powershell
git add <파일명>             # 특정 파일을 스테이징
git add .                   # 현재 폴더의 변경 사항을 스테이징
git commit -m "메시지"      # 스테이징된 변경 사항 커밋
git commit --amend           # 마지막 커밋 수정
```

`git add .`를 실행하기 전에 `git status`로 포함할 파일을 확인하세요. 마지막 커밋 수정은 아직 다른 사람과 공유하지 않은 커밋에 사용하는 것이 안전합니다.

## 로그 및 변경 취소

```powershell
git log --oneline --graph     # 커밋 이력을 그래프로 간단히 표시
git show <커밋>               # 특정 커밋의 변경 사항 확인
git restore <파일명>          # 스테이징 전 파일 변경을 취소
git restore --staged <파일명> # 파일을 스테이징에서 내리기
```

`git restore <파일명>`은 해당 파일의 미커밋 변경 내용을 버립니다. 실행하기 전에 필요한 변경이 남아 있는지 확인하세요.

## 브랜치 작업

```powershell
git branch                    # 로컬 브랜치 목록
git switch -c <브랜치명>      # 새 브랜치를 만들고 전환
git switch <브랜치명>         # 기존 브랜치로 전환
git branch -d <브랜치명>      # 병합된 로컬 브랜치 삭제
git merge <브랜치명>         # 지정한 브랜치를 현재 브랜치에 병합
```

## 원격 저장소와 동기화

```powershell
git remote -v                              # 원격 저장소 주소 확인
git fetch                                  # 원격 변경 사항 가져오기(현재 브랜치에 병합하지 않음)
git pull                                   # 원격 변경 사항을 가져와 현재 브랜치에 반영
git push                                   # 연결된 원격 브랜치로 푸시
git push -u origin <브랜치명>              # 원격 브랜치에 처음 푸시하고 추적 설정
git push                                   # 추적 설정 후 이후 변경 사항 푸시
```

## 일반적인 작업 흐름

```powershell
git status
git add <파일명>
git diff --staged
git commit -m "변경 내용 요약"
git push
```

## 참고

- `git status`: 현재 변경 및 브랜치 상태
- `git diff`: 아직 스테이징하지 않은 변경 내용
- `git log --oneline`: 커밋 이력
- `.gitignore`: Git에서 추적하지 않을 파일과 폴더 지정
