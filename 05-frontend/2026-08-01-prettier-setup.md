# Prettier 세팅

> ### 문서 개요
> - 작성 시작: 2026-08-01 13:41
> - 최종 업데이트: 2026-08-01 13:41
> - 핵심 내용 한줄 요약: jote-frontend에 Prettier를 설치하고 ESLint, Tailwind 클래스 정렬과 함께 동작하도록 세팅
> - 관련 문서: [2026-07-30-git-workflow-and-conventions.md](../04-collaboration/2026-07-30-git-workflow-and-conventions.md)

<br/><br/>

## 배경

`git-workflow-and-conventions.md`의 트랙별 코드 컨벤션에서 jote-frontend 범위로 "ESLint+Prettier(+Tailwind 정렬)"를 정해뒀는데, ESLint는 스캐폴딩 시점(create-next-app)에 이미 되어 있었고 Prettier가 빠져 있었음. (`jote-frontend` 이슈 [#1](https://github.com/daaoooy-lab/jote-frontend/issues/1) / PR [#2](https://github.com/daaoooy-lab/jote-frontend/pull/2))

<br/><br/>

## 결정

- `prettier`, `eslint-config-prettier`, `prettier-plugin-tailwindcss` 설치
- `.prettierrc.json`에 `tailwindStylesheet` 옵션으로 Tailwind v4 스타일시트 위치 지정 (v4는 `tailwind.config.js`가 없어서 플러그인이 스타일시트 위치를 직접 알아야 함)
- `.prettierignore`에서 `AGENTS.md` 제외 — Next.js 스캐폴딩 툴이 `<!-- BEGIN/END -->` 마커로 관리하는 파일이라 포맷팅 대상에서 제외
- `eslint.config.mjs` 배열 마지막에 `eslint-config-prettier` 추가해 ESLint 스타일 규칙과 충돌 방지
- `package.json`에 `format`(전체 포맷), `format:check`(CI용 검사) 스크립트 추가

<br/><br/>

## 수정 히스토리

- 2026-08-01 13:41 — 최초 작성
