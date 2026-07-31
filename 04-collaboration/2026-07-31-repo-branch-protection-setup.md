# 레포 생성 및 브랜치 보호 규칙 설정

> ### 문서 개요
> - 작성 시작: 2026-07-31 18:00
> - 최종 업데이트: 2026-07-31 18:00
> - 핵심 내용 한줄 요약: jote-frontend/backend/ai 3개 레포 생성, default 브랜치를 develop으로 설정, GitHub Rulesets로 브랜치 보호 규칙(main-develop-protection)을 구성한 과정 정리
> - 관련 문서: [2026-07-30-git-workflow-and-conventions.md](2026-07-30-git-workflow-and-conventions.md), [sprint-plan.md](../00-summary/sprint-plan.md)

<br/><br/>

## 배경

Sprint 1(레포 세팅) 첫날 작업으로, `git-workflow-and-conventions.md`에서 정한 Git Flow 스타일(main/develop/feature) 전략을 실제 GitHub 레포 3개(jote-frontend, jote-backend, jote-ai)에 적용함. 레포 생성, 브랜치 구성, 브랜치 보호 규칙까지 설정하는 과정에서 나온 세부 결정을 정리함.

<br/><br/>

## Default 브랜치를 develop으로 설정

### 이유
GitHub의 default 브랜치는 새 PR을 만들 때 base 브랜치로 자동 지정됨. Jote 워크플로우는 `feature/*` → `develop`으로 PR을 보내는 구조인데, default가 `main`이면 PR 만들 때마다 base를 수동으로 `develop`으로 바꿔야 해서 실수로 `main`에 바로 머지될 위험이 있음.

### 결정
`develop`을 default 브랜치로 설정함. `main`은 릴리즈 시점에만 건드리는 보호된 브랜치로 역할을 명확히 함.

<br/><br/>

## 브랜치 보호 규칙 — GitHub Rulesets

### 설정 방식
Classic Branch protection rules 대신 최신 **Rulesets**(Settings → Rules → Rulesets) 사용. 하나의 룰셋에 여러 브랜치 패턴(`main`, `develop`)을 동시에 타겟으로 지정할 수 있어, 레포당 룰셋 1개로 두 브랜치를 함께 커버함.

- 룰셋 이름: **`main-develop-protection`** (처음엔 `main`으로 시작했으나, `develop`까지 포함하도록 범위가 넓어져서 이름을 변경함)
- Enforcement status: **Active**
- Target branches: `main`, `develop` 패턴 둘 다 추가
- Bypass list: **Repository admin** 추가 (혼자 개발하는 지금 단계에서 급한 상황에 스스로 규칙을 우회할 수 있도록. 팀원 합류 시 재검토 필요)

<br/>

### 규칙 항목별 설정

**켠 항목**
- Require a pull request before merging (Required approvals: 0 — 현재 리뷰어가 없어서)
- Require conversation resolution before merging
- Restrict deletions
- Block force pushes

**끈 항목**
- Restrict creations / Restrict updates — "Require a pull request before merging"와 중복되고 머지 흐름과 충돌 가능성 있어 제외
- Require merge queue, Require deployments to succeed, Require signed commits — 현재 규모/단계에 과함
- Dismiss stale approvals, Require review from teams/Code Owners, Restrict who dismisses reviews, Require approval of most recent push — 리뷰어(Required approvals=0)가 없어 의미 없음
- Require code scanning / code quality / Restrict code coverage — 별도 툴 셋업이 선행되어야 해서 현재 범위 밖
- **Require linear history — 반드시 꺼야 함.** `develop → main`은 merge commit으로 합치기로 한 기존 결정(`architecture-decisions.md` 아님, `git-workflow-and-conventions.md`)과 정면 충돌하기 때문

<br/>

### 삽질 1 — Require status checks to pass
처음에는 "체크 항목 없어도 켜두면 무해할 것"이라고 판단해서 켜려고 시도했으나, 실제로는 GitHub이 **"Required status checks cannot be empty"** 에러를 내며 저장을 막음. CI가 아직 없어서 선택할 체크 항목 자체가 없기 때문.

**결정**: 이 규칙은 지금은 꺼두고, Sprint 1 Day 5(CI 세팅)에서 실제 GitHub Actions 워크플로우가 생긴 뒤 그 체크 이름을 선택해서 다시 켜기로 함.

<br/>

### 삽질 2 — Allowed merge methods
`feature → develop`은 Squash, `develop → main`은 Merge commit으로 서로 다른 머지 방식을 쓰기로 이미 정했었는데, 룰셋 하나로 `main`/`develop`을 같이 묶다 보니 브랜치별로 다른 머지 방식을 강제할 방법이 없음.

**검토한 선택지**
- Squash/Merge commit 둘 다 허용하고, 어느 브랜치인지에 따라 수동으로 알맞은 방식을 선택
- `main`용, `develop`용 룰셋을 따로 만들어 각각 다른 머지 방식만 허용하도록 엄격하게 분리

**결정**: 혼자 개발하는 지금 단계에서 룰셋 두 개를 관리하는 번거로움이 실익보다 크다고 판단해, **두 방식 모두 허용**해두고 머지 시 수동으로 챙기는 쪽으로 결정함.

<br/><br/>

## 기본 파일 — README만 우선 추가, .gitignore/.env.example은 스캐폴딩 시점으로 이동

### 재검토 배경
원래 계획(Sprint 1 Day 1)엔 README, `.gitignore`, `.env.example`을 오늘 다 추가하는 것으로 되어 있었음. 그런데 `create-next-app`, `nest new` 같은 프레임워크 스캐폴딩 CLI가 각 스택에 맞는 `.gitignore`를 자동 생성해준다는 점을 다시 확인함. 미리 만들어두면 스캐폴딩 시점에 중복/충돌만 생김. `.env.example`도 실제로 어떤 환경변수가 필요한지는 기능을 붙여나가면서 자연스럽게 드러나는 것이라, 지금 미리 추측해서 만드는 것보다 필요해지는 시점(Supabase 연동, API 키 추가 등)에 그때그때 채우는 게 더 정확함.

<br/>

### 결정
- **README**: 오늘(07/31) 3개 레포 모두 추가 완료
- **.gitignore**: jote-frontend(08/01), jote-backend(08/02)는 각 스캐폴딩 CLI가 자동 생성 → 별도 작업 불필요. jote-ai(08/03)는 공식 스캐폴딩 CLI가 없어 프로젝트 구조 잡을 때 직접 추가
- **.env.example**: 각 레포에서 실제 연동 작업(Supabase, API 키 등)이 생기는 시점마다 필요한 항목을 추가해나가는 방식으로 진행

이 결정에 따라 Sprint 1 세부 일정을 조정함 (자세한 내용은 [sprint-plan.md](../00-summary/sprint-plan.md) 참고).

<br/><br/>

## 수정 히스토리

- 2026-07-31 18:00 — 최초 작성 (default 브랜치, 브랜치 보호 규칙 설정 과정 및 기본 파일 범위 재조정 정리)
