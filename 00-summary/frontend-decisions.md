# 프론트엔드 확정 사항 요약

> ### 문서 개요
> - 작성 시작: 2026-08-01 13:41
> - 최종 업데이트: 2026-08-01 13:41
> - 핵심 내용 한줄 요약: jote-frontend 구현(툴링, 폴더 구조 등)에 대해 현재까지 확정된 사항만 모은 요약 문서 (계속 갱신됨)
> - 관련 문서: [2026-08-01-prettier-setup.md](../05-frontend/2026-08-01-prettier-setup.md), [2026-08-01-fsd-folder-structure.md](../05-frontend/2026-08-01-fsd-folder-structure.md)

<br/><br/>

## 이 문서에 대해

이 문서는 과정 기록이 아니라 **jote-frontend 구현 관련, 현재 확정된 사항만 모아놓은 요약본**이다. 서비스 전체(기능/기술스택/아키텍처/협업 컨벤션)에 대한 확정 사항은 [confirmed-decisions.md](confirmed-decisions.md)를 보고, 이 문서는 jote-frontend 레포 안에서의 구현 방식만 다룬다. 세부 논의 배경과 근거는 위 "관련 문서"에 있고, 여기서는 결론만 다룬다. 새로운 사항이 확정될 때마다 이 문서를 직접 갱신한다.

<br/><br/>

## 툴링

- **린트/포맷**: ESLint(`eslint-config-next` core-web-vitals + typescript) + Prettier(`prettier-plugin-tailwindcss`로 Tailwind 클래스 자동 정렬)
- Tailwind v4는 `tailwind.config.js` 없이 CSS-first로 설정(`@theme inline`), Prettier도 `tailwindStylesheet` 옵션으로 스타일시트 위치를 직접 참조

<br/><br/>

## 폴더 구조

Next.js 맞춤 Feature-Sliced Design(FSD).

```
src/
 ├─ app/                    # Next.js 라우팅 전용
 ├─ widgets/                # 여러 feature를 조합한 UI 블록
 ├─ features/               # 독립된 기능 단위 (auth, editor, note-search, ai-chat)
 ├─ entities/               # 핵심 도메인 모델 (Note, User)
 └─ shared/                 # ui, api, lib, hooks, config, types
```

- `app/`은 라우팅 전용, FSD의 `pages`/`processes` 레이어는 쓰지 않음 (Next `app/`이 pages 역할 대신, processes는 최신 FSD에서도 폐기됨)
- 슬라이스 내부는 FSD 표준 세그먼트(`ui`/`model`/`api`/`lib`) + `index.ts` 공개 API 패턴 사용
- 참조 방향: `shared` ← `entities` ← `features` ← `widgets` ← `app` (상위만 하위를 참조 가능)
- 절대경로 alias `@/*` → `./src/*`

<br/><br/>

## 아직 미확정

- 네이밍 규칙
- API 통신 레이어 (NestJS 호출 방식)

<br/><br/>

## 수정 히스토리

- 2026-08-01 13:41 — 최초 작성 (툴링, 폴더 구조 확정 사항 정리)
