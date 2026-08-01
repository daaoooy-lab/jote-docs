# 확정 사항 요약

> ### 문서 개요
> - 작성 시작: 2026-07-28 18:50
> - 최종 업데이트: 2026-08-01 13:24
> - 핵심 내용 한줄 요약: 서비스 기능, 기술 스택, 아키텍처, 협업 컨벤션에 대해 현재까지 확정된 사항만 모은 요약 문서 (계속 갱신됨)
> - 관련 문서: [2026-07-27-feature-prioritization.md](../01-planning/2026-07-27-feature-prioritization.md), [2026-07-28-should-have-order.md](../01-planning/2026-07-28-should-have-order.md), [2026-07-28-tech-stack-selection.md](../03-architecture/2026-07-28-tech-stack-selection.md), [2026-07-28-embedding-provider-selection.md](../03-architecture/2026-07-28-embedding-provider-selection.md), [2026-07-28-architecture-decisions.md](../03-architecture/2026-07-28-architecture-decisions.md), [2026-07-31-auth-provider-and-session-policy.md](../03-architecture/2026-07-31-auth-provider-and-session-policy.md), [2026-07-30-git-workflow-and-conventions.md](../04-collaboration/2026-07-30-git-workflow-and-conventions.md), [2026-08-01-branch-strategy-refinement.md](../04-collaboration/2026-08-01-branch-strategy-refinement.md), [2026-08-01-issue-branch-pr-workflow.md](../04-collaboration/2026-08-01-issue-branch-pr-workflow.md), [2026-08-01-commit-message-gitmoji.md](../04-collaboration/2026-08-01-commit-message-gitmoji.md), [2026-08-01-github-label-taxonomy.md](../04-collaboration/2026-08-01-github-label-taxonomy.md)
> - jote-frontend 구현 관련 확정 사항은 [frontend-decisions.md](frontend-decisions.md) 별도 문서 참고

<br/><br/>

## 이 문서에 대해

이 문서는 과정 기록이 아니라 **현재 확정된 사항만 모아놓은 요약본**이다. 세부 논의 배경과 근거는 위 "관련 문서"에 있고, 여기서는 결론만 다룬다. 새로운 사항이 확정될 때마다 이 문서를 직접 갱신한다.

<br/><br/>

## 서비스 기능

### Must have
- 메모 작성 / 수정 / 삭제
- 리치 텍스트 에디터 + Markdown 지원
- 자동 저장
- 의미 기반(자연어) 검색
- 내 메모 기반 AI 채팅

<br/>

### Should have 착수 순서
1. 비-AI 기본 — 키워드 전체 검색, 즐겨찾기/고정, 폴더 구조, 체크리스트, 코드 블록, 이미지 첨부, 다크모드/테마
2. 독립적인 AI 작성 지원 — 메모 요약, 태그 자동 생성
3. 구조에 얹는 AI — 메모 자동 분류 / 카테고리 추천
4. 임베딩 인프라 위에 얹는 AI — 관련 메모 추천

<br/>

### Won't have
- 종단간 암호화
- 멀티 디바이스 동기화, 실시간 공동 편집, 댓글
- 크로스 플랫폼(모바일 네이티브) — 웹으로 한정
- Notion/Evernote 마이그레이션, 웹 클리퍼

<br/><br/>

## 기술 스택

- **프론트엔드**: Next.js + TypeScript, Tiptap 에디터, Tailwind CSS, SUIT 폰트
- **백엔드**: NestJS
- **AI 서비스**: Python + FastAPI
- **DB**: PostgreSQL + pgvector (Supabase 호스팅)
- **임베딩 공급자**: 관리형 API (OpenAI 또는 Voyage AI)
- **인증**: Supabase Auth (소셜 로그인 포함)
- **배포**: Vercel(프론트엔드) / Railway 또는 Fly.io(백엔드, AI 서비스)

<br/><br/>

## 아키텍처

### 콘텐츠 저장 포맷
Tiptap JSON을 소스로 저장, Markdown은 export/import 시점에만 변환

<br/>

### 서비스 간 통신 구조
```
Frontend (Next.js)
      │  REST API, 인증된 요청만
      ▼
Backend (NestJS)  ← 유일한 공개 API, 인증/인가 담당
      │  내부 네트워크, REST
      ▼
AI 서비스 (FastAPI)  ← 외부 비노출, 내부 호출만 신뢰
```
- AI 채팅 스트리밍: FastAPI가 SSE로 스트리밍 → NestJS가 그대로 중계

<br/>

### 인증
- Supabase Auth로 로그인 처리(소셜 로그인 포함) + JWT 발급, NestJS는 JWT 검증 가드만 구현
- 소셜 로그인 Provider: Google + Kakao(기본 지원) + Naver(Custom OAuth2-only Provider로 등록). 안 되면 NestJS 직접 처리 + Supabase Admin API 브리지로 절충
- 세션 만료 정책: Refresh token 절대 만료 30일 (활동 여부와 무관하게 30일 후 재로그인 필요)

<br/><br/>

## 협업 컨벤션

### Git 브랜치 전략
정석 Git Flow (`main` / `develop` / 작업 브랜치 / `release/*` / `hotfix/*`). **`jote-frontend`/`jote-backend`/`jote-ai` 3개 코드 레포에만 적용** — `jote-docs`(이 저장소)는 `main` 단일 브랜치만 쓰며 이슈/브랜치/PR 없이 바로 커밋한다.

```
main       ← 배포 가능한 안정 버전
  ↑ (merge commit, release 시점에 태그 예: v0.1.0)   ↑ (hotfix/* 직접 merge)
develop ──────────────────────────────────────────────┘
  ↑ (squash merge)              ↓ (release/* 분기, 배포 준비 후 main+develop 양쪽 merge)
feature/*, fix/*, refactor/*,
style/*, chore/*, docs/*,
test/*, perf/*  ← 개별 작업
```

- 작업 브랜치는 `feature/*`, `fix/*`, `refactor/*`, `style/*`, `chore/*`, `docs/*`, `test/*`, `perf/*` (Conventional Commits 타입과 동일 체계), `develop`에서 분기해 `develop`으로 squash merge
- `release/*`는 `develop`에서 분기해 배포 준비, 완료 후 `main`+`develop` 양쪽 merge
- `hotfix/*`는 `main`에서 분기해 긴급 수정, `main`+`develop` 양쪽 merge
- `main`, `develop` 둘 다 브랜치 보호 규칙(Ruleset `main-develop-protection`) 적용, 직접 push 금지·PR로만 진행

<br/>

### 작업 진행 순서
설정 작업이든 기능 작업이든 예외 없이 적용:

1. 이슈 생성
2. 브랜치 생성 — `develop`에서 분기, `<type>/<이슈번호>-<핵심키워드>` (예: `feature/12-editor-tiptap`, `chore/8-prettier-setup`)
3. 작업 진행
4. 커밋 및 푸시
5. `develop`으로 PR 생성 (squash merge)

`develop`에 직접 커밋/푸시하지 않는다. Repository admin bypass는 정말 급한 상황에만 사용.

<br/>

### 커밋 메시지 — Gitmoji
형식: `<gitmoji> (#이슈번호) <type>: <Title>` (코드 3개 레포). `jote-docs`는 문서 저장소라 gitmoji·이슈번호 없이 `tag: Title`만 사용.

| type | gitmoji | type | gitmoji |
|---|---|---|---|
| `feat` | ✨ | `test` | ✅ |
| `fix` | 🐛 | `chore` | 🔧 |
| `docs` | 📝 | `hotfix` | 🚑️ |
| `refactor` | ♻️ | `release` | 🔖 |
| `style` | 🎨 | `perf` | ⚡️ |

예: `✨ (#12) feat: Add Tiptap editor`

<br/><br/>

## 수정 히스토리

- 2026-08-01 13:24 — jote-docs 커밋은 gitmoji도 붙이지 않는 것으로 정정 (문서 저장소라 깔끔하게)
- 2026-08-01 13:20 — 협업 컨벤션에 커밋 메시지 Gitmoji 규칙 추가
- 2026-08-01 12:52 — Git 브랜치 전략이 코드 3개 레포에만 적용됨을 명시 (jote-docs는 main 단일 브랜치)
- 2026-08-01 12:49 — 협업 컨벤션에 작업 진행 순서(이슈→브랜치→PR) 추가
- 2026-08-01 12:38 — 협업 컨벤션(Git 브랜치 전략) 섹션 추가: 작업 브랜치 prefix 다양화, release/hotfix 분기 전략 확정
- 2026-07-31 14:00 — 소셜 로그인 Provider 재검토: Naver도 Supabase Custom OAuth2 Provider로 MVP부터 지원하도록 변경
- 2026-07-31 11:00 — 인증 Provider/세션 만료 정책 확정 사항 추가
- 2026-07-28 18:50 — 최초 작성 (서비스 기능, 기술 스택, 아키텍처 확정 사항 정리)
