
# AI-Agent-Setting-Template

AI-Agent-Setting-Template는 TDD와 Tidy First 원칙을 실천하는 AI 지침서 템플릿입니다. 다양한 프론트엔드 앱과 UI 컴포넌트, 개발 가이드, 아키텍처 문서를 포함하며, AI 기반 개발 자동화 실험을 통해 생산성을 올리는 것이 목표입니다.

## 주요 폴더 구조

- `apps/` : 실제 서비스 앱(`web`, `docs` 등)
- `packages/ui/` : 재사용 가능한 React UI 컴포넌트
- `packages/eslint-config/` : 조직 공통 ESLint 설정
- `packages/typescript-config/` : 조직 공통 TypeScript 설정

## 개발 원칙

- 모든 기능은 테스트 주도 개발(TDD)로 구현
- 구조적 변경과 행위적 변경을 분리(Tidy First)
- 작은 단위로 자주 커밋, 커밋 메시지에 변경 유형 명확히 표기

## 실행 방법

```bash
# 의존성 설치
pnpm install

# 전체 빌드
pnpm build

# 앱 개발 서버 실행 (예: web)
pnpm --filter web dev
```

## 문서

- `AGENTS.md` : TDD/Tidy First 개발 가이드
- `ARCHITECTURE.md` : 시스템 아키텍처 개요
- 각 패키지/컴포넌트별 README 참고

## 기여 가이드

- plan.md의 워크플로우를 따라 기능을 추가/수정하세요.
- 새로운 기능은 반드시 실패하는 테스트부터 작성하세요.
- 구조적 변경과 행위적 변경을 분리하여 커밋하세요.

---

본 프로젝트는 Kent Beck의 TDD와 Tidy First 철학을 실제 코드와 협업에 적용하는 실험적 레퍼런스입니다.
