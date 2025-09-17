## YoonCarrot Monorepo AI 작업 지침서

본 문서는 이 레포 내에서 AI가 코드를 생성·수정·리팩터링·문서화할 때 따라야 할 공통 규칙과 각 워크스페이스(웹/모바일/패키지)별 컨벤션을 정의합니다. 명시되지 않은 사항은 기존 코드 스타일을 따릅니다.

### 1) 모노레포 구조 개요

- 루트: `yarn` + `turbo` 기반 모노레포. 작업은 기본적으로 패키지 경계 내에서 수행합니다.
- 워크스페이스: `apps/*`, `packages/*`
  - `apps/carrot-blog-front`: Next.js 15 웹 앱
  - `apps/carrotMobileApp`: React Native 앱
  - `packages/design-system`: 공용 UI 컴포넌트 라이브러리
  - `packages/editor`: Tiptap 기반 에디터 라이브러리
  - `packages/domain`: API 클라이언트, 엔티티, DTO, 유틸
  - `packages/utils`: 범용 훅/유틸리티
  - `packages/eslint-config`, `packages/typescript-config`: 내부 설정 패키지

### 2) 러닝(실행) 규칙

- 루트 스크립트(권장):
  - 개발: `pnpm dev` (필요 시 `pnpm dev:web`, `pnpm dev:story`)
  - 빌드: `pnpm build` 또는 특정 앱만 `pnpm build:web`
  - 시작: `pnpm start` 또는 `pnpm start:web`
  - 린트/포맷: `pnpm lint`, `pnpm format`
- 앱 개별:
  - 웹: `pnpm -C apps/carrot-blog-front dev|build|start|lint|test`
  - 모바일: `pnpm -C apps/carrotMobileApp android|ios|start|lint`
- Node/PNPM 버전 준수: 루트 `package.json`의 `engines.node`와 `packageManager` 버전을 사용.

### 3) 코드 작성 원칙

- 언어/런타임:
  - TypeScript 우선. `type: module` 환경을 존중.
  - React 19 / Next 15 기준 API 사용. Server/Client 컴포넌트 경계 준수.
- 명명/가독성:
  - 의미 있는 전체 단어 사용, 축약 지양. 함수는 동사, 값은 명사.
  - 가드 절/조기 반환 선호. 불필요한 중첩 금지.
- 예외/에러 처리:
  - 의미 있는 메시지와 처리. 빈 `catch` 금지.
  - 도메인 계층(`packages/domain`)의 에러 타입을 우선 사용.
- 사이드 이펙트/의존성:
  - 앱 레벨에서 `@tanstack/react-query`, 상태는 `zustand`를 따름.
  - 공통 로직은 패키지(`domain`, `utils`)로 분리 후 앱에서 사용.

### 4) 디렉터리/계층 경계

- `apps/*`는 화면/라우팅/주입 조립, 비즈니스 로직 최소화.
- `packages/domain`은 API/엔티티/DTO/도메인 유틸의 소스. 외부 통신과 데이터 모델은 여기서만 정의/수정.
- `packages/design-system`은 UI 컴포넌트만. 도메인 지식/네트워크 로직 금지.
- `packages/editor`는 에디터 관련 로직과 UI. 외부 의존 최소화, 스타일은 패키지 내부에서 완결.
- `packages/utils`는 범용 훅/함수. React 의존이 필요한 훅은 peerDependencies를 유지.

### 5) 의존성 규칙

- 앱 → 패키지 방향(`apps`는 `packages`를 참조), 역참조 금지.
- 패키지 간 참조는 안정성 높은 계층만 허용:
  - `design-system`/`editor`/`utils`는 서로 독립을 기본으로, 도메인 참조 금지.
  - `domain`은 다른 패키지를 참조하지 않음(순수).
- 내보내기 경로는 각 패키지 `exports`에 등록 후 사용. 상대경로 import 금지.

### 6) 스타일/린트/TS 설정

- 린트: 루트 `pnpm lint`로 무경고 원칙(`--max-warnings 0`).
- 포맷: `prettier` + 정렬 플러그인. import 정렬을 유지.
- 타입스크립트: `packages/typescript-config`를 참조. any/강제 캐스팅 지양.

### 7) 테스트/커버리지

- 웹앱: Jest + Testing Library. `pnpm -C apps/carrot-blog-front test(:watch|:coverage)`
- 모바일앱: 현재 RN 테스트 비활성. e2e 도입 시 별도 문서화.
- 터보 캐시: 테스트 커버리지는 `coverage/**` 산출물.

### 8) 빌드/배포

- 터보 파이프라인을 따른다. 각 패키지는 `build` 정의 시 상위 의존 `^build`가 먼저 실행됨.
- Next 빌드 출력은 `.next/**` (캐시 제외). 환경변수는 `.env*`를 입력으로 캐시 키에 포함.
- 스토리북 별도 앱이 있을 경우 해당 워크스페이스 내에서 관리.

### 9) 환경변수/보안

- `.env*` 파일 사용. 민감정보는 코드 저장소에 커밋 금지.
- 클라이언트 주입이 필요한 키는 Next의 공개 프리픽스 규칙을 준수.

### 10) 커밋/브랜치 전략

- 기본 브랜치: `develop` 기준. 기능은 feature 브랜치에서 작업 후 PR.
- 커밋 메시지: 일관된 형식 사용(예: Conventional Commits 권장). 레포의 git hooks가 있다면 준수.

### 11) 변경 작업 가이드(에이전트용 체크리스트)

1. 작업 범위 결정: 앱/패키지/파일 목록과 목적 정의
2. 관련 패키지 의존/exports 점검 및 필요시 먼저 정리
3. 코드 작성: 기존 스타일/레이아웃 유지, 불필요한 리포맷 금지
4. 린트/타입체크: `pnpm lint` 통과 확인
5. 단위테스트(해당 시) 실행
6. 문서화: 공개 API, 사용 방법, 예제 추가 또는 갱신
7. 변경 요약: 영향 범위, 마이그레이션 포인트 기록

### 12) 앱별 특이사항

- `apps/carrot-blog-front`:
  - Next App Router 사용. 서버/클라이언트 컴포넌트 구분 엄격.
  - 전역 스타일은 `src/app/globals.css`(imports `#globals`). Tailwind v4 구성 유지.
  - 분석/모니터링: Sentry, GA 연동 파일 존재. 비활성화/키값 주의.
- `apps/carrotMobileApp`:
  - RN 0.80/Hermes 기준. 플랫폼별 설정은 각 폴더에서 관리.
  - 개발 시 `adb reverse` 스크립트 자동 포함. 에뮬레이터 포트 충돌 주의.

### 13) 금지/주의 사항

- 패키지 경계 침범(앱에서 도메인 모델 수정 등) 금지.
- 내부 경로로 타 패키지 소스 직접 import 금지. 오직 `exports`를 통해 사용.
- 불필요한 대규모 리포맷/정렬 변경 금지. 변경 최소화 원칙.
- 에러를 무시하거나 콘솔만 출력하고 종료하는 패턴 금지.

### 14) 빠른 레시피

- 새 도메인 API 추가: `packages/domain/src/api`에 구현 → `exports` 등록 → 웹앱에서 사용.
- 공통 UI 컴포넌트 추가: `packages/design-system/src/components`에 구현 → `exports` 등록.
- 에디터 기능 확장: `packages/editor/src` 내부에 기능 추가 → 외부 API 노출 시 `exports` 등록.
- 공통 훅 추가: `packages/utils/src`에 구현 → `exports` 등록 → peerDependencies 점검.

### 15) 명령어 요약

```bash
# 루트
pnpm install
pnpm dev           # 전체 개발 모드
pnpm dev:web       # 웹만 개발 모드
pnpm build         # 전체 빌드
pnpm lint          # 전 워크스페이스 린트

# 웹앱 전용
pnpm -C apps/carrot-blog-front dev
pnpm -C apps/carrot-blog-front test:coverage

# RN 앱 전용
pnpm -C apps/carrotMobileApp android
pnpm -C apps/carrotMobileApp ios
```

부족하거나 바뀐 규칙이 있으면 본 문서를 업데이트합니다.
