---
{"dg-publish":true,"permalink":"/02-concepts/backend/infra/git/github/","tags":["type/concept","area/concepts","domain/backend","topic/infra"],"dg-note-properties":{"type":"concept","tags":["type/concept","area/concepts","domain/backend","topic/infra"]}}
---


# Github

## GitHub

---

- **원격 저장소 & 협업 플랫폼**

- 코드 호스팅, 권한 관리, **Pull Request(코드 리뷰)**, **Issues(작업 관리)**, **Actions(CI/CD)**, 보안 분석 등을 제공

- 개인 계정/조직(Organization) 단위로 저장소 운영 가능

## 기본 개념

---

- **원격 저장소(Remote Repository)**: 클라우드에 있는 레포(예: GitHub의 저장소)

- **origin / upstream**`origin`: 내가 클론하거나 포크한 원격

- `upstream`: 보통 원본(포크한 경우 원본 레포를 upstream으로 추가)

- **Fork vs Clone****Fork**: 남의 레포를 내 계정으로 **복제(원격→원격)** → 권한 없어도 PR 가능

- **Clone**: 원격 레포를 **내 로컬로 복제(원격→내 PC)**

- **Pull Request(PR)**: 한 브랜치의 변경을 다른 브랜치(주로 `main`)로 넣어달라는 요청. 리뷰/토론/검증의 단위

- **Issues/Labels/Projects**: 할 일 관리, 태그(라벨) 표준화, 칸반/타임라인으로 진행 관리

- **Actions(워크플로우)**: 빌드/테스트/배포 자동화(CI/CD)

- **Environments & Secrets**: 배포 환경 구분, 토큰/키 등 민감정보 암호화 저장

- **Releases/Tags**: 배포 가능한 스냅샷, 버전 관리

- **Branch Protection Rules**: `main` 등 중요한 브랜치에 **직접 푸시 금지 + 리뷰/체크 필수** 같은 안전장치

- **Merge 전략****Squash**: 여러 커밋을 하나로 합쳐 깔끔한 히스토리(교육/소규모 권장)

- Merge commit / Rebase merge: 팀 규칙에 따라 선택

## 필수 설정(최소 셋업)

---

### 1) 새 레포 만들 때

- **기본 브랜치**: `main`

- **Initialize**: `README` 추가(권장), `.gitignore` 템플릿 선택, 라이선스(MIT 등) 필요 시 추가

- **About**: 설명, Topics(키워드), Website 링크 정리

### 2) 브랜치 보호(가장 중요)

> 경로: Settings → Code and automation → Branches → Add rule

- **Branch name pattern**: `main`

- 체크 권장:**Require a pull request before merging** (PR 필수)**Require approvals: 1** (리뷰 1명 이상)

- **Dismiss stale approvals** (새 커밋 시 이전 승인 무효)

- (선택) **Require review from Code Owners**

- **Require status checks to pass** (CI가 있다면 필수)**Require branches to be up to date** (머지 전 최신 동기화)

- **Require linear history** (Squash와 궁합 좋음)

- **Require conversation resolution** (코멘트 해결 필수)

- **Restrict who can push** (main 직접 푸시 금지)

- (상황에 따라) **Include administrators**

### 3) PR/머지 정책

> 경로: Settings → General → Pull Requests

- **Allow squash merging** (권장)

- **Disallow merge commits / rebase merges** (히스토리 간단 유지 시 권장)

- **Automatically delete head branches** (머지 후 브랜치 자동 삭제)

### 4) 협업 품질 기본값

- **Labels 정비**: `feat`, `fix`, `docs`, `refactor`, `chore`, `test` 등

- **PR/Issue 템플릿**(선택): `.github/pull_request_template.md`, `.github/ISSUE_TEMPLATE/*.md`

- **CODEOWNERS**(선택): 특정 경로 자동 리뷰어 지정 → `.github/CODEOWNERS`

### 5) Actions/보안

> 경로: Settings → Actions → General, Code security & analysis

- Actions 권한: 필요 시 **Read and write permissions**

- 외부 PR에 대한 권한/승인 정책 점검

- **Dependabot alerts/updates** 활성화

- **Code scanning(CodeQL)** 워크플로우(선택)

- **Secret scanning** 활성화(가능 시)

## 기본 흐름

---

1. (선택) **Fork** → 내 계정 원격 생성

1. **Clone** → 로컬로 가져오기

1. **브랜치 생성** → 로컬 작업, 커밋

1. **Push** → 원격 브랜치 생성

1. **PR 생성** → 리뷰/체크 통과

1. **Merge(Squash 권장)** → `main` 갱신

1. (옵션) **브랜치 자동 삭제**, 다음 작업 전 **main 최신화**
