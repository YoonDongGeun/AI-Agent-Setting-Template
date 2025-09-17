# Button 컴포넌트

TDD와 Tidy First 원칙에 따라 개발된 재사용 가능한 Button 컴포넌트입니다.

## 위치
- 코드: `packages/ui/src/Button/Button.tsx`
- 테스트: `packages/ui/src/Button/Button.test.tsx`
- 계획: `packages/ui/src/Button/plan.md`

## 주요 기능
- 기본 버튼 렌더링
- children 렌더링
- onClick 동작
- disabled 상태 지원
- className 및 추가 props 전달

## 사용 예시
```tsx
import { Button } from 'ui/Button';

<Button onClick={() => alert('clicked!')}>Click me</Button>
<Button disabled>Disabled</Button>
<Button className="my-btn">Custom Style</Button>
```

## 개발 원칙
- 모든 기능은 테스트 주도 개발(TDD)로 구현
- 구조적 변경과 행위적 변경을 분리(Tidy First)
- 작은 단위로 자주 커밋, 커밋 메시지에 변경 유형 명확히 표기

## 테스트 실행
```bash
# 패키지 루트에서
pnpm test
```

## 기여 가이드
- plan.md의 워크플로우를 따라 기능을 추가/수정하세요.
- 새로운 기능은 반드시 실패하는 테스트부터 작성하세요.
- 구조적 변경과 행위적 변경을 분리하여 커밋하세요.
