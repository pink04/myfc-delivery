# MyFC 네이밍 규칙

이 문서는 현재 프로젝트에서 클래스 네이밍을 일관되게 유지하기 위한 기준이다.

## 1) 기본 원칙

- 페이지/도메인 클래스는 `kebab-case`를 사용한다.
  - 예: `profile-menu`, `situation-banner-top`, `search-results-title`
- 상태 클래스는 `is-*`를 사용한다.
  - 예: `is-active`, `is-open`, `is-selected`
- 공용 라이브러리 클래스는 유지한다.
  - 예: `swiper`, `swiper-wrapper`, `swiper-slide`

## 2) 공용 컴포넌트

- 공용 UI 컴포넌트도 동일하게 `kebab-case`를 사용한다.
  - `c-modal-*`
  - `c-modal-option-*`
- 폼 하단 버튼 묶음: **`form-actions`**(구분선·주 CTA 전폭), 필요 시 **`form-actions-sub`**(보조 버튼 한 행), 2열(보조+주요)은 **`form-actions form-actions-split`**. (`common.css`)
- 디자인 시스템 클래스 변경 시 HTML/CSS/JS를 반드시 함께 동기화한다.

## 3) HTML `id` 지양

- 전역에서 유일해야 하는 `id` 속성은 **남발하지 않는다.** 스타일 훅·앵커·`label for` 연결 등 때문에 생기는 이름 충돌·유지보수 비용을 줄인다.
- DOM 탐색·스크립트 바인딩은 **`data-*`**, **구조 선택자**, **`class`(스코프 한정)** 를 우선한다.
- 접근성: **`fieldset`/`legend`**, 인접한 **보이는 제목·설명**, 네이티브 **`label`/`for`**(필요할 때만 id) 등으로 `id` 없이 표현할 수 있는지 먼저 검토한다.
- 예외: 서드파티 위젯·인라인 스킵 링크 등 **명세/외부 모듈이 `id`를 요구하는 경우**에만 최소 범위로 사용한다.

## 3-1) `aria-labelledby` · `aria-label` (범위 고정)

**신규 마크업·수정 시 아래를 기본으로 따른다.** (섹션/모달/폼에 임의로 이름 속성을 붙이지 않는다.)

- **`aria-labelledby`**: **`button`**, **`a`** 에만 둔다.  
  `section`, `article`, `nav`, `div`, `div[role="dialog"]`, `input`, `textarea` 등 **그 외 요소에는 두지 않는다.**
- **`aria-label`** (접근용 이름): **`button`**, **`a`** 에만 둔다.  
  `input`, `textarea`, `nav`, `section`, `div`, `p` 등에는 두지 않는다. 보이는 제목·`label`·`placeholder`·`fieldset`/`legend` 등으로 맞춘다.
- **`aria-hidden`**, **`aria-pressed`**, **`aria-expanded`**, **`aria-current`**, **`aria-modal`** 등 **상태·장식** 속성은 위 제한과 별개로, 필요 시 해당 요소에 둘 수 있다.
- 동적 스크립트로 이름을 붙일 때도 **대상이 `button` 또는 `a`인 경우에만** `aria-label` / `aria-labelledby`를 사용한다.

## 4) 금지/지양 (클래스)

- 페이지 클래스에서 `__`와 `-`를 혼용하지 않는다.
  - 페이지/도메인 영역은 `-`만 사용
- 의미 없는 래퍼 클래스 추가를 지양한다.
  - 부모 스코프로 해결 가능한 경우 자식 클래스를 추가하지 않는다.
- 같은 역할에 다른 접두를 혼용하지 않는다.
  - 예: `consult-*`와 `language-*` 동시 사용 금지

## 5) 권장 패턴

- 블록: `block-name`
- 요소(페이지 영역): `block-name-part`
- 상태: `is-*`

예시:

- `profile-menu`, `profile-menu-summary`, `profile-menu-drawer`
- `situation-banner`, `situation-banner-list`, `situation-banner-title`
- `search-results`, `search-results-head`, `search-results-title`

## 6) 변경 시 체크리스트

1. HTML 클래스 변경
2. 연결 CSS 선택자 동기화
3. JS 선택자(`querySelector`, `closest`) 동기화
4. 잔여 구명칭 전역 검색(`rg`)
5. 린트/기본 렌더 확인

## 7) 최근 정리된 주요 계열

- `profile-menu-*`
- `recommend-*`
- `situation-banner-*`
- `situation-list-carousel`
- `language-*`
- `review-*`
- `securities-*`
- `favorite-*`
- `search-results-*`
- `story-detail-*`

