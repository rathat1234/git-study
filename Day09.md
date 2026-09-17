# Git Day 9 - Fork / Upstream

## 핵심 명령어

| 명령어 | 설명 |
|---|---|
| `git clone <URL>` | Remote Repository를 Local로 복제 |
| `git remote -v` | Remote 목록 확인 |
| `git remote add upstream <URL>` | 원본 Repository를 upstream으로 등록 |
| `git fetch upstream` | 원본 최신 정보 가져오기 |
| `git branch -a` | Local / Remote-tracking Branch 확인 |
| `git merge upstream/main` | 원본 변경을 현재 Local Branch에 병합 |
| `git push origin main` | Local main을 내 Fork에 Push |

---

## Fork와 Clone

```text
Fork
= GitHub 원본 → 내 GitHub 사본
```

```text
Clone
= Remote Repository → Local PC
```

---

## Fork 구조

```text
원본 Repository
      ↑
   upstream

Local Repository

      ↓
    origin
      ↓
내 Fork Repository
```

---

## origin

```text
origin
= 내가 Fork한 내 GitHub Repository
```

Fork Repository를 Clone하면 일반적으로 Clone한 주소가 `origin`으로 등록된다.

---

## upstream

```text
upstream
= Fork하기 전 원본 Repository
```

등록:

```bash
git remote add upstream <원본 URL>
```

---

## Remote-tracking Branch

```text
origin/main
= 내 Fork main의 마지막으로 확인한 상태
```

```text
upstream/main
= 원본 main의 마지막으로 확인한 상태
```

둘 다 Local에 존재하는 Remote-tracking Branch다.

---

## 원본 정보 가져오기

```bash
git fetch upstream
```

결과:

```text
upstream/main 갱신
```

하지만 Local main은 자동으로 움직이지 않는다.

---

## 원본 변경을 Local에 반영

```bash
git merge upstream/main
```

의미:

```text
upstream/main
      ↓
현재 Local Branch에 Merge
```

---

## 내 Fork에 반영

```bash
git push origin main
```

의미:

```text
Local main
    ↓
내 Fork main
```

---

## Fork 동기화 전체 과정

```bash
git fetch upstream
git merge upstream/main
git push origin main
```

흐름:

```text
upstream
   ↓ fetch

upstream/main
   ↓ merge

Local main
   ↓ push

origin/main
```

---

## Rebase 방식

```bash
git fetch upstream
git rebase upstream/main
```

최신 원본 위에서 현재 Branch Commit을 다시 생성할 수도 있다.

---

## 핵심 차이

```text
upstream
= 원본 Remote
```

```text
upstream/main
= 원본 main의 Remote-tracking Branch
```

```text
origin
= 내 Fork Remote
```

```text
origin/main
= 내 Fork main의 Remote-tracking Branch
```

---

## ahead / behind

```text
main -> D
upstream/main -> E
```

E가 D 다음 Commit이라면:

```text
Local main
= behind 1
```

---

## 핵심 정리

```text
git fetch upstream
= 원본 최신 정보 가져오기
```

```text
git merge upstream/main
= 원본 변경을 Local에 반영
```

```text
git push origin main
= Local 변경을 내 Fork에 반영
```

> Fork 기반 협업에서는 upstream에서 최신 변경을 가져오고, Local에서 작업한 뒤 origin인 내 Fork를 통해 원본 Repository에 기여한다.

---

## 다음 학습

### Day 10 - Pull Request Collaboration

- Feature Branch
- Push
- Pull Request
- Base / Compare
- Review
- 추가 Commit
- Merge
- Branch 정리