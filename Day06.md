# Git Day 6 - Fetch / Pull

## 오늘 사용한 명령어

| 명령어 | 설명 |
|---|---|
| `git fetch origin` | `origin`의 최신 Commit 정보를 가져오고 Remote-tracking Branch를 갱신 |
| `git branch -vv` | Local Branch와 upstream의 ahead / behind 상태 확인 |
| `git status` | 현재 Branch의 동기화 상태와 Working Directory 상태 확인 |
| `git merge origin/main` | 가져온 `origin/main`의 변경사항을 현재 Local Branch에 병합 |
| `git pull` | 원격 변경사항을 가져오고 현재 Branch에 바로 반영 |
| `git pull --rebase` | 원격 변경사항을 가져온 뒤 merge 대신 rebase 방식으로 반영 |
| `git log --oneline --graph --all --decorate` | Local / Remote-tracking Branch 위치를 그래프로 확인 |

---

# 1. Fetch란?

`git fetch`는 Remote Repository의 최신 정보를 Local Git으로 가져오는 명령이다.

```bash
git fetch origin
```

의 의미:

```text
origin Remote의 최신 Commit 정보 가져오기
        ↓
Remote-tracking Branch 갱신
```

중요:

```text
fetch
≠
현재 Local Branch에 바로 반영
```

---

# 2. Fetch 전 상태

처음 Local과 Remote가 같은 위치라고 가정한다.

```text
A --- B --- C
          ↑
         main
          ↑
     origin/main
```

즉:

```text
main        -> C
origin/main -> C
```

이다.

---

# 3. GitHub에서 새로운 Commit이 생긴 경우

GitHub에서 누군가 새로운 Commit D를 만들었다고 하자.

Remote:

```text
A --- B --- C --- D
                ↑
        GitHub main
```

하지만 아직 Local에서는 fetch하지 않았다.

따라서 Local Git은 여전히:

```text
A --- B --- C
          ↑
         main
          ↑
     origin/main
```

까지만 알고 있을 수 있다.

---

# 4. origin/main은 실시간 Remote Branch가 아니다

`origin/main`은 GitHub 서버의 `main`을 실시간으로 보고 있는 것이 아니다.

정확히는:

> Local Git이 마지막으로 확인한 Remote main의 위치

이다.

따라서 GitHub에서 새로운 Commit D가 생겨도 `origin/main`은 자동으로 움직이지 않는다.

---

# 5. git fetch origin

Remote의 최신 상태를 가져온다.

```bash
git fetch origin
```

실행 후:

```text
A --- B --- C --- D
          ↑         ↑
         main   origin/main
```

즉:

```text
main        -> C
origin/main -> D
```

가 된다.

---

# 6. Fetch 후 main은 움직이지 않는다

`git fetch`는 Remote 정보 가져오기까지만 수행한다.

현재 직접 작업하는 Local Branch인 `main`은 자동으로 움직이지 않는다.

```text
fetch 전

main        -> C
origin/main -> C
```

```text
fetch 후

main        -> C
origin/main -> D
```

---

# 7. Fetch의 장점

Fetch의 가장 큰 장점:

> 내 현재 작업 Branch를 건드리지 않고 Remote 변경사항을 먼저 확인할 수 있다.

흐름:

```text
git fetch
    ↓
Remote 변경 가져오기
    ↓
main은 그대로
    ↓
Git Graph / log 확인
    ↓
merge 할지
rebase 할지
결정
```

즉:

```text
가져오기
+
확인
+
선택
```

이 가능하다.

---

# 8. ahead / behind

현재:

```text
main        -> C
origin/main -> D
```

이고 D가 C의 다음 Commit이라면 Local `main`은:

```text
behind 1
```

상태이다.

반대로:

```text
origin/main -> C
main        -> D
```

이면:

```text
ahead 1
```

이다.

기억:

```text
내 main이 앞
= ahead

내 main이 뒤
= behind
```

---

# 9. Fetch 후 Remote 변경 반영

Fetch만 하면 Local `main`은 움직이지 않는다.

가져온 Remote 변경사항을 현재 Branch에 반영하려면:

```bash
git merge origin/main
```

을 사용할 수 있다.

현재:

```text
main        -> C
origin/main -> D
```

상태에서:

```bash
git merge origin/main
```

을 실행하면 단순한 직선 구조라면 Fast-forward 된다.

결과:

```text
C --- D
      ↑
     main
      ↑
 origin/main
```

즉:

```text
main        -> D
origin/main -> D
```

가 된다.

---

# 10. Fetch + Merge

우리가 나누어서 실행한:

```bash
git fetch origin
git merge origin/main
```

은 개념적으로:

```text
Remote 변경 가져오기
        +
현재 Branch에 병합
```

이다.

---

# 11. git pull

이 두 단계를 한 번에 수행하는 명령이:

```bash
git pull
```

이다.

기본 개념:

```text
git pull
=
git fetch
+
git merge
```

---

# 12. Fetch와 Pull 차이

## git fetch

```bash
git fetch
```

결과:

```text
main        -> C
origin/main -> D
```

특징:

```text
Remote 변경 가져옴
main은 그대로
확인 후 결정 가능
```

## git pull

```bash
git pull
```

결과:

```text
main        -> D
origin/main -> D
```

특징:

```text
Remote 변경 가져옴
+
현재 Branch에 바로 반영
```

비교:

| 명령어 | Remote 정보 가져오기 | 현재 Branch 반영 |
|---|---:|---:|
| `git fetch` | O | X |
| `git pull` | O | O |

---

# 13. git pull --rebase

```bash
git pull --rebase
```

는 개념적으로:

```text
git fetch
+
git rebase
```

이다.

비교:

```text
git pull
= fetch + merge
```

```text
git pull --rebase
= fetch + rebase
```

`rebase`는 Day 7에서 자세히 학습한다.

---

# 14. Git Graph에서 Fetch 확인

Fetch 전:

```text
● C   main, origin/main
│
● B
│
● A
```

Remote에 새로운 Commit D가 생긴 뒤 Fetch:

```text
● D   origin/main
│
● C   main
│
● B
│
● A
```

즉 Git Graph에서 `origin/main`만 앞으로 이동한 것을 확인할 수 있다.

---

# 15. Git Graph에서 Merge 확인

Fetch 후:

```text
● D   origin/main
│
● C   main
```

여기서:

```bash
git merge origin/main
```

을 실행하면:

```text
● D   main, origin/main
│
● C
```

이 된다.

즉 `main`이 `origin/main` 위치까지 이동한다.

---

# 16. 오늘 실습 전체 흐름

처음:

```text
main
 ↓
C
 ↑
origin/main
```

GitHub에서 D 생성:

```text
GitHub

C --- D
      ↑
     main
```

Local에서는 아직 모름:

```text
Local

main        -> C
origin/main -> C
```

Fetch:

```bash
git fetch origin
```

결과:

```text
main        -> C
origin/main -> D
```

상태:

```text
behind 1
```

Merge:

```bash
git merge origin/main
```

결과:

```text
main        -> D
origin/main -> D
```

동기화 완료.

---

# 17. 오늘 헷갈렸던 부분

## main -> C / origin/main -> D

초기 답:

```text
ahead 1
```

이 아니라:

```text
behind 1
```

이다.

왜냐하면 Local main 기준으로 Remote보다 뒤에 있기 때문이다.

---

## git pull --rebase

```text
pull
= fetch + merge
```

```text
pull --rebase
= fetch + rebase
```

이다.

---

# 18. 오늘 문제 결과

## 문제 1

```bash
git fetch origin
```

기본적으로 움직이는 것:

```text
origin/main
```

✅ 정답

## 문제 2

현재:

```text
main        -> C
origin/main -> D
```

정답:

```text
behind 1
```

초기 답변에서 `ahead 1`이라고 했으나 개념 수정 완료.

## 문제 3

```bash
git fetch origin
git merge origin/main
```

두 작업을 한 번에 하는 명령:

```bash
git pull
```

✅ 정답

## 문제 4

Fetch의 장점:

```text
Remote 변경사항을 가져온 뒤
현재 Branch를 바로 변경하지 않고
확인 후 merge/rebase 여부를 결정할 수 있다.
```

✅ 정답

## 문제 5

```bash
git pull
```

은:

```text
fetch + merge
```

✅ 정답

## 문제 6

Git Graph:

```text
● D   origin/main
│
● C   main
```

의미:

```text
Remote-tracking Branch가
Local main보다 Commit 하나 앞에 있음
```

즉 Local main:

```text
behind 1
```

✅ 정답

## 문제 7

```bash
git pull --rebase
```

정답:

```text
fetch + rebase
```

초기 답변에서 merge라고 했으나 수정 완료.

---

# 19. 마지막 복습 문제 결과

현재:

```text
main        -> C
origin/main -> D
```

에서:

```bash
git merge origin/main
```

실행 시 움직이는 포인터:

```text
main
```

✅ 정답

Fetch 후 파일 내용이 바로 바뀌지 않는 이유:

```text
Remote Commit은 Local Git에 가져왔지만
현재 main Branch에는 아직 반영하지 않았기 때문
```

✅ 정답

```bash
git pull
```

관계:

```text
git fetch
+
git merge origin/main
```

✅ 정답

Git Graph:

```text
● D   origin/main
│
● C   main
```

Local main 상태:

```text
behind 1
```

✅ 정답

---

# 20. Day 6 핵심 비교

```text
git fetch
= 가져오기만
= origin/main 갱신
= main 그대로
```

```text
git merge origin/main
= 가져온 Remote 변경사항을
  현재 main에 반영
```

```text
git pull
= fetch + merge
```

```text
git pull --rebase
= fetch + rebase
```

---

# 21. Day 6 핵심 그래프

Fetch 전:

```text
        main
         ↓
A --- B --- C
         ↑
    origin/main
```

Fetch 후:

```text
A --- B --- C --- D
          ↑         ↑
         main   origin/main
```

Merge 후:

```text
A --- B --- C --- D
                    ↑
                   main
                    ↑
               origin/main
```

---

# Day 6 핵심 한 줄

> `git fetch`는 Remote의 최신 상태를 가져오지만 내 현재 Branch는 건드리지 않고, `git pull`은 가져온 뒤 현재 Branch에 반영까지 한다.

---

# Day 6 완료

다음 학습:

## Day 7 - Rebase

예정 내용:

- Rebase가 필요한 이유
- Merge와 Rebase 차이
- Commit을 재배치한다는 의미
- `git rebase main`
- `git rebase origin/main`
- `D → D'`처럼 Commit Hash가 바뀌는 이유
- Rebase Conflict
- `git rebase --continue`
- `git rebase --abort`
- `git push --force-with-lease`
- Git Graph에서 Rebase 전후 비교
- Merge Commit 없이 히스토리를 일자로 만드는 원리