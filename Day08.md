# Git Day 8 - Stash / Restore / Reset

## 핵심 명령어

| 명령어 | 설명 |
|---|---|
| `git stash` | 작업 중 변경사항 임시 보관 |
| `git stash list` | Stash 목록 확인 |
| `git stash pop` | Stash 적용 후 삭제 |
| `git stash apply` | Stash 적용 후 유지 |
| `git restore <file>` | Working Directory의 파일 변경사항 복구 |
| `git restore --staged <file>` | Staging 취소, Working 변경사항 유지 |
| `git reset --soft <commit>` | HEAD/Branch만 이동 |
| `git reset --mixed <commit>` | HEAD 이동 + Staging 복구 |
| `git reset --hard <commit>` | HEAD + Staging + Working 모두 복구 |

---

## 1. Stash

`git stash`는 아직 Commit하기 싫은 작업을 임시로 보관한다.

```text
수정 중인 작업
     ↓
git stash
     ↓
Stash에 임시 보관
     ↓
Working Directory 정리
```

Branch pointer는 움직이지 않는다.

### pop

```bash
git stash pop
```

```text
적용 + Stash 삭제
```

### apply

```bash
git stash apply
```

```text
적용 + Stash 유지
```

---

## 2. Restore

```bash
git restore file.txt
```

Working Directory의 파일 변경사항을 복구한다.

```text
HEAD     = v1
Staging  = v1
Working  = v2

git restore file.txt

HEAD     = v1
Staging  = v1
Working  = v1
```

---

## 3. Restore --staged

```bash
git restore --staged file.txt
```

Staging Area만 복구하고 Working Directory 수정사항은 유지한다.

```text
실행 전

HEAD     = v1
Staging  = v2
Working  = v2
```

```text
실행 후

HEAD     = v1
Staging  = v1
Working  = v2
```

---

## 4. Reset

Reset은 Branch / HEAD 위치를 움직일 수 있다.

주요 옵션:

```text
--soft
--mixed
--hard
```

---

## 5. Reset --soft

```bash
git reset --soft HEAD~1
```

```text
HEAD     = 이전 Commit
Staging  = 유지
Working  = 유지
```

즉:

```text
Commit만 취소
```

---

## 6. Reset --mixed

```bash
git reset --mixed HEAD~1
```

```text
HEAD     = 이전 Commit
Staging  = 이전 Commit 상태
Working  = 수정사항 유지
```

즉:

```text
Commit + Add 취소
```

`git reset`의 기본 모드는 `--mixed`이다.

---

## 7. Reset --hard

```bash
git reset --hard HEAD~1
```

```text
HEAD     = 이전 Commit
Staging  = 이전 Commit 상태
Working  = 이전 Commit 상태
```

즉:

```text
Commit + Add + Working 수정까지 취소
```

Working Directory 수정사항까지 사라질 수 있으므로 주의한다.

---

## 8. 세 옵션 비교

| 옵션 | HEAD | Staging | Working |
|---|---|---|---|
| `--soft` | 이동 | 유지 | 유지 |
| `--mixed` | 이동 | 복구 | 유지 |
| `--hard` | 이동 | 복구 | 복구 |

암기:

```text
soft
= Commit만
```

```text
mixed
= Commit + Add
```

```text
hard
= Commit + Add + Working
```

---

## 9. Stash / Restore / Reset 비교

```text
stash
= 작업 변경사항 임시 보관
```

```text
restore
= 파일 상태 복구
= Branch pointer는 움직이지 않음
```

```text
reset
= Branch/HEAD 위치 이동
= 옵션에 따라 Staging / Working까지 복구
```

---

## 핵심 정리

```text
git stash
= 현재 작업 임시 보관
```

```text
git restore file.txt
= Working Directory 변경 버리기
```

```text
git restore --staged file.txt
= Add 취소, 파일 수정은 유지
```

```text
git reset --soft HEAD~1
= Commit만 취소
```

```text
git reset --mixed HEAD~1
= Commit + Add 취소
```

```text
git reset --hard HEAD~1
= Commit + Add + Working 수정까지 취소
```

---

## 다음 학습

### Day 9 - Fork / Upstream

- Fork
- Clone과 Fork 차이
- `origin`
- `upstream`
- `git remote add upstream`
- `git fetch upstream`
- `upstream/main`
- Fork Repository 동기화