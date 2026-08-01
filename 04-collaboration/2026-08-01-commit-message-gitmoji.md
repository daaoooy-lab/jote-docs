# 커밋 메시지에 Gitmoji 추가

> ### 문서 개요
> - 작성 시작: 2026-08-01 13:20
> - 최종 업데이트: 2026-08-01 13:20
> - 핵심 내용 한줄 요약: 커밋 메시지 태그 앞에 표준 Gitmoji를 붙이기로 하고, 코드 3개 레포는 이슈번호까지 포함한 형식을 확정
> - 관련 문서: [2026-08-01-issue-branch-pr-workflow.md](2026-08-01-issue-branch-pr-workflow.md), [CLAUDE.md](../CLAUDE.md)

<br/><br/>

## 배경

기존 커밋 메시지 형식(`tag: Title`)이 밋밋해서, gitmoji.dev 기준 표준 Gitmoji를 태그 앞에 붙이기로 함. 코드 3개 레포(`jote-frontend`/`jote-backend`/`jote-ai`)는 "작업 진행 순서"([issue-branch-pr-workflow.md](2026-08-01-issue-branch-pr-workflow.md))에 따라 모든 작업이 이슈에서 시작하므로, 커밋 메시지에도 이슈번호를 포함하기로 함. `jote-docs`(이 저장소)는 문서 저장소라 깔끔함을 우선해 gitmoji·이슈번호 둘 다 붙이지 않기로 함(이슈도 원래 안 씀).

<br/><br/>

## 결정

### 코드 3개 레포 (jote-frontend / jote-backend / jote-ai)
형식: `<gitmoji> (#이슈번호) <type>: <Title>`

| type | gitmoji |
|---|---|
| `feat` | ✨ |
| `fix` | 🐛 |
| `docs` | 📝 |
| `refactor` | ♻️ |
| `style` | 🎨 |
| `perf` | ⚡️ |
| `test` | ✅ |
| `chore` | 🔧 |
| `hotfix` | 🚑️ |
| `release` | 🔖 |

예: `✨ (#12) feat: Add Tiptap editor`

<br/>

### jote-docs (이 저장소)
형식: `tag: Title` (gitmoji·이슈번호 둘 다 없음 — 문서 저장소라 깔끔하게)

- `docs`, `chore`, `fix`

예: `docs: Add Jote feature brainstorming doc and project conventions`

<br/><br/>

## 수정 히스토리

- 2026-08-01 13:24 — jote-docs는 gitmoji도 붙이지 않는 것으로 정정 (문서 저장소라 깔끔하게)
- 2026-08-01 13:20 — 최초 작성
