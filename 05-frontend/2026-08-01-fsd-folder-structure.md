# Next.js 맞춤 Feature-Sliced Design 폴더 구조

> ### 문서 개요
> - 작성 시작: 2026-08-01 13:41
> - 최종 업데이트: 2026-08-01 13:41
> - 핵심 내용 한줄 요약: jote-frontend의 폴더 구조로 Feature-Sliced Design(FSD)을 채택하되, Next.js App Router의 예약 폴더와 충돌하지 않도록 조정한 구조를 확정
> - 관련 문서: [2026-07-30-git-workflow-and-conventions.md](../04-collaboration/2026-07-30-git-workflow-and-conventions.md)

<br/><br/>

## 배경

Sprint 1(08/01) 스캐폴딩 항목 중 폴더 구조가 남아있었음. 기능(도메인) 중심 구조를 원해서 Feature-Sliced Design(FSD)을 검토함. FSD는 원래 `app/`, `pages/`를 최상위 레이어 이름으로 쓰는데, Next.js App Router는 `app/`을 라우팅 전용 예약 폴더로 쓰기 때문에 이름이 그대로 충돌함. 이 문제를 조정해서 적용함. (`jote-frontend` 이슈 [#5](https://github.com/daaoooy-lab/jote-frontend/issues/5) / PR [#6](https://github.com/daaoooy-lab/jote-frontend/pull/6))

<br/><br/>

## 결정: Next.js 맞춤 FSD

```
src/
 ├─ app/                    # Next.js 라우팅 전용 (layout.tsx, page.tsx, globals.css)
 ├─ widgets/                # 여러 feature를 조합한 UI 블록 (예: NoteEditorPanel, Sidebar)
 ├─ features/               # 독립된 기능 단위 (auth, editor, note-search, ai-chat)
 ├─ entities/               # 핵심 도메인 모델 (Note, User)
 └─ shared/                 # ui, api, lib, hooks, config, types (공용 리소스)
```

<br/>

### 조정 사항
- `app/` → `src/app/`로 이동 (Next.js가 지원하는 `src/` 디렉터리 방식). Next의 `app/`은 라우팅 전용으로만 쓰고, 실제 로직은 다른 레이어에 둠
- `tsconfig.json`의 `@/*` alias를 `./*`에서 `./src/*`로 변경
- FSD의 `pages` 레이어는 따로 안 만듦 — Next `app/`의 라우트 파일들이 여러 `widgets`/`features`를 조합하는 역할을 그대로 대신함
- FSD의 `processes` 레이어는 제외 — 최신 FSD 스펙에서도 사실상 폐기(deprecated)된 레이어로, 여러 페이지에 걸친 시나리오는 `features`나 페이지 레벨 조합으로 처리하라고 권장됨

<br/>

### 슬라이스 내부 구조
각 슬라이스는 FSD 표준 세그먼트 이름(`ui`/`model`/`api`/`lib`)을 쓰고, `index.ts`로 공개 API만 노출함 (다른 레이어는 슬라이스 내부 경로를 직접 참조하지 않고 `index.ts`를 통해서만 import).

```
features/
 └─ editor/
     ├─ ui/      # 에디터 컴포넌트, 툴바
     ├─ model/   # 상태, 훅
     ├─ api/     # 백엔드(NestJS) 호출
     └─ index.ts # 공개 API
```

<br/>

### 레이어 간 참조 규칙
`shared` ← `entities` ← `features` ← `widgets` ← `app` 순으로, 상위 레이어만 하위 레이어를 참조할 수 있고 그 반대는 불가능함 (예: `shared`는 `entities`/`features`를 import할 수 없음).

<br/><br/>

## 수정 히스토리

- 2026-08-01 13:41 — 최초 작성
