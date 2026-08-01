# GitHub 이슈 라벨 체계

> ### 문서 개요
> - 작성 시작: 2026-08-01 13:41
> - 최종 업데이트: 2026-08-01 13:41
> - 핵심 내용 한줄 요약: Organization 설정에서 만든 26개 커스텀 라벨을 확정하고, 기존 GitHub 기본 라벨과의 충돌을 정리
> - 관련 문서: [2026-07-30-git-workflow-and-conventions.md](2026-07-30-git-workflow-and-conventions.md)

<br/><br/>

## 배경

`daaoooy-lab` organization 설정의 **Repository defaults** 페이지에서 라벨을 편집했는데, 이 설정은 앞으로 새로 생성되는 레포에만 적용되고 이미 있는 레포(jote-frontend 등)에는 소급 적용되지 않음을 확인함. GitHub 라벨 자체가 레포별 독립 리소스라 조직 차원에서 자동 동기화되는 기능이 없음. 기존 레포에는 `gh label create`로 직접 반영해야 함.

<br/><br/>

## 라벨 목록

### 우선순위
- `priority: 🔴🔴🔴` `#f9d0c4` — 즉시 처리해야 하는 중요 작업
- `priority: 🟡🟡🟡` `#fef2c0` — 중요하지만 긴급하지 않은 작업
- `priority: 🟢🟢🟢` `#c2e0c6` — 급하지 않은 작업

<br/>

### 작업 유형
- `♻️ refactor` `#d5f0aa` — 리팩터링 작업
- `⚙️ chore` `#dbdbdb` — 설정 및 기타
- `⚡ performance` `#ffbd80` — 성능 개선
- `✨ feature` `#ffe396` — 새로운 기능 추가
- `🐛 bug` `#ff6f5c` — 버그 발생
- `📝 documentation` `#a5cffa` — 문서 작업
- `🔧 enhancement` `#ECB762` — 기능 개선
- `🚀 deployment` `#f40818` — 배포
- `🚨 hotfix` `#fa9789` — 급하게 고쳐야 하는 것

<br/>

### 영역
- `🖥️ FE` `#80d0ff` — 프론트엔드 관련
- `🔌 BE` `#759fd2` — 백엔드 관련
- `🗃️ DB` `#0052cc` — DB 스키마·마이그레이션
- `📱 responsive` `#c2e3ff` — 반응형 관련 작업
- `🎨 UI/UX` `#b7abd6` — UI/UX 업데이트
- `🤖 AI` `#1b7fdf` — AI 관련 기능

<br/>

### 상태/논의
- `❓ question` `#ffb09e` — 질문 혹은 제안
- `💬 Request For Comments` `#ffffff` — 기술적 제안이나 설계에 대해 의견을 구함
- `💭 thinking` `#222222` — 기술 결정 고민, 구현 방향 논의
- `🚫 Blocked` `#F79797` — 외부 요인이나 의존성으로 인해 현재 작업을 진행할 수 없는 상태
- `🤚 help wanted` `#EF4388` — 도움 필요

<br/>

### AI 어시스턴트
- `⚪ Claude` `#C15F3C` — Claude와 함께한 작업
- `⚪ Codex` `#54595d` — Codex와 함께한 작업
- `⚫ Gemini` `#4285F4` — Gemini와 함께한 작업

<br/><br/>

## 적용 현황

- `bug`/`documentation`/`enhancement`/`help wanted`/`question` 등 이름이 겹치는 GitHub 기본 라벨(이모지 없음)은 삭제하고 커스텀 버전만 유지
- `duplicate`/`good first issue`/`invalid`/`wontfix`도 함께 삭제
- 2026-08-01 기준 **jote-frontend**에만 적용 완료. jote-backend/jote-ai/jote-docs/.github는 아직 미적용 — 필요해지면 동일한 방식(`gh label create --force`)으로 반영

<br/><br/>

## 수정 히스토리

- 2026-08-01 13:41 — 최초 작성
