# 기술 스택 선정

> ### 문서 개요
> - 작성 시작: 2026-07-28 18:26
> - 최종 업데이트: 2026-07-28 18:26
> - 핵심 내용 한줄 요약: FE/BE/AI 분리 아키텍처 목표에 맞춰 프론트엔드, 백엔드, AI 서비스, DB, 배포 스택을 선정
> - 관련 문서: [2026-07-27-feature-prioritization.md](../01-planning/2026-07-27-feature-prioritization.md), [2026-07-28-should-have-order.md](../01-planning/2026-07-28-should-have-order.md)

<br/><br/>

## 배경

`feature-brainstorming.md`에서 정한 프로젝트 목표(Next.js 학습, AI Agent/LLM 구현 경험, 프론트엔드·백엔드·AI 분리 아키텍처 설계 경험)와 Must have 범위(에디터+Markdown, 자동저장, 의미 기반 검색, AI 채팅)를 기준으로 기술 스택을 선정함.

<br/><br/>

## 백엔드 프레임워크 검토

### NestJS와 비교 대상
- **Express** — 가장 널리 쓰이는 표준. 미니멀하지만 구조를 직접 설계해야 함
- **Fastify** — 성능 중심, 스키마 기반 검증 내장. Express보다 빠르지만 Nest만큼 구조를 강제하진 않음
- **Hono** — 최근 가장 뜨는 프레임워크. 초경량, TS-first, 엣지 런타임(Cloudflare Workers/Deno/Bun)에서도 동작
- **tRPC** — REST 대체가 아니라 FE-BE 타입 공유 RPC 레이어. Next.js 모노레포 안에 백엔드를 둘 때 강력하지만, 백엔드를 완전히 분리하려는 목표와는 궁합이 안 맞음

<br/>

### 결정: NestJS
Angular식 DI/데코레이터로 구조를 강제하는 프레임워크라, "아키텍처 설계 경험"이라는 프로젝트 목표에 맞게 처음 설계할 때 가이드 역할을 해줌. 이전 프로젝트에서 Hono(가볍고 사용하기 쉬움)를 이미 경험해봤기 때문에, 이번엔 새로운 스타일을 배우는 차원에서 NestJS를 선택함.

<br/><br/>

## DB 엔진 및 호스팅

### pgvector란
PostgreSQL의 오픈소스 확장으로, 벡터(임베딩) 데이터를 저장하고 유사도 검색(코사인 유사도, 유클리드 거리 등)을 가능하게 해줌. `CREATE EXTENSION vector`만으로 활성화되며, 메모 원본 데이터와 임베딩 벡터를 같은 DB·같은 테이블에 저장할 수 있어 Pinecone 같은 별도 벡터 DB 없이 의미 기반 검색/RAG를 구현할 수 있음.

<br/>

### DB 엔진과 호스팅의 구분
- **DB 엔진**: 어떤 DB 기술을 쓸지 (PostgreSQL, MySQL, MongoDB 등) — 프론트엔드로 치면 React/Vue 같은 프레임워크 선택에 해당
- **호스팅**: 그 엔진을 실제로 어디서 돌릴지 (직접 서버에 설치 vs 관리형 서비스) — 프론트엔드로 치면 Next.js 앱을 Vercel에 배포할지 직접 서버에 배포할지의 문제에 해당

<br/>

### 결정: PostgreSQL + pgvector, Supabase 호스팅
- DB 엔진은 PostgreSQL + pgvector로 선정 (별도 벡터 DB 인프라 없이 의미 기반 검색 구현 가능)
- 호스팅은 이전 프로젝트에서 이미 사용해본 **Supabase**로 결정. Supabase는 관리형 PostgreSQL이면서 pgvector를 기본 제공해 별도 설정이 필요 없고, DB 운영에 새로 학습 시간을 쓸 필요 없이 NestJS·AI 서비스·아키텍처 설계에 학습 에너지를 집중할 수 있음

<br/><br/>

## 최종 기술 스택

- **프론트엔드**: Next.js + TypeScript, Tiptap 에디터, Tailwind CSS
- **백엔드**: NestJS
- **AI 서비스**: Python + FastAPI (임베딩 생성, RAG 검색, LLM 호출 오케스트레이션 담당)
- **DB**: PostgreSQL + pgvector (Supabase 호스팅)
- **배포**: Vercel(프론트엔드) / Railway 또는 Fly.io(백엔드, AI 서비스)

<br/><br/>

## 다음 단계

- [ ] 임베딩 공급자 결정 (Claude+OpenAI 혼합 vs 오픈소스 자체 호스팅)

<br/><br/>

## 수정 히스토리

- 2026-07-28 18:26 — 최초 작성
