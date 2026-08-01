# 브랜치 전략 보완 — 작업 브랜치 prefix 다양화 및 release/hotfix 분기

> ### 문서 개요
> - 작성 시작: 2026-08-01 12:38
> - 최종 업데이트: 2026-08-01 12:38
> - 핵심 내용 한줄 요약: 기존에 `feature/*`만 정의되어 있던 작업 브랜치 prefix를 Conventional Commits 타입 체계로 확장하고, `release/*`·`hotfix/*` 분기 전략을 정석 Git Flow에 맞춰 추가로 확정
> - 관련 문서: [2026-07-30-git-workflow-and-conventions.md](2026-07-30-git-workflow-and-conventions.md)

<br/><br/>

## 배경

`git-workflow-and-conventions.md`에서 브랜치 전략을 Git Flow 스타일(`main`/`develop`/`feature/*`)로 정했지만, 개별 기능 작업 외에 버그 수정·리팩터링·스타일 수정 등 다양한 작업 유형을 모두 `feature/*`로 뭉뚱그리는 건 실제 작업 성격을 드러내지 못한다는 문제가 있었음. 또한 프로덕션에 이미 배포된 버전에서 긴급 수정이 필요하거나, 배포 직전 마무리 작업이 필요한 경우에 대한 흐름이 없었음. jote-frontend 작업 중 CLAUDE.md에 브랜치 전략을 옮겨 적으면서 이 두 가지 공백이 확인되어 보완함.

<br/><br/>

## 작업 브랜치 prefix 확장

### 결정: Conventional Commits 타입과 동일한 prefix 체계 사용
`develop`에서 분기하는 개별 작업 브랜치는 `feature/*` 하나가 아니라 아래 prefix들을 상황에 맞게 사용한다.

- `feature/*` — 새 기능
- `fix/*` — 버그 수정
- `refactor/*` — 동작 변경 없는 구조 개선
- `style/*` — 포맷팅 등 스타일 변경
- `chore/*` — 빌드/설정 등 관리성 작업
- `docs/*` — 문서 변경
- `test/*` — 테스트 추가/수정
- `perf/*` — 성능 개선

모두 `develop`으로 PR하고 **squash merge**하는 흐름은 기존 결정과 동일. 커밋 태그(`docs`/`chore`/`fix` 등 `jote-docs/CLAUDE.md` 커밋 규칙)와 동일한 체계를 브랜치 prefix에도 그대로 적용해 일관성을 맞춤.

<br/><br/>

## release / hotfix 분기 전략

### 배경
기존 문서는 "`develop`이 어느 정도 쌓이면 `main`으로 PR/머지"라고만 되어 있어, 배포 직전 마무리 작업이나 프로덕션 긴급 수정을 어느 브랜치에서 해야 하는지 정의가 없었음.

<br/>

### 결정: `release/*`, `hotfix/*` 도입 (정석 Git Flow)
- **`release/*`**: 배포 준비용. `develop`에서 분기해 버전 태깅 등 배포 전 마무리 작업만 담당. 준비가 끝나면 `main`과 `develop` 양쪽에 merge
- **`hotfix/*`**: 프로덕션 긴급 버그 수정용. `main`에서 분기해 수정하고, `develop`의 진행 상황과 무관하게 `main`과 `develop` 양쪽에 바로 merge

<br/><br/>

## 최종 브랜치 전략

```
main       ← 배포 가능한 안정 버전
  ↑ (merge commit, release 시점에 태그 예: v0.1.0)   ↑ (hotfix/* 직접 merge)
develop ──────────────────────────────────────────────┘
  ↑ (squash merge)              ↓ (release/* 분기, 배포 준비 후 main+develop 양쪽 merge)
feature/*, fix/*, refactor/*,
style/*, chore/*, docs/*,
test/*, perf/*  ← 개별 작업
```

- `main`, `develop` 둘 다 브랜치 보호 규칙(Ruleset `main-develop-protection`) 적용 — 직접 push 대신 PR로 진행. 그 외 브랜치는 보호 대상 아님

<br/><br/>

## 수정 히스토리

- 2026-08-01 12:38 — 최초 작성
