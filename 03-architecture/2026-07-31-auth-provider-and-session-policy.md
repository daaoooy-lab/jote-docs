# 인증 세부사항 결정 (소셜 로그인 Provider, 세션 만료 정책)

> ### 문서 개요
> - 작성 시작: 2026-07-31 11:00
> - 최종 업데이트: 2026-07-31 14:00
> - 핵심 내용 한줄 요약: 소셜 로그인 Provider(Google + Kakao + Naver, Supabase Auth 하나로 통일)와 세션 절대 만료 정책(30일)을 확정
> - 관련 문서: [2026-07-28-architecture-decisions.md](2026-07-28-architecture-decisions.md), [2026-07-30-git-workflow-and-conventions.md](../04-collaboration/2026-07-30-git-workflow-and-conventions.md)

<br/><br/>

## 배경

`architecture-decisions.md`에서 Supabase Auth 채택은 확정했지만, 세부사항(소셜 로그인 Provider 종류, 세션 만료 정책)은 "다음 단계"로 미뤄뒀었음. `git-workflow-and-conventions.md`의 협업 준비 체크리스트에서도 "남은 아키텍처 결정"으로 동일 항목이 걸려있었음. 이 문서에서 두 항목을 확정함.

<br/><br/>

## 소셜 로그인 Provider

### 검토한 후보
Google, Kakao, Naver 3개를 고려함. 한국 사용자 대상 서비스라 Kakao/Naver 비중도 함께 검토함.

<br/>

### 제약사항 발견 및 재검토
Supabase Auth 확인 결과 Google/Kakao는 기본 지원 Provider지만, **Naver는 기본 지원 목록에 없음**. 처음에는 이 때문에 Naver를 MVP에서 제외하는 방향으로 잠정 결정했었음.

다시 찾아보니 Supabase가 **Custom OAuth2-only Provider** 기능을 제공함 — OIDC 디스커버리 문서가 없는 Provider도 authorization/token/userinfo 엔드포인트 URL을 직접 등록하면 기본 Provider와 동일하게 `signInWithOAuth({ provider: 'custom:naver' })`로 사용 가능함. Naver가 정확히 이 케이스(OAuth2는 지원하지만 OIDC 디스커버리는 없음)에 해당해서, 별도 백엔드 구현 없이 Supabase Dashboard/Admin API 설정만으로 통합 가능함.

또한 실무에서 Google/Kakao/Naver를 모두 지원하는 서비스들도 대부분 Firebase 같은 BaaS의 기본 지원에 의존하기보다, 자체 서버(백엔드)에서 OAuth를 직접 처리하는 경우가 많다는 것도 확인함 — Client Secret을 서버에서 안전하게 관리할 수 있고 정책 변경에 유연하기 때문. Jote는 이미 NestJS 백엔드가 있어 이 방식도 선택지가 될 수 있으나, Custom OAuth2 Provider로 Supabase 안에서 해결되면 별도 인증 인프라를 직접 구축하지 않아도 되는 기존 결정의 장점을 그대로 유지할 수 있음.

<br/>

### 결정: Supabase Auth 하나로 통일, Naver는 Custom OAuth2-only Provider로 등록
Google/Kakao는 Supabase 기본 Provider로, Naver는 Supabase Custom OAuth2-only Provider(authorization/token/userinfo 엔드포인트 직접 등록)로 설정해 3개 Provider 모두 MVP부터 지원함. 신원/세션 관리는 계속 Supabase Auth 하나로 통일됨.

**절충안 (Fallback)**: 만약 실제 연동 과정에서 Naver의 OAuth2 응답 형식이 Supabase Custom Provider 설정과 맞지 않아 동작하지 않으면, NestJS 백엔드에서 Naver OAuth 핸드셰이크(공식 `passport-naver` 패키지 등 활용)를 직접 처리한 뒤, 검증된 사용자 정보를 Supabase Admin API(service role)로 넘겨 사용자를 생성/연결하고 세션을 발급하는 방식으로 전환함. 이 경우에도 신원/세션 저장소는 Supabase 하나로 유지됨.

<br/><br/>

## 세션(로그인 유지) 만료 정책

### 검토한 방식
- **Supabase 기본값** — Access token 1시간 + Refresh token, 활동 기준으로 계속 갱신되어 사실상 무기한 로그인 유지
- **절대 만료 기간 설정** — Refresh token 자체에 최대 유효기간을 둬서, 활동 여부와 무관하게 해당 기간이 지나면 재로그인 요구

<br/>

### 결정: 절대 만료 30일
Refresh token의 최대 유효기간을 30일로 설정함. 30일이 지나면 활동 여부와 무관하게 재로그인이 필요함.

<br/><br/>

## 다음 단계

- [ ] Supabase 프로젝트에서 Google/Kakao OAuth 앱 등록 및 연동 (레포 세팅 단계에서 진행)
- [ ] Naver Custom OAuth2-only Provider 등록 시도 (안 되면 절충안: NestJS 직접 처리 + Supabase Admin API 브리지)
- [ ] Refresh token 30일 절대 만료 설정 (Supabase 프로젝트 Auth 설정에서 구성)

<br/><br/>

## 수정 히스토리

- 2026-07-31 14:00 — 소셜 로그인 Provider 재검토: Naver를 Supabase Custom OAuth2-only Provider로 통합, 절충안(NestJS 직접 처리 + Admin API 브리지) 추가
- 2026-07-31 11:00 — 최초 작성 (소셜 로그인 Provider, 세션 만료 정책 확정)
