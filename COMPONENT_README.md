# 📘 컴포넌트 문서 작성 지침

## 1. 파일 위치 및 규칙

- 각 컴포넌트 폴더 안에 `README.md` 또는 `[ComponentName].mdx` 파일로 작성한다.
- 문서 파일명은 PascalCase를 따른다.
   예: `Button/README.md`, `Input/README.md`

------

## 2. 문서 구조

### ① 제목 & 한줄 설명

- 컴포넌트 이름
- 간단한 역할 설명

예시:

```
# Button
사용자가 액션을 수행할 수 있도록 돕는 기본적인 인터랙션 컴포넌트입니다.
```

------

### ② Import 방법

```
## Import

```tsx
import { Button } from "@/components/ui/button"
---

### ③ Props 표
- **Props 이름, 타입, 기본값, 설명**을 반드시 작성
- 공용 타입(`variant`, `size`)은 enum/union type 명시

예시:
```md
## Props

| Prop     | Type                        | Default    | Description               |
|----------|-----------------------------|------------|---------------------------|
| variant  | "primary" \| "secondary"    | "primary"  | 버튼 스타일               |
| size     | "sm" \| "md" \| "lg"        | "md"       | 버튼 크기                 |
| disabled | boolean                     | false      | 버튼 비활성화 여부        |
| onClick  | () => void                  | -          | 클릭 이벤트 핸들러        |
```

------

### ④ 기본 사용 예제

```
## Usage

```tsx
<Button variant="primary">확인</Button>
<Button variant="secondary">취소</Button>
---

### ⑤ 다양한 변형(Variants) & 상태(State)
- 색상, 크기, 아이콘 포함 여부 등
- hover, focus, disabled 등 상태 표현

```md
## Variants

```tsx
<Button variant="primary">Primary</Button>
<Button variant="secondary">Secondary</Button>
<Button variant="danger">Danger</Button>
---

### ⑥ 접근성 (Accessibility, a11y)
- 스크린 리더 지원 여부
- 키보드 포커스 가능 여부
- ARIA 속성 사용 시 설명

예시:
```md
## 접근성

- 기본적으로 `<button>` 태그를 사용합니다.
- `disabled` 속성이 true일 경우 `aria-disabled="true"`가 적용됩니다.
- 키보드로 `Enter`, `Space` 키 입력 시 클릭 이벤트가 실행됩니다.
```

------

### ⑦ Best Practices / 주의사항

- 어떤 상황에서 쓰면 좋은지
- 사용을 지양해야 하는 케이스

예시:

```
## Best Practices

✅ Submit 버튼은 항상 `type="submit"`으로 명시한다.  
❌ 텍스트 링크를 버튼으로 쓰지 않는다. (대신 `<a>` 태그 사용)
```

------

## 3. 선택 사항

- **Storybook 연동** 시 `.stories.tsx` 파일과 연결
- **디자인 시스템**일 경우 Figma 링크 추가
- 팀별 코드 컨벤션/스타일 가이드 반영