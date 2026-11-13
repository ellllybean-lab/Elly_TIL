<a name="top"></a>

<div align="center">

# 🐙 GitHub CLI · gh

꒰ ⸝⸝•ᴗ•⸝⸝ ꒱ 터미널에서 GitHub 저장소 만들기

[🌱 Git 목차](./README.md) · [📔 TIL 홈](../README.md)

</div>

---

## 🧁 1. git과 gh의 차이

| 구분 | 역할 | 예시 |
|---|---|---|
| `git` | 코드 변경 기록 관리 | add, commit, push |
| `gh` | GitHub 기능을 터미널에서 사용 | 로그인, 저장소 생성, PR |

=> gh를 사용해도 파일 수정 후 add와 commit은 필요함

## 🐣 2. 설치 확인

```bash
gh --version
```

=> 버전이 나오면 설치된 상태

명령을 찾을 수 없다면 Windows에서 설치:

```powershell
winget install --id GitHub.cli --exact
```

설치 후 VS Code를 다시 열고 확인하기.

## 🔑 3. GitHub 로그인

```bash
gh auth login
```

화면 안내에 따라 선택:

- GitHub.com
- HTTPS
- 브라우저를 이용한 로그인

=> 브라우저에서 인증을 마치면 터미널에서도 GitHub 기능을 사용할 수 있음

로그인 상태 확인:

```bash
gh auth status
```

=> 어떤 계정으로 로그인했는지 확인  
=> Git의 user.name 설정과 GitHub 로그인은 별개

## 📦 4. 질문에 답하면서 저장소 만들기

```bash
gh repo create
```

=> 화면에서 생성 방식, 이름, 공개 여부 등을 선택

처음에는 이 방식으로 질문을 읽으며 진행하기.

## 🌱 5. 빈 원격 저장소 만들기

### 공개 저장소

```bash
gh repo create 새저장소이름 --public
```

### 비공개 저장소

```bash
gh repo create 새저장소이름 --private
```

=> 둘 중 원하는 방식 하나만 실행  
=> 저장소 생성만으로 내 파일이 모두 업로드되지는 않음

소유자를 생략하면 로그인한 사용자 계정에 생성됨.

## 🧸 6. 기존 로컬 프로젝트 올리기

> 아직 GitHub에 만들지 않은 새 저장소에 올리는 예시

먼저 올릴 프로젝트 폴더로 이동.

```bash
git status
git remote -v
```

=> 현재 프로젝트와 기존 연결 확인  
=> 과제 저장소라면 연결을 함부로 바꾸지 않기

Git 관리 전인 프로젝트라면 초기화:

```bash
git init
git branch -M main
```

파일 확인 후 커밋:

```bash
git add .
git diff --cached --stat
git commit -m "첫 프로젝트 업로드"
```

기존 origin이 없는 상태에서 새 저장소 생성·연결·업로드:

```bash
gh repo create 새저장소이름 --public --source=. --remote=origin --push
```

| 옵션 | 의미 |
|---|---|
| `--public` | 공개 저장소 생성 |
| `--source=.` | 현재 로컬 저장소 사용 |
| `--remote=origin` | 새 원격 연결 이름 지정 |
| `--push` | 로컬 커밋을 새 저장소에 업로드 |

=> 비공개로 만들려면 public 대신 private 사용  
=> 새저장소이름은 실제 원하는 이름으로 변경  
=> 같은 이름의 저장소가 이미 있으면 생성할 수 없음

## ☁️ 7. 이미 GitHub에 저장소가 있다면

gh repo create를 다시 실행하지 않기.

### origin이 없을 때

```bash
git remote add origin 저장소주소
```

### origin 주소를 변경하려는 경우

```bash
git remote set-url origin 저장소주소
```

=> 현재 상태에 맞는 명령 하나만 사용

커밋이 준비됐다면:

```bash
git push -u origin main
```

=> 기존 저장소에 README 등 별도 기록이 있다면 먼저 기록 통합이 필요할 수 있음

## 📥 8. 저장소 내려받기

```bash
gh repo clone ellllybean-lab/Elly_TIL
cd Elly_TIL
code .
```

=> 저장소 파일과 Git 기록을 새 폴더로 내려받기  
=> 같은 폴더를 이미 내려받았다면 중복 clone하지 않기

git으로도 가능:

```bash
git clone https://github.com/ellllybean-lab/Elly_TIL.git
```

=> 둘 중 한 방식 사용

## 🏡 9. 저장소 확인하기

현재 로컬 저장소에 연결된 GitHub 정보 확인:

```bash
gh repo view
```

브라우저로 열기:

```bash
gh repo view --web
```

특정 저장소 열기:

```bash
gh repo view ellllybean-lab/Elly_TIL --web
```

=> 현재 연결과 상관없이 지정한 저장소를 열 수 있음

## 🍓 10. 생성 이후의 평소 작업

gh로 저장소를 만든 뒤에도 변경 기록은 git으로 관리.

```bash
git status
git add .
git diff --cached --stat
git commit -m "오늘 공부한 내용 추가"
git push
```

=> repo create는 매일 실행하는 명령이 아님  
=> 이미 있는 저장소에는 commit과 push로 변경 반영

## 🩹 11. 막혔을 때

| 상황 | 확인할 것 |
|---|---|
| gh 명령을 찾을 수 없음 | 설치 여부 확인 후 터미널·VS Code 다시 열기 |
| 로그인이 필요하다고 나옴 | gh auth status 확인 |
| 같은 저장소 이름이 이미 있음 | 기존 저장소를 연결하거나 새 이름 사용 |
| origin 연결에서 오류 | git remote -v로 기존 연결 확인 |
| 파일이 GitHub에 안 보임 | 커밋과 push를 했는지 확인 |
| 다른 계정으로 작업됨 | gh 로그인 계정과 저장소 소유자 확인 |

## 💌 자주 쓰는 명령 모음

| 명령어 | 역할 |
|---|---|
| `gh --version` | 설치 확인 |
| `gh auth login` | 로그인 |
| `gh auth status` | 로그인 상태 확인 |
| `gh repo create` | 질문에 답하며 저장소 생성 |
| `gh repo clone 소유자/저장소` | 저장소 내려받기 |
| `gh repo view` | 저장소 정보 확인 |
| `gh repo view --web` | 브라우저에서 저장소 열기 |
| `gh repo create --help` | 저장소 생성 도움말 |

## 🔗 공식 문서

- [GitHub CLI](https://cli.github.com/)
- [저장소 생성](https://cli.github.com/manual/gh_repo_create)
- [로그인](https://cli.github.com/manual/gh_auth_login)

---

<div align="center">

### ꒰ঌ git은 기록 · gh는 GitHub 작업 ໒꒱

[🌱 Git 목차](./README.md) · [☁️ 맨 위로](#top)

</div>