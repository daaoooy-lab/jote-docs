# 확정 사항 요약

> ### 문서 개요
> - 작성 시작: 2026-07-28 18:50
> - 최종 업데이트: 2026-07-28 18:50
> - 핵심 내용 한줄 요약: 서비스 기능, 기술 스택, 아키텍처에 대해 현재까지 확정된 사항만 모은 요약 문서 (계속 갱신됨)
> - 관련 문서: [2026-07-27-feature-prioritization.md](../01-planning/2026-07-27-feature-prioritization.md), [2026-07-28-should-have-order.md](../01-planning/2026-07-28-should-have-order.md), [2026-07-28-tech-stack-selection.md](../03-architecture/2026-07-28-tech-stack-selection.md), [2026-07-28-embedding-provider-selection.md](../03-architecture/2026-07-28-embedding-provider-selection.md), [2026-07-28-architecture-decisions.md](../03-architecture/2026-07-28-architecture-decisions.md)

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
Supabase Auth로 로그인 처리(소셜 로그인 포함) + JWT 발급, NestJS는 JWT 검증 가드만 구현

<br/><br/>

## 수정 히스토리

- 2026-07-28 18:50 — 최초 작성 (서비스 기능, 기술 스택, 아키텍처 확정 사항 정리)
