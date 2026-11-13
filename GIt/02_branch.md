<a name="top"></a>

<div align="center">

# 🌿 브랜치와 병합

(ง •̀ᴗ•́)ง 작업을 나누고 다시 합치기

[🌱 Git 목차](./README.md) · [📔 TIL 홈](../README.md)

</div>

---

## 1. 브랜치란?

기존 작업 흐름에서 갈라져 별도 작업을 진행하는 방법.

```text
main        ●──●────────●
                ＼      ／
docs/readme      ●──●
```

=> main과 나눠서 수정하고, 완료한 변경을 다시 합칠 수 있음  
=> 폴더를 통째로 하나 더 만드는 기능은 아님

## 2. 브랜치 확인

```bash
git branch
```

=> 로컬 브랜치 목록. 앞에 별표가 붙은 것이 현재 브랜치

```bash
git branch -a
```

=> 로컬과 원격 추적 브랜치 목록

```bash
git branch -vv
```

=> 로컬 브랜치의 최근 커밋과 원격 추적 관계 확인

## 3. 생성과 이동

### 만들기만 하기

```bash
git branch docs/readme
```

=> 브랜치를 생성하지만 현재 브랜치는 그대로

### 기존 브랜치로 이동

```bash
git switch docs/readme
```

### 만들면서 이동

```bash
git switch -c docs/readme
```

=> 현재 커밋 위치에서 새 브랜치를 만들고 이동

기존 자료에서 볼 수 있는 명령:

```bash
git checkout docs/readme
git checkout -b docs/readme
```

=> 각각 이동 / 생성하면서 이동  
=> 브랜치 이동은 switch로 구분해서 익히기

## 4. 브랜치 이름 예시

| 이름 | 용도 예시 |
|---|---|
| `docs/readme` | README 설명 수정 |
| `feat/cart` | 장바구니 기능 추가 |
| `fix/stock` | 재고 관련 오류 수정 |

=> 이름은 팀이나 프로젝트 규칙에 맞춰 정하기

## 5. 브랜치에서 수정하고 기록

```bash
git switch -c docs/readme
```

README 수정 후 Ctrl+S로 저장:

```bash
git add README.md
git commit -m "README 목차 정리"
```

=> 이 커밋은 현재 작업 중인 docs/readme의 기록

브랜치 이동 전에는 status를 확인하고 미커밋 변경을 정리하기. 변경 내용이 이동을 막거나 다른 브랜치로 함께 따라갈 수 있음.

## 6. main으로 병합

```bash
git switch main
git merge docs/readme
```

=> docs/readme의 변경을 현재 브랜치인 main에 합치기

병합 후 확인:

```bash
git status
git log --oneline --all --graph
```

### Fast-forward

main에 별도 변경이 없다면 main의 위치만 앞으로 이동하며 병합될 수 있음.

### Merge commit

양쪽에 서로 다른 커밋이 있다면 병합 커밋이 만들어질 수 있음.

=> 모든 merge가 새 병합 커밋을 만드는 것은 아님

## 7. 충돌이 생겼다면

같은 부분을 서로 다르게 수정한 경우 자동 병합이 어려울 수 있음.

```text
<<<<<<< HEAD
현재 브랜치의 내용
=======
합치려는 브랜치의 내용
>>>>>>> docs/readme
```

### 해결 순서

1. git status로 충돌 파일 확인
2. 양쪽 내용을 읽고 최종 내용 결정
3. 필요한 내용으로 수정
4. 충돌 표시를 제거하고 저장
5. 해결한 파일을 add
6. 병합 완료

```bash
git add README.md
git merge --continue
```

=> 어느 한쪽을 무조건 선택하는 것이 아니라 필요한 최종 내용을 만드는 작업

VS Code의 충돌 해결 화면을 사용해도 최종 파일은 직접 확인하기.

### 병합을 취소하려면

```bash
git merge --abort
```

=> 진행 중인 merge 취소 시도  
=> 병합 전 미커밋 수정이 있었다면 완전한 복원이 어려울 수 있으므로 작업 전 정리하기

## 8. 사용한 브랜치 정리

먼저 다른 브랜치로 이동:

```bash
git switch main
git branch -d docs/readme
```

=> 병합 여부 등을 확인하며 로컬 브랜치 삭제  
=> GitHub의 브랜치는 별도이며 함께 삭제되지 않음

## 9. GitHub에서 PR로 합치기

작업 브랜치를 업로드:

```bash
git push -u origin docs/readme
```

이후 GitHub에서:

```text
Pull Request 생성
       ↓
변경 내용 검토
       ↓
필요한 수정
       ↓
Merge
```

=> PR은 다른 브랜치로 변경을 합치자고 제안하는 기능

GitHub에서 병합했다면 로컬 main도 업데이트:

```bash
git switch main
git pull --ff-only
```

## 💌 꼭 기억하기

- merge는 현재 브랜치가 기준
- 브랜치 이동 전에 git status 확인
- 공유 브랜치의 기록은 혼자 판단해서 바꾸지 않기
- 충돌 표시만 없애지 말고 코드 내용도 확인하기

---

[← 기본 명령어](./01_basic.md) · [🌱 목차](./README.md) · [GitHub →](./03_github.md)