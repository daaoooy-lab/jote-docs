# 작업 진행 순서 — 이슈 생성부터 PR까지

> ### 문서 개요
> - 작성 시작: 2026-08-01 12:49
> - 최종 업데이트: 2026-08-01 12:49
> - 핵심 내용 한줄 요약: 설정 작업이든 기능 작업이든 예외 없이 따라야 할 이슈 생성 → 브랜치 생성 → 작업 → 커밋/푸시 → PR 순서와 브랜치 네이밍 규칙을 확정
> - 관련 문서: [2026-07-30-git-workflow-and-conventions.md](2026-07-30-git-workflow-and-conventions.md), [2026-08-01-branch-strategy-refinement.md](2026-08-01-branch-strategy-refinement.md), [2026-07-31-repo-branch-protection-setup.md](2026-07-31-repo-branch-protection-setup.md)

<br/><br/>

## 배경

`branch-strategy-refinement.md`에서 작업 브랜치 prefix와 release/hotfix 분기까지는 정리했지만, 실제로 작업을 시작할 때 이슈부터 만드는지, 브랜치 이름은 정확히 어떤 형식인지에 대한 규칙이 빠져 있었음. jote-frontend에서 Prettier 세팅 작업을 `develop`에 바로 커밋해버린 뒤에야 이 공백을 인지함. `repo-branch-protection-setup.md`에서 브랜치 보호 규칙에 **Repository admin bypass**를 걸어둔 게 원래는 "혼자 개발하는 단계에서 급한 상황"을 위한 예외였는데, 실제로는 매번 이 bypass로 `develop`에 직접 커밋하게 되는 문제를 발견해 작업 순서를 명문화함.

<br/><br/>

## 결정: 이슈 → 브랜치 → 작업 → 커밋/푸시 → PR

설정 작업(예: Prettier 세팅)이든 기능 작업이든 예외 없이 아래 순서를 따른다.

1. **이슈 생성** — 작업 단위로 GitHub 이슈를 먼저 만든다
2. **브랜치 생성** — `develop`에서 분기, 이름은 `<type>/<이슈번호>-<핵심키워드>` (예: `feature/12-editor-tiptap`, `chore/8-prettier-setup`). `<type>`은 `branch-strategy-refinement.md`에서 정한 작업 브랜치 prefix(`feature`, `fix`, `refactor`, `style`, `chore`, `docs`, `test`, `perf`) 중 하나
3. **작업 진행**
4. **커밋 및 푸시**
5. **`develop`으로 PR 생성** — 머지는 기존 결정대로 squash merge

`develop`에 직접 커밋/푸시하지 않는다. Repository admin bypass는 정말 급한 상황에만 쓰고, 평소 작업에는 사용하지 않는다.

<br/><br/>

## 적용 범위

이 워크플로우는 `jote-frontend`/`jote-backend`/`jote-ai` 3개 코드 레포에만 적용된다. `jote-docs`(이 저장소)는 `main` 단일 브랜치만 쓰며 이슈/브랜치/PR 없이 바로 커밋한다 (`jote-docs/CLAUDE.md`의 "브랜치 정책" 참고).

<br/><br/>

## 수정 히스토리

- 2026-08-01 12:49 — 최초 작성
