# 아키텍처 미결 사항 결정

> ### 문서 개요
> - 작성 시작: 2026-07-28 18:45
> - 최종 업데이트: 2026-07-28 18:45
> - 핵심 내용 한줄 요약: 콘텐츠 저장 포맷, 서비스 간 통신 구조, 인증 전략 3가지 아키텍처 미결 사항을 결정
> - 관련 문서: [2026-07-28-tech-stack-selection.md](2026-07-28-tech-stack-selection.md), [2026-07-28-embedding-provider-selection.md](2026-07-28-embedding-provider-selection.md)

<br/><br/>

## 배경

`tech-stack-selection.md`에서 기술 스택은 정했지만, `feature-prioritization.md`의 "초반부터 잡고 가야 하는 것" 중 콘텐츠 저장 포맷, 서비스 간 통신 구조, 인증 전략 3가지는 미결로 남아있었음. 이 문서에서 세 항목을 논의하고 결정함.

<br/><br/>

## 콘텐츠 저장 포맷

### 검토한 선택지
- **Tiptap JSON을 소스로 저장** — 에디터가 다루는 구조(문단, 리스트, 코드블록, 체크리스트 등)를 그대로 JSON으로 저장하고, Markdown은 export/import 시점에만 변환
- **Markdown을 소스로 저장** — 텍스트 자체를 소스로 두고, 에디터는 그걸 파싱해서 표시

<br/>

### 결정: Tiptap JSON을 소스로 저장
Markdown은 `tiptap-markdown` 같은 확장으로 export/import 시점에만 변환해서 지원함.

**근거**
- Must have인 AI 작성 지원(문장 다듬기, 이어쓰기 등)은 에디터 안에서 선택한 범위에 정확히 작용해야 하는데, 구조화된 JSON(어떤 노드/마크가 선택됐는지 명확함)이 Markdown 텍스트보다 이 작업을 훨씬 안정적으로 만듦
- 체크리스트, 코드 블록 같은 구조화된 요소도 JSON이 표현하기 자연스러움 (Markdown은 checklist 문법이 도구마다 달라 표준이 아님)
- Markdown 지원은 변환 레이어로 처리하면 되고, "외부 연동"에 있던 Markdown export 기능과도 자연스럽게 연결됨

<br/><br/>

## 서비스 간 통신 구조

### 구조
```
Frontend (Next.js)
      │  (REST API, 인증된 요청만)
      ▼
Backend (NestJS)  ← 유일한 공개 API, 인증/인가 담당
      │  (내부 네트워크, REST)
      ▼
AI 서비스 (FastAPI)  ← 외부에 노출 안 됨, 내부 호출만 신뢰
```

<br/>

### 결정: NestJS를 단일 진입점(게이트웨이)으로, AI 서비스는 내부 전용
프론트엔드든 AI 서비스든 모든 외부 요청은 항상 NestJS를 거침.

**근거**
- 인증/인가를 NestJS 한 곳에서만 처리하면 됨. AI 서비스가 직접 노출되면 별도 인증 로직이 중복됨
- 프론트엔드 입장에서 API가 항상 하나(NestJS)라 단순함
- AI 서비스는 내부에서 오는 요청을 이미 검증됐다고 신뢰하고, 순수 AI 로직(임베딩, RAG, LLM 호출)에만 집중 가능

<br/>

### AI 채팅 스트리밍 처리
Must have인 "내 메모 기반 AI 채팅"의 실시간 스트리밍 응답도 같은 구조로 처리함: FastAPI가 LLM 응답을 SSE(Server-Sent Events)로 스트리밍하면, NestJS가 그 스트림을 그대로 프론트엔드까지 중계.

<br/>

### 통신 방식
NestJS ↔ FastAPI는 둘 다 내부 서비스라 별도 메시지 큐 없이 **REST(HTTP/JSON)**로 충분함. 실시간 협업이 없어서(Won't have) 메시지 큐나 gRPC 같은 인프라는 과함.

<br/><br/>

## 인증 전략

### 인증 전략이 다루는 범위
- 로그인 방식 (이메일/비밀번호, 소셜 로그인, 매직링크 등)
- 로그인 이후 신원 유지 방식 (JWT 토큰, 세션 쿠키 등)
- 각 서비스가 그 신원을 어떻게 검증하는지

<br/>

### 검토한 선택지
- **Supabase Auth** — 이미 채택한 Supabase에 내장된 인증 기능. 이메일/비밀번호, 소셜 로그인(Google, GitHub 등), JWT 발급까지 처리
- **NextAuth.js (Auth.js)** — Next.js 진영의 인기 인증 라이브러리. NestJS 백엔드와 별도로 다시 연동 필요
- **NestJS 자체 구현** — Passport.js 기반으로 회원가입/로그인/JWT 발급을 직접 구현

<br/>

### 결정: Supabase Auth
소셜 로그인(Google, GitHub 등)을 포함해 Supabase Auth를 사용하고, NestJS는 발급된 JWT를 검증하는 가드만 구현함.

**근거**
- DB 호스팅 결정과 동일한 논리 — 이미 채택한 Supabase 인프라를 재사용해서, 인증 시스템을 처음부터 구현하는 데 학습 에너지를 쓰지 않고 NestJS 아키텍처 설계·AI/RAG 쪽에 집중
- 원하는 소셜 로그인이 Supabase Auth에 기본 제공되어 별도 구현 불필요
- "NestJS가 단일 진입점에서 인가를 담당한다"는 통신 구조 결정과 자연스럽게 연결 (Supabase는 신원 발급만, 검증/인가는 NestJS가 담당)

<br/><br/>

## 다음 단계

- [ ] 인증 전략 세부 사항 추가 검토 (예: 소셜 로그인 Provider 종류, 세션 만료 정책 등)

<br/><br/>

## 수정 히스토리

- 2026-07-28 18:45 — 최초 작성
