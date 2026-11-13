<a name="top"></a>

<div align="center">

# 🐣 Git 기본 명령어

꒰ᐢ. .ᐢ꒱ 변경한 내용을 확인하고 기록하기

[🌱 Git 목차](./README.md) · [📔 TIL 홈](../README.md)

</div>

---

## 1. Git과 GitHub 구분하기

| 이름 | 의미 |
|---|---|
| Git | 파일의 변경 이력을 관리하는 프로그램 |
| GitHub | Git 저장소를 온라인에 보관하고 공유하는 서비스 |
| 저장소 · repository | 프로젝트 파일과 Git 기록을 관리하는 공간 |
| 커밋 · commit | 선택한 변경 내용을 기록한 단위 |
| 브랜치 · branch | 작업 흐름을 나누는 가지 |

=> Git에 커밋했다고 GitHub에도 자동으로 올라가지는 않음

## 2. Git의 세 공간

| 공간 | 하는 일 |
|---|---|
| 작업 폴더 | 내가 파일을 열고 수정하는 곳 |
| 스테이징 영역 | 다음 커밋에 넣을 변경을 선택하는 곳 |
| 로컬 저장소 | 커밋으로 남긴 이력이 저장되는 곳 |

```text
작업 폴더 → 스테이징 영역 → 로컬 저장소
          add           commit
```

=> add는 파일을 옮기는 것이 아니라 현재 변경 내용을 선택하는 작업

## 3. 현재 위치 확인

Bash에서 사용하는 명령어이며 Git 명령어는 아님.

| 명령어 | 의미 |
|---|---|
| `pwd` | 현재 폴더 위치 확인 |
| `ls` | 파일과 폴더 목록 확인 |
| `ls -a` | 숨김 파일도 포함해서 확인 |
| `cd 폴더명` | 해당 폴더로 이동 |
| `cd ..` | 상위 폴더로 이동 |

```bash
cd ~/Elly_TIL
```

=> 내 사용자 홈 폴더 아래의 Elly_TIL로 이동

폴더 이름에 공백이 있다면:

```bash
cd "my project"
```

## 4. Git 설치와 작성자 확인

```bash
git --version
```

=> Git 버전 확인

```bash
git config --get user.name
git config --get user.email
```

=> 현재 적용되는 커밋 작성자 확인

처음 설정한다면:

```bash
git config --global user.name "내 이름"
git config --global user.email "내 이메일"
```

- `--global` => 현재 사용자 계정의 기본 설정
- 이름과 이메일은 커밋에 남는 작성자 정보
- GitHub 로그인 설정과는 별개
- 이메일 비공개를 원하면 GitHub의 비공개 커밋 이메일 사용

해당 저장소에만 다른 정보를 쓰려면 `--global`을 빼고 설정.

## 5. Git 관리 시작

```bash
git init
```

=> 현재 폴더에 Git 저장소를 초기화

`.git`에는 변경 기록과 설정 등이 들어 있음.

- 일반 프로젝트 파일과 역할이 다름
- 함부로 삭제하거나 다른 프로젝트에 복사하지 않기
- clone으로 받은 저장소에서는 보통 다시 init할 필요 없음

현재 브랜치 이름을 main으로 정하려면:

```bash
git branch -M main
```

=> 현재 브랜치를 main으로 강제 이름 변경하는 옵션  
=> 다른 main 브랜치가 있는 저장소에서는 먼저 상태 확인

## 6. 파일 상태 확인

```bash
git status
```

| 표시 | 의미 |
|---|---|
| `On branch main` | 현재 main 브랜치에서 작업 중 |
| `modified` | 수정된 파일 |
| `deleted` | 삭제된 파일 |
| `Untracked files` | 아직 추적하지 않는 새 파일 |
| `Changes to be committed` | 커밋하도록 선택한 변경 |
| `Changes not staged for commit` | 아직 선택하지 않은 변경 |
| `working tree clean` | 기록되지 않은 변경이 없는 상태 |

짧게 확인하려면:

```bash
git status -sb
```

=> 글자 색보다 메시지의 의미를 읽기

## 7. 실제 수정 내용 확인

### add하기 전 수정

```bash
git diff
```

=> 작업 폴더와 스테이징 영역의 차이

### add한 수정

```bash
git diff --cached
```

=> 다음 커밋에 들어갈 변경

### 변경량만 간단히 확인

```bash
git diff --cached --stat
```

=> 파일 이름과 추가·삭제된 줄 수 요약

### 마지막 커밋과 현재 상태 비교

```bash
git diff HEAD
```

=> 추적 중인 파일의 선택된 변경과 미선택 변경을 함께 비교

새로 만든 미추적 파일의 내용은 일반적인 diff에 나오지 않으므로 status도 함께 확인하기.

## 8. 커밋할 변경 선택

특정 파일:

```bash
git add README.md
```

특정 폴더:

```bash
git add Git/
```

현재 폴더와 하위 폴더:

```bash
git add .
```

저장소 전체:

```bash
git add -A
```

=> 새 파일뿐 아니라 수정과 삭제도 포함

수정한 내용을 부분별로 고르고 싶다면:

```bash
git add -p
```

=> 변경 묶음별로 확인하며 선택하는 방식

### 주의할 점

add한 뒤 파일을 다시 수정하면 추가 수정분은 자동으로 선택되지 않음.

```text
파일 수정 → add → 다시 수정
                 ↓
         추가 수정도 넣으려면 다시 add
```

## 9. 커밋 만들기

```bash
git commit -m "Git 기본 명령어 정리"
```

=> 스테이징한 변경을 하나의 커밋으로 기록

커밋 메시지는 무엇을 변경했는지 작성.

```text
README 목차 추가
쇼핑몰 재고 계산 오류 수정
Docker 실습 명령어 정리
Git 오류 해결 과정 추가
```

`git commit -am "설명"`도 있지만, 새 미추적 파일은 포함하지 않음. 처음에는 add와 commit을 나눠서 사용하기.

## 10. 커밋 기록 보기

| 명령어 | 의미 |
|---|---|
| `git log --oneline` | 기록을 한 줄씩 보기 |
| `git log -5 --oneline` | 최근 커밋 5개 보기 |
| `git log --oneline --all --graph` | 전체 브랜치 관계 보기 |
| `git log -- README.md` | README의 커밋 기록 |
| `git show 커밋ID` | 특정 커밋 상세 보기 |
| `git ls-files` | Git이 추적하는 파일 목록 보기 |

```text
a1b2c3d README 목차 추가
```

=> 앞부분이 커밋ID, 뒷부분이 커밋 메시지

기록 화면에서 `END`가 보이면 영문 `q`로 나가기.

## 🍓 직접 해보기

README를 수정하고 저장한 뒤:

```bash
git status
git diff
git add README.md
git diff --cached
git commit -m "README 소개 정리"
git log -3 --oneline
```

=> GitHub 업로드는 push 단계가 별도로 필요

---
[🌱 목차](./README.md) · [☁️ 맨 위로](#top)