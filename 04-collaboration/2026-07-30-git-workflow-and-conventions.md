# 깃 브랜치 전략 및 개발 컨벤션 준비

> ### 문서 개요
> - 작성 시작: 2026-07-30 22:54
> - 최종 업데이트: 2026-07-31 18:00
> - 핵심 내용 한줄 요약: jote-frontend/jote-backend/jote-ai 3개 레포에 적용할 브랜치 전략, 머지 방식, 릴리즈 태깅 방침을 정하고 남은 협업 준비 작업을 목록화
> - 관련 문서: [2026-07-28-tech-stack-selection.md](../03-architecture/2026-07-28-tech-stack-selection.md)

<br/><br/>

## 배경 및 목적

서비스 기획/기술 스택/아키텍처/디자인 시스템까지 정리가 끝나서 개발 착수 단계로 넘어가는 시점. 지금은 혼자 개발하지만 언제 팀원이 합류할지 모르니, 처음부터 협업 가능한 형태로 깃 워크플로우를 잡아두기로 함. jote-frontend, jote-backend, jote-ai 3개 레포에 공통으로 적용할 브랜치 전략과 컨벤션을 논의함.

<br/><br/>

## 브랜치 전략

### 결정: Git Flow 스타일 (main / develop / feature)

```
main       ← 배포 가능한 안정 버전
  ↑
develop    ← 다음 릴리즈로 통합되는 브랜치
  ↑
feature/*  ← 개별 기능 작업
```

- `feature/*` 브랜치는 `develop`으로 PR
- `develop`은 어느 정도 쌓이면 `main`으로 PR/머지 (배포 시점)
- 브랜치 보호 규칙은 `main`, `develop` 둘 다 적용 (feature 브랜치는 보호 불필요)

<br/><br/>

## 머지 방식

### feature → develop: Squash merge
Conventional Commits와 궁합이 맞음 — PR 하나가 병합되면 "feat: ...", "fix: ..." 같은 커밋 하나로 정리됨. feature 브랜치 안에서 쌓인 wip/오타 수정 커밋들이 develop 히스토리에 남지 않아 깔끔함.

<br/>

### develop → main: Merge commit (일반 merge)
develop에는 이미 squash로 정리된 커밋들이 쌓여있으므로, 여기서 또 squash하면 오히려 구조가 뭉개짐. 일반 merge commit으로 합쳐서 "이 지점이 릴리즈 시점"이라는 표시만 남김.

> 위 머지 방식은 임시 확정. 실제 작업하면서 조정 가능.

<br/><br/>

## Release 태깅

`develop`을 `main`에 머지하는 시점에 태그(예: `v0.1.0`)를 남기기로 함.

<br/><br/>

## 다음 단계 — 협업 준비 작업 목록

### 남은 아키텍처 결정
- [x] 인증 세부사항 (소셜 로그인 Provider 종류, 세션 만료 정책) → [2026-07-31-auth-provider-and-session-policy.md](../03-architecture/2026-07-31-auth-provider-and-session-policy.md)에서 확정

<br/>

### 레포 공통 세팅 (jote-frontend / jote-backend / jote-ai)
- [x] 레포 생성 + `main`, `develop` 브랜치 세팅 (default 브랜치는 `develop`) → [2026-07-31-repo-branch-protection-setup.md](2026-07-31-repo-branch-protection-setup.md)
- [x] 브랜치 보호 규칙 (Ruleset `main-develop-protection`으로 구성) → [2026-07-31-repo-branch-protection-setup.md](2026-07-31-repo-branch-protection-setup.md)
- [ ] 이슈 템플릿 작성
- [ ] PR 템플릿 작성
- [ ] 이슈/PR 라벨 작업 (bug, feature, docs 등 기본 세트)
- [ ] CI(GitHub Actions) — PR 시 lint/build/test 자동 실행
- [ ] Commit lint 훅 — `commitlint` + `husky`로 커밋 형식 자동 검증
- [x] 기본 파일 — README (완료), `.gitignore`/`.env.example`은 각 스택 스캐폴딩 시점에 추가하기로 조정
- [ ] 환경변수/시크릿 관리 방식 (배포 플랫폼별 등록 방식 정리)

<br/>

### 트랙별 코드 컨벤션
- [ ] 공통: pre-commit 훅(`lint-staged`), 테스트 프레임워크/커버리지 기준
- [ ] jote-frontend: ESLint+Prettier(+Tailwind 정렬), 폴더 구조, 네이밍 규칙, API 통신 레이어, 절대경로 import
- [ ] jote-backend: ESLint+Prettier, 모듈/컨트롤러/서비스 구조, DTO 검증, 공통 응답 포맷/예외 처리, Swagger 문서화
- [ ] jote-ai: 린터/포매터(Ruff/Black), 프로젝트 구조, 의존성 관리 도구(Poetry vs uv), Pydantic 스키마 검증, OpenAPI 문서화

<br/><br/>

## 수정 히스토리

- 2026-07-31 18:00 — 레포 생성/브랜치 보호 규칙/README 완료 체크, 관련 문서 링크 추가
- 2026-07-31 11:00 — 인증 세부사항 결정 완료 체크
- 2026-07-30 22:54 — 최초 작성 (브랜치 전략, 머지 방식, 릴리즈 태깅 결정 및 협업 준비 작업 목록 정리)
