<a name="top"></a>

<div align="center">

# ☁️ GitHub 연결과 업로드

꒰ •͈ᴗ•͈ ꒱ 내 컴퓨터의 기록을 온라인으로 보내기

[🌱 Git 목차](./README.md) · [📔 TIL 홈](../README.md)

</div>

---

## 1. 먼저 구분하기

| 이름 | 의미 |
|---|---|
| 로컬 저장소 | 내 컴퓨터에 있는 Git 저장소 |
| 원격 저장소 | GitHub 등에 있는 저장소 |
| origin | 원격 저장소에 붙인 이름 |
| main | 브랜치 이름 |
| push | 로컬 커밋을 원격으로 전송 |
| fetch | 원격 기록을 가져오기 |
| pull | 원격 기록을 가져와 현재 브랜치에 통합 |

=> origin은 GitHub 아이디가 아님  
=> main은 폴더 이름이 아님

## 2. 기존 GitHub 저장소 내려받기

```bash
git clone https://github.com/ellllybean-lab/Elly_TIL.git
cd Elly_TIL
```

=> 저장소 파일과 Git 기록을 함께 내려받음  
=> origin 연결도 보통 자동으로 설정됨

확인:

```bash
git remote -v
git status
```

clone한 저장소에서 origin을 다시 add할 필요는 없음.

## 3. 내 파일을 새 저장소에 올리기

GitHub에서 빈 저장소를 먼저 생성.

기존 로컬 파일을 연결할 때는 README 등을 추가하지 않은 빈 저장소로 시작하면 초기 기록 충돌을 줄일 수 있음.

프로젝트 폴더에서:

```bash
git init
git branch -M main
git add .
git diff --cached --stat
git commit -m "첫 실습 파일 추가"
```

원격 연결 후 업로드:

```bash
git remote add origin 저장소주소
git push -u origin main
```

=> 저장소주소는 GitHub의 Code → HTTPS에서 복사

## 4. 연결 주소 확인과 변경

```bash
git remote -v
```

=> fetch와 push에 사용할 주소 확인

이미 origin이 있고 주소만 바꾸려면:

```bash
git remote set-url origin 저장소주소
```

연결 이름을 제거하려면:

```bash
git remote remove origin
```

=> 로컬 연결 설정만 제거  
=> GitHub 저장소나 로컬 파일을 삭제하는 명령은 아님

### 과제 저장소 주의

선생님이 확인하는 저장소의 연결을 바꾸지 않도록 현재 프로젝트와 주소 확인.

별도 저장소로 올리려면 원본을 보존하고 복사본에서 작업하는 방법도 있음.

## 5. push하기

첫 업로드:

```bash
git push -u origin main
```

- origin => 업로드할 원격 저장소
- main => 업로드할 로컬 브랜치
- -u => 성공하면 해당 원격 브랜치와 추적 관계 설정

설정 후:

```bash
git push
```

=> 기본 설정과 추적 관계에 따라 업로드

새 브랜치는:

```bash
git push -u origin 브랜치이름
```

## 6. fetch와 pull

### fetch

```bash
git fetch origin
```

=> 원격의 기록과 원격 추적 브랜치를 갱신  
=> 현재 브랜치나 작업 파일에 바로 합치지는 않음

비교 예시:

```bash
git log --oneline HEAD..origin/main
```

=> 현재 커밋에서 아직 도달할 수 없는 origin/main의 커밋 확인

### pull

```bash
git pull --ff-only
```

=> 원격 기록을 가져온 뒤 단순 전진이 가능할 때만 현재 브랜치에 반영

로컬과 원격 양쪽에 별도 커밋이 생기면 실패할 수 있음. 이 경우 기록을 확인하고 merge나 rebase 방식을 판단해야 함.

## 7. GitHub에서 README를 수정했다면

```text
GitHub에서 수정하고 커밋
           ↓
내 컴퓨터에서는 아직 예전 파일
           ↓
pull로 변경 반영
```

작업 파일을 커밋하거나 stash로 정리한 뒤:

```bash
git pull --ff-only
```

=> GitHub 웹 편집과 VS Code 편집을 함께 쓸 때 자주 필요한 작업

## 8. 매일 작업하는 순서

### 시작 전

```bash
git status
git pull --ff-only
```

### 수정·저장 후

```bash
git add .
git diff --cached --stat
git commit -m "Git 공부 노트 추가"
git push
```

=> 변경이 없다면 새 커밋도 만들 필요 없음

## 9. 올리지 않을 파일

저장소 맨 위에 `.gitignore` 생성:

```gitignore
.venv/
venv/
__pycache__/
*.py[cod]
.ipynb_checkpoints/
.env
.env.*
!.env.example
.DS_Store
Thumbs.db
```

=> 가상환경, 자동 생성 파일, 로컬 비밀값 제외  
=> 소스 코드와 uv.lock 같은 의존성 잠금 파일은 보통 함께 관리

이미 추적 중인 파일에는 ignore 규칙이 바로 적용되지 않음.

```bash
git rm --cached -- 파일명
```

=> 실제 파일은 유지하고 Git 추적에서 제외하도록 스테이징  
=> 커밋·push하면 원격 최신 버전에서도 해당 파일이 제거됨  
=> 이전 기록에 남은 내용까지 삭제되지는 않음

## 10. 마크다운 링크 경로

| 링크 | 의미 |
|---|---|
| `./README.md` | 현재 폴더의 README |
| `../README.md` | 한 단계 위 폴더의 README |
| `./01_basic.md` | 같은 폴더의 기본 명령어 노트 |
| `#top` | 현재 문서의 top 위치 |

Git 폴더 안의 파일에서:

```markdown
[Git 목차](./README.md)
[TIL 대문](../README.md)
```

=> 파일 위치가 달라지면 링크 경로도 함께 확인

---

[← 브랜치](./02_branch.md) · [🌱 목차](./README.md) · [문제 해결 →](./04_troubleshooting.md)