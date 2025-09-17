## 코딩 가이드 (ESLint/Prettier/TypeScript 표준)

이 문서는 모노레포 전반의 코딩 스타일, 네이밍 규칙, 린트/포맷/타입 규칙을 정리합니다. 각 워크스페이스는 내부 `eslint.config.js`와 `packages/eslint-config`를 통해 일관된 기준을 공유합니다.

### 1) 언어/런타임 기본

- **TypeScript 우선**. `type: module`(ESM) 환경을 준수합니다.
- **엄격 모드**: `strict: true`, `noUnusedParameters: true`, `noUnusedLocals: true`.
- **모듈 해석**: `module: esnext`, `moduleResolution: bundler`.
- **JSX**:
  - Next 앱: `jsx: preserve` (Next 플러그인 사용)
  - 라이브러리/일반 React: `jsx: react-jsx`, `jsxImportSource: react`

### 2) ESLint 규칙 요약

- 공통 베이스: `@eslint/js` recommended + `typescript-eslint` recommended + `eslint-config-prettier`.
- **사용 금지/완화**:
  - `no-unused-vars`: off (대신 `@typescript-eslint/no-unused-vars: warn`)
  - `_`로 시작하는 변수/파라미터는 미사용 허용(`^_` 패턴)
  - Turbo: `turbo/no-undeclared-env-vars` off
- **React/Next**:
  - React Hooks 규칙 준수(`react-hooks` recommended)
  - `react/react-in-jsx-scope`: off (새 JSX 트랜스폼)
  - Next: `@next/next` recommended + `core-web-vitals` 적용, `no-page-custom-font`: off
- **React Native** (모바일 앱):
  - RN 공식 설정 기반 + `react-native/no-inline-styles`: off
  - JS 파일은 `hermes-eslint`, TS/TSX는 `@typescript-eslint/parser`

### 3) Prettier 스타일

- 루트 `prettier` 사용. 정렬 플러그인으로 import/속성/스타일 정렬 일관성 유지.
- 기본 원칙:
  - 포맷은 자동화에 맡기고 수동 정렬 지양
  - 의미 없는 대규모 리포맷 금지(필요한 변경만)

### 4) 네이밍 규칙

- **파일/폴더**:
  - React 컴포넌트 파일: `PascalCase.tsx` (예: `HomeIntroduction.tsx`)
  - 유틸/훅/일반 함수: `camelCase.ts` (예: `useUserAwareness.ts`)
  - 타입/엔티티/DTO 인덱스: `index.ts`로 집약(export 전용)
- **코드 심볼**:
  - 클래스/컴포넌트: PascalCase
  - 함수/변수/훅: camelCase (`use` 접두사는 React 훅 전용)
  - 타입/인터페이스/Enum: PascalCase (`User`, `ArticleDto`, `UserRole`)
  - 상수: UPPER_SNAKE_CASE (필요 시만. 일반 상수는 camelCase)
  - 이벤트/핸들러: `onX`, `handleX` 페어
  - 비동기 함수: 동사 + 목적어 + `Async`
- **테스트**: 대상 파일명 + `.test.ts(x)` (웹앱은 Jest/RTL 기준)

### 5) import 규칙

- **내부 패키지 참조**: 반드시 각 패키지의 `exports` 경로 사용. 상대경로로 타 패키지 소스 접근 금지.
- **정렬**: Prettier import 정렬 플러그인 규칙에 따름(알파벳/그룹 기준).
- **CSS**: Next/Design System는 `#globals` 또는 패키지 export를 통해 import.

### 6) React/Next 작성 규칙

- Server/Client 컴포넌트 경계를 명확히. 클라이언트 전용 훅/상태는 Client 컴포넌트에서만.
- 상태관리: 앱에서는 `@tanstack/react-query`, `zustand` 사용 원칙.
- 이벤트 핸들러는 **짧고 순수**하게, 무거운 로직은 훅/유틸로 분리.
- 컴포넌트 props는 명확한 타입을 부여하고, 불필요한 `any` 지양.

### 7) 오류/예외 처리

- 빈 `catch` 금지. 의미 있는 메시지/로깅/사용자 피드백 제공.
- 가능한 `packages/domain`의 에러 타입을 사용해 계층 간 일관성 유지.
- 네트워크/API는 `packages/domain`에만 정의. 앱에서는 호출/바인딩만 수행.

### 8) 주석/문서화

- 자명한 코드 주석 금지. 필요한 경우 "왜"를 설명.
- 공개 API(패키지 `exports`)의 사용 예시는 각 패키지 README 또는 스토리/테스트로 제공.

### 9) 성능/접근성 체크

- React re-render 최소화: props 안정성, memoization은 필요 시에만.
- 리스트/표 렌더링 시 key 안정성 보장.
- 접근성 속성(aria-\*)과 시맨틱 태그 우선.

### 10) 커밋/브랜치 워크플로

- 기본 브랜치: `develop`. 기능은 feature/\* 분기 → PR.
- 커밋: 의미 있는 단위로, Conventional Commits 권장.
- 레포의 git hooks가 있을 경우 이를 준수.

### 11) 린트/테스트 실행

```bash
pnpm lint                # 전 워크스페이스 린트 (무경고 원칙)
pnpm -C apps/carrot-blog-front test
pnpm -C apps/carrot-blog-front test:coverage
```

### 12) 타입스크립트 옵션 참고(공유 베이스)

- `strict: true`, `forceConsistentCasingInFileNames: true`
- `esModuleInterop: true`, `resolveJsonModule: true`
- `noEmit: true` (앱), 라이브러리는 `declaration`/`declarationMap`로 타입 배포
- `allowImportingTsExtensions: true` (확장자 명시 허용)

필요 시 규칙이 바뀌면 본 문서를 업데이트합니다.
