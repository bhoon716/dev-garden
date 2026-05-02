---
{"dg-publish":true,"permalink":"/02-concepts/backend/infra/git/git/","tags":["type/concept","area/concepts","domain/backend","topic/infra"],"dg-note-properties":{"type":"concept","tags":["type/concept","area/concepts","domain/backend","topic/infra"]}}
---


# Git

## Git

---

- **분산 버전 관리 시스템(Distributed Version Control System)**

- 코드(파일)의 변경 이력을 기록하고, 필요할 때 과거로 되돌리거나 여러 버전을 관리할 수 있게 도와줌

- 협업 시에는 각자 브랜치에서 작업 후 합치는 방식으로 충돌을 줄이고 관리가 쉬워짐

## 기본 개념

---

- **저장소(Repository)**: 프로젝트 전체를 담는 상자

- **스테이징 영역(Stage, Index)**: 타임캡슐에 넣기 전에 임시로 모아두는 공간

- **커밋(Commit)**: 타임캡슐을 땅에 묻어 기록을 확정하는 행위

- **브랜치(Branch)**: 작업 줄기. 같은 프로젝트 안에서 여러 갈래의 실험/개발을 할 수 있음

- **체크아웃(Checkout/Switch)**: 다른 줄기로 이동하기

- **머지(Merge)**: 갈라진 줄기를 다시 합치는 것

- **리베이스(Rebase)**: 작업 기록을 다른 줄기 위로 재배치하여 히스토리를 깔끔하게 만드는 것

## 기본 명령어와 의미

---

```bash
# 현재 상태 확인
git status          # 어떤 파일이 변경/스테이징/커밋되었는지 확인
git log             # 커밋 이력 확인

# 브랜치 관리
git branch                  # 브랜치 목록 보기
git branch <브랜치명>        # 브랜치 생성
git branch -d <브랜치명>     # 브랜치 삭제
git checkout <브랜치명>      # 다른 브랜치로 이동
git checkout -b <브랜치명>   # 브랜치 생성 + 이동

# 파일 변경 관리
git add <파일명>    # 파일을 스테이징 영역에 추가
git add .           # 모든 변경 파일을 스테이징
git commit -m "메시지"   # 스테이징된 변경을 기록(커밋)

# 브랜치 병합
git merge <브랜치명>   # 현재 브랜치에 대상 브랜치의 커밋을 합침

# 변경 비교
git diff            # 워킹 디렉토리 ↔ 스테이징 영역 차이 확인
git diff --staged   # 스테이징 ↔ 최신 커밋 차이 확인

# 되돌리기/복구
git restore <파일명>           # 파일을 최근 커밋 상태로 되돌림
git restore --staged <파일명>  # 스테이징에서 빼기
git reset --soft HEAD~1        # 마지막 커밋 취소, 변경은 보존
git reset --hard HEAD~1        # 마지막 커밋 취소 + 변경도 제거
git revert <커밋ID>            # 특정 커밋을 반대 동작으로 되돌림
```

## Git의 기본 흐름

---

1. **작업(파일 수정)** → 워킹 디렉토리

1. **git add** → 스테이징 영역으로 이동

1. **git commit** → 로컬 저장소에 기록 확정

1. 필요 시 **브랜치로 나눠 작업** → `git merge` 또는 `git rebase`로 합침
