# 스프린트 계획

> ### 문서 개요
> - 작성 시작: 2026-07-31 15:00
> - 최종 업데이트: 2026-07-31 15:00
> - 핵심 내용 한줄 요약: 기획 완료 이후 개발 착수부터 Must have 완성까지의 8주 스프린트 일정 (계속 갱신됨)
> - 관련 문서: [2026-07-31-sprint-planning.md](../01-planning/2026-07-31-sprint-planning.md)

<br/><br/>

## 이 문서에 대해

이 문서는 과정 기록이 아니라 **현재 확정된 스프린트 일정만 모아놓은 요약본**이다. 스프린트를 이렇게 짠 배경과 논의 과정은 "관련 문서"에 있고, 여기서는 일정과 상태만 다룬다. 스프린트가 끝나거나 계획이 조정될 때마다 이 문서를 직접 갱신한다.

<br/><br/>

## 전체 일정

- 기획 시작: 2026-07-26
- 개발 착수: 2026-07-31
- 목표 완성 시점: 2026-09-24 (개발 착수 기준 8주)
- 개발 방식: 기능 단위 수직 슬라이스 (프론트+백엔드를 기능별로 함께 완성해나감)

<br/><br/>

## 스프린트 표

| 스프린트 | 기간 | 목표 | 상태 |
|---|---|---|---|
| Sprint 0 — 기획 | 07/26 ~ 07/30 (5일) | 기능 브레인스토밍/우선순위, 기술스택·아키텍처 결정, 브랜드 디자인, 협업 컨벤션, 인증 세부사항 확정 | 완료 |
| Sprint 1 | 07/31 ~ 08/06 | 레포 세팅 (jote-frontend/backend/ai 생성, 브랜치, CI, 코드 컨벤션, 이슈/PR 템플릿·라벨) | 예정 |
| Sprint 2 | 08/07 ~ 08/13 | 인증 연동(Google/Kakao/Naver) + 메모 CRUD 기본 + 에디터(Tiptap) 붙이기 | 예정 |
| Sprint 3 | 08/14 ~ 08/20 | 에디터 확장(체크리스트/코드블록) + 자동 저장 + Markdown export/import | 예정 |
| Sprint 4 | 08/21 ~ 08/27 | 임베딩 파이프라인 + pgvector 연동 + 시맨틱 검색 | 예정 |
| Sprint 5 | 08/28 ~ 09/03 | AI 채팅 — FastAPI RAG 파이프라인 구축 | 예정 |
| Sprint 6 | 09/04 ~ 09/10 | AI 채팅 — SSE 스트리밍 연동 + 채팅 UI | 예정 |
| Sprint 7 | 09/11 ~ 09/17 | Must have 통합 버그 픽스/폴리싱 (버퍼 주간) | 예정 |
| Sprint 8 | 09/18 ~ 09/24 | Should have 1단계 일부 (즐겨찾기/폴더, 다크모드, 이미지 첨부 등 우선순위 항목) | 예정 |

<br/><br/>

## Sprint 1 세부 일정

| 일자 | 작업 |
|---|---|
| 07/31 | jote-frontend/backend/ai 3개 레포 생성 + main/develop 브랜치 + 브랜치 보호 규칙 + 기본 파일(README, .gitignore, .env.example) |
| 08/01 | jote-frontend 스캐폴딩 (Next.js+TS, Tailwind, 폴더 구조, 절대경로 import, ESLint/Prettier) |
| 08/02 | jote-backend 스캐폴딩 (NestJS 모듈/컨트롤러/서비스 구조, DTO 검증, 공통 응답/예외 처리, Swagger) |
| 08/03 | jote-ai 스캐폴딩 (FastAPI 구조, Ruff/Black, Poetry/uv, Pydantic, OpenAPI) |
| 08/04 | CI(GitHub Actions lint/build/test) 3개 레포 + Commit lint(commitlint+husky) + pre-commit(lint-staged) |
| 08/05 | 이슈/PR 템플릿, 라벨 세팅 |
| 08/06 | 환경변수/시크릿 관리 방식 정리 + 버퍼 |

<br/><br/>

## Sprint 2 세부 일정

| 일자 | 작업 |
|---|---|
| 08/07 | Supabase Google OAuth 연동 |
| 08/08 | Supabase Kakao OAuth 연동 |
| 08/09 | Naver Custom OAuth2-only Provider 시도 (안 되면 절충안 착수) |
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
| 08/17 ~ 08/18 | Markdown export/import (tiptap-markdown) |
| 08/19 ~ 08/20 | 버그 수정 + 버퍼 |

<br/><br/>

## Sprint 4 세부 일정

| 일자 | 작업 |
|---|---|
| 08/21 | Supabase pgvector 확장 설정 |
| 08/22 | 임베딩 공급자 연동 (OpenAI/Voyage 결정 후 API 연동) |
| 08/23 | 메모 저장 시 임베딩 생성/저장 파이프라인 |
| 08/24 ~ 08/25 | 벡터 유사도 검색 백엔드 API |
| 08/26 ~ 08/27 | 검색 UI 프론트 연동 + 버퍼 |

<br/><br/>

## Sprint 5 세부 일정

| 일자 | 작업 |
|---|---|
| 08/28 ~ 08/29 | RAG 검색 로직 (관련 메모 retrieve) |
| 08/30 ~ 08/31 | LLM 연동 (프롬프트 설계, 컨텍스트 구성) |
| 09/01 ~ 09/03 | FastAPI 채팅 엔드포인트 구현 + 테스트 |

<br/><br/>

## Sprint 6 세부 일정

| 일자 | 작업 |
|---|---|
| 09/04 ~ 09/05 | FastAPI SSE 스트리밍 구현 |
| 09/06 | NestJS 스트림 중계 |
| 09/07 ~ 09/09 | 프론트 채팅 UI (스트리밍 렌더링) |
| 09/10 | 통합 테스트 + 버퍼 |

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

- 2026-07-31 15:00 — 최초 작성 (Sprint 0~8 전체 일정 및 세부 일정 정리)
