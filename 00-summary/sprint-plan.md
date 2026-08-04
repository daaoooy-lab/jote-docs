# 스프린트 계획

> ### 문서 개요
> - 작성 시작: 2026-07-31 15:00
> - 최종 업데이트: 2026-08-04 11:00
> - 핵심 내용 한줄 요약: 기획 완료 이후 개발 착수부터 Must have 완성까지의 8주 스프린트 일정 (계속 갱신됨)
> - 관련 문서: [2026-07-31-sprint-planning.md](../01-planning/2026-07-31-sprint-planning.md), [2026-07-31-repo-branch-protection-setup.md](../04-collaboration/2026-07-31-repo-branch-protection-setup.md)

<br/><br/>

## 이 문서에 대해

이 문서는 과정 기록이 아니라 **현재 확정된 스프린트 일정만 모아놓은 요약본**이다. 스프린트를 이렇게 짠 배경과 논의 과정은 "관련 문서"에 있고, 여기서는 일정과 상태만 다룬다. 스프린트가 끝나거나 계획이 조정될 때마다 이 문서를 직접 갱신한다.

<br/><br/>

## 전체 일정

- 기획 시작: 2026-07-26
- 개발 착수: 2026-07-31
- 목표 완성 시점: 2026-09-28 (개발 착수 기준 8주 + 4일. 08/17~08/21 해외 일정으로 인한 지연 반영)
- 개발 방식: 기능 단위 수직 슬라이스 (프론트+백엔드를 기능별로 함께 완성해나감)

<br/><br/>

## 스프린트 표

| 스프린트 | 기간 | 목표 | 상태 |
|---|---|---|---|
| Sprint 0 — 기획 | 07/26 ~ 07/30 (5일) | 기능 브레인스토밍/우선순위, 기술스택·아키텍처 결정, 브랜드 디자인, 협업 컨벤션, 인증 세부사항 확정 | 완료 |
| Sprint 1 | 07/31 ~ 08/09 | 레포 세팅 (jote-frontend/backend/ai 생성, 브랜치, CI, 코드 컨벤션, 이슈/PR 템플릿·라벨) | 진행중 |
| Sprint 2 | 08/10 ~ 08/13 | 메모 CRUD 기본 + 에디터(Tiptap) 붙이기 (인증 연동 Google/Kakao/Naver는 Sprint 1 지연으로 08/08~08/09에 조기 진행) | 예정 |
| Sprint 3 | 08/14 ~ 08/24 (08/17~08/21 공백) | 에디터 확장(체크리스트/코드블록) + 자동 저장 + Markdown export/import | 예정 |
| Sprint 4 | 08/25 ~ 08/31 | 임베딩 파이프라인 + pgvector 연동 + 시맨틱 검색 | 예정 |
| Sprint 5 | 09/01 ~ 09/07 | AI 채팅 — FastAPI RAG 파이프라인 구축 | 예정 |
| Sprint 6 | 09/08 ~ 09/14 | AI 채팅 — SSE 스트리밍 연동 + 채팅 UI | 예정 |
| Sprint 7 | 09/15 ~ 09/21 | Must have 통합 버그 픽스/폴리싱 (버퍼 주간) | 예정 |
| Sprint 8 | 09/22 ~ 09/28 | Should have 1단계 일부 (즐겨찾기/폴더, 다크모드, 이미지 첨부 등 우선순위 항목) | 예정 |

<br/><br/>

## Sprint 1 세부 일정

| 일자 | 작업 |
|---|---|
| 07/31 | ✅ jote-frontend/backend/ai 3개 레포 생성, main/develop 브랜치, default 브랜치 develop 설정, 브랜치 보호 규칙(Ruleset `main-develop-protection`) 설정, README 추가 |
| 08/01 | ✅ jote-frontend 스캐폴딩 (Next.js+TS, Tailwind, 폴더 구조는 Next.js 맞춤 Feature-Sliced Design, 절대경로 import, ESLint/Prettier). `.gitignore`는 CLI 자동 생성. 이슈/브랜치/PR 워크플로우, 커밋 Gitmoji 컨벤션 확정 및 jote-frontend 이슈 라벨 세팅도 조기 진행 |
| 08/02 ~ 08/05 | (개인 일정으로 진행 안 함 — 아래 08/06~08/09로 통합 이동) |
| 08/06 | jote-backend 스캐폴딩 (NestJS 모듈/컨트롤러/서비스 구조, DTO 검증, 공통 응답/예외 처리, Swagger) + jote-ai 스캐폴딩 (FastAPI 구조, Ruff/Black, Poetry/uv, Pydantic, OpenAPI). `.gitignore`는 backend는 CLI 자동 생성, ai는 직접 추가 (공식 스캐폴딩 CLI 없음) (원래 08/03 예정) |
| 08/07 | CI(GitHub Actions lint/build/test) 3개 레포 + Commit lint(commitlint+husky) + pre-commit(lint-staged) + Require status checks 규칙 재설정 (원래 08/04) + 환경변수/시크릿 관리 방식 정리 + 각 레포 `.env.example` 채우기 (원래 08/06) |
| 08/08 | 이슈/PR 템플릿, 라벨 세팅 (jote-frontend는 08/01에 조기 완료 — jote-backend/jote-ai/jote-docs/.github만 남음, 원래 08/05) + Supabase Google OAuth 연동 (Sprint 2 항목 조기 진행, 원래 08/07) |
| 08/09 | Supabase Kakao OAuth 연동 + Naver Custom OAuth2-only Provider 시도 (안 되면 절충안 착수) (Sprint 2 항목 조기 진행, 원래 각각 08/08·08/09) |

<br/><br/>

## Sprint 2 세부 일정

| 일자 | 작업 |
|---|---|
| ~~08/07~~ | ~~Supabase Google OAuth 연동~~ (Sprint 1 지연으로 08/08에 조기 진행, [Sprint 1 세부 일정](#sprint-1-세부-일정) 참고) |
| ~~08/08~~ | ~~Supabase Kakao OAuth 연동~~ (Sprint 1 지연으로 08/09에 조기 진행) |
| ~~08/09~~ | ~~Naver Custom OAuth2-only Provider 시도 (안 되면 절충안 착수)~~ (Sprint 1 지연으로 08/09에 조기 진행) |
| 08/10 | NestJS JWT 검증 가드 + 프론트 로그인 플로우 연결 |
| 08/11 | 메모 DB 스키마(Tiptap JSON) 설계 + 백엔드 CRUD API |
| 08/12 | 프론트 Tiptap 에디터 붙이기 (기본 텍스트 편집) |
| 08/13 | 메모 목록/상세 화면 + CRUD 프론트 연동, 버퍼 |

<br/><br/>

## Sprint 3 세부 일정

| 일자 | 작업 |
|---|---|
| 08/14 ~ 08/15 | 체크리스트, 코드 블록 에디터 확장 |
| 08/16 | 자동 저장 구현 (debounce + API) |
| 08/17 ~ 08/21 | (해외 일정으로 진행 안 함) |
| 08/22 ~ 08/23 | Markdown export/import (tiptap-markdown) (원래 08/17~08/18) |
| 08/24 | 버그 수정 (버퍼 압축, 원래 08/19~08/20 2일 → 1일) |

<br/><br/>

## Sprint 4 세부 일정

| 일자 | 작업 |
|---|---|
| 08/25 | Supabase pgvector 확장 설정 (원래 08/21) |
| 08/26 | 임베딩 공급자 연동 (OpenAI/Voyage 결정 후 API 연동) (원래 08/22) |
| 08/27 | 메모 저장 시 임베딩 생성/저장 파이프라인 (원래 08/23) |
| 08/28 ~ 08/29 | 벡터 유사도 검색 백엔드 API (원래 08/24~08/25) |
| 08/30 ~ 08/31 | 검색 UI 프론트 연동 + 버퍼 (원래 08/26~08/27) |

<br/><br/>

## Sprint 5 세부 일정

| 일자 | 작업 |
|---|---|
| 09/01 ~ 09/02 | RAG 검색 로직 (관련 메모 retrieve) (원래 08/28~08/29) |
| 09/03 ~ 09/04 | LLM 연동 (프롬프트 설계, 컨텍스트 구성) (원래 08/30~08/31) |
| 09/05 ~ 09/07 | FastAPI 채팅 엔드포인트 구현 + 테스트 (원래 09/01~09/03) |

<br/><br/>

## Sprint 6 세부 일정

| 일자 | 작업 |
|---|---|
| 09/08 ~ 09/09 | FastAPI SSE 스트리밍 구현 (원래 09/04~09/05) |
| 09/10 | NestJS 스트림 중계 (원래 09/06) |
| 09/11 ~ 09/13 | 프론트 채팅 UI (스트리밍 렌더링) (원래 09/07~09/09) |
| 09/14 | 통합 테스트 + 버퍼 (원래 09/10) |

<br/><br/>

## Must have 기능 ↔ 스프린트 매핑

| Must have 기능 | 커버하는 스프린트 |
|---|---|
| 메모 작성/수정/삭제 (CRUD) | Sprint 2 |
| 리치 텍스트 에디터 + Markdown 지원 | Sprint 2(기본 편집) + Sprint 3(확장, Markdown export/import) |
| 자동 저장 | Sprint 3 |
| 의미 기반(자연어) 검색 | Sprint 4 |
| 내 메모 기반 AI 채팅 | Sprint 5(RAG 파이프라인) + Sprint 6(스트리밍+UI) |

<br/><br/>

## 수정 히스토리

- 2026-08-04 11:00 — 08/17~08/21 해외 일정으로 진행 불가 반영. Sprint 3 후반(Markdown export/import, 버그수정+버퍼)과 Sprint 4 첫날(pgvector 설정)을 08/22~08/25로 재배치(버퍼 2일→1일 압축). 이번 지연은 압축만으로 흡수되지 않아 Sprint 4~8 전체를 4일씩 순연, 완성 목표일 09/24 → 09/28로 조정
- 2026-08-04 10:00 — 개인 일정으로 08/03~08/05 작업 미진행, 08/06~08/09로 통합 이동. Sprint 2 초반 인증 연동(Google/Kakao/Naver OAuth)을 08/08~08/09에 조기 진행하는 것으로 당겨와 압축한 결과, Sprint 2 이후(08/10 JWT 가드~)는 원래 일정과 그대로 일치해 추가 순연 없음. Sprint 1 기간 07/31~08/09, Sprint 2 기간 08/10~08/13으로 조정, 완성 목표일(09/24)은 변동 없음
- 2026-08-02 09:00 — 개인 일정으로 08/02(jote-backend 스캐폴딩) 작업 미진행, 08/03(jote-ai 스캐폴딩)에 합쳐서 진행하도록 변경. 08/06 버퍼를 흡수해 이후 일정(08/04~08/06)은 그대로 유지
- 2026-08-01 13:41 — Sprint 1 Day 2(08/01) 완료 반영, 라벨 세팅 조기 진행 사항 08/05에 메모
- 2026-07-31 18:00 — Sprint 1 Day 1 완료 반영, `.gitignore`/`.env.example`을 스캐폴딩 시점(08/01~08/06)으로 재배치
- 2026-07-31 15:00 — 최초 작성 (Sprint 0~8 전체 일정 및 세부 일정 정리)
