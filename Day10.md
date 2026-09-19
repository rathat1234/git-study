# Git Day 10 - Pull Request Collaboration

## 핵심 명령어

| 명령어 | 설명 |
|---|---|
| `git switch -c <branch>` | Feature Branch 생성 |
| `git push -u origin <branch>` | Remote Feature Branch 생성 및 upstream 설정 |
| `git push` | 추가 Commit을 기존 Remote Branch에 반영 |
| `git pull origin main` | Merge된 Remote main을 Local에 반영 |
| `git branch -d <branch>` | Local Feature Branch 삭제 |
| `git push origin --delete <branch>` | Remote Feature Branch 삭제 |
| `git fetch --prune` | 삭제된 Remote Branch 추적 Ref 정리 |

---

## PR 방향

```text
base
= 변경사항을 받을 Branch

compare
= 변경사항을 보내는 Branch
```

이번 실습:

```text
day10-pr → main
```

---

## PR 생성 흐름

```text
main
 ↓
day10-pr 생성
 ↓
작업
 ↓
commit
 ↓
push
 ↓
PR
```

---

## PR 생성 후 추가 Commit

PR을 만든 후에도 같은 Feature Branch에서:

```bash
git add ...
git commit -m "..."
git push
```

하면 기존 PR이 자동으로 갱신된다.

새 PR을 만들 필요가 없다.

---

## 이유

PR은 특정 Commit 하나가 아니라:

```text
base Branch
vs
compare Branch
```

의 차이를 추적하기 때문이다.

---

## Merge

실제 PR Merge 결과:

```text
450f6c6 Merge pull request #1
```

그래프:

```text
*   Merge Commit
|\
| * Feature Commit 2
| * Feature Commit 1
|/
* 이전 main
```

Feature Branch가 main에서 갈라졌다가 Merge Commit에서 다시 합쳐졌다.

---

## GitHub Merge 후 Local 동기화

GitHub Remote가 변경돼도 Local Branch는 자동으로 움직이지 않는다.

```bash
git switch main
git pull origin main
```

으로 최신 main을 가져온다.

---

## Branch 정리

Local 삭제:

```bash
git branch -d day10-pr
```

Remote 삭제:

```bash
git push origin --delete day10-pr
```

---

## Prune

```bash
git fetch --prune
```

Remote에 더 이상 존재하지 않는 Branch의 오래된 Remote-tracking Ref를 Local에서 정리한다.

```text
prune
= Remote Branch 추적 정보 청소
```

---

## 전체 과정

```text
Feature 생성
↓
Commit
↓
Push
↓
PR
↓
추가 Commit + Push
↓
기존 PR 자동 갱신
↓
Review
↓
Merge
↓
Local main Pull
↓
Local Feature 삭제
↓
Remote Feature 삭제
↓
Prune
```

---

## 핵심 정리

```text
base
= 받을 Branch
```

```text
compare
= 보낼 Branch
```

```text
같은 Feature Branch에 Push
= 기존 PR 갱신
```

```text
git branch -d
= Local 삭제
```

```text
git push origin --delete
= Remote 삭제
```

```text
git fetch --prune
= 삭제된 Remote Branch 추적 Ref 정리
```

---

## 다음 학습

### Day 11 - Recovery / Reflog

- `git reflog`
- `reset --hard` 복구
- 삭제된 Branch 복구
- HEAD 이동 기록
- `git log`와 `reflog` 차이