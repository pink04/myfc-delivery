# MYFC 사용자 앱 · 공통 구조 가이드

현재 완성된 HTML/CSS/JS 기준으로, 화면 간 **재사용되는 레이어·마크업 패턴·운영 규칙**을 정리한 산출물입니다.

---

## 1. 폴더 구조

| 경로 | 역할 |
|------|------|
| `html/` | 화면별 정적 HTML (`MFC001.html` 등) |
| `assets/css/` | 전역 스타일시트 (아래 로드 순서 준수) |
| `assets/js/` | 전역 스크립트 (`MyFC` 네임스페이스) |
| `assets/fonts/` | Pretendard 등 웹폰트 |
| `assets/images/` | 아이콘·로고·임시 이미지 |
| `guide/data.js` | IA 시트 연동용 화면 메타데이터 (`TABLE_DATE`) |
| `guide/list.js` | `index.html` 목록 렌더링 |
| `index.html` | 관리용 화면목록 (표) |

---

## 2. CSS 로드 순서 (표준)

대부분의 서브 페이지는 아래 **4종**을 같은 순서로 링크합니다.

```html
<link rel="stylesheet" href="../assets/css/reset.css" />
<link rel="stylesheet" href="../assets/css/common.css" />
<link rel="stylesheet" href="../assets/css/layout.css" />
<link rel="stylesheet" href="../assets/css/content.css" />
```

- **메인(`MFC001.html`)** 등 일부는 Swiper·`main.css`가 추가됩니다.
- **역할 분리**
  - `reset.css` — 기본 리셋
  - `common.css` — 디자인 토큰(`:root`), 폰트, `.form-box`/`.form-field` 등 폼 공통, 버튼·체크·라디오 베이스
  - `layout.css` — `.site-header`, `.site-footer`, `.page-inner`, 그리드 뼈대
  - `content.css` — 페이지 유형별 블록(M 검색, FC 상세, 회원가입 흐름 `.signup-page` 등)

새 화면 추가 시 **동일 순서**를 유지하면 스펙이 깨지지 않습니다.

---

## 3. 디자인 토큰 (`common.css` `:root`)

공통으로 참조되는 변수 예시입니다. 컴포넌트 스타일은 이 값들을 우선 사용합니다.

| 구분 | 변수 예시 |
|------|-----------|
| 텍스트 | `--text`, `--text-sub`, `--text-muted`, `--text-placeholder` |
| 테두리 | `--border`, `--border-strong`, `--focus-border` |
| 배경 | `--bg`, `--bg-sub`, `--bg-elevated` |
| 브랜드 | `--primary`, `--primary-hover`, `--primary-active` |
| 상태 | `--danger`, `--success` |

본문 폰트 스택: **`Pretendard`** 우선 (woff2 `@font-face` 다중 굵기).

---

## 4. 페이지 래퍼·레이아웃 타입

### 4.1 최상위 래퍼

```html
<div class="layout-wrapper …">
```

자주 쓰이는 조합:

| 클래스 조합 | 용도 |
|-------------|------|
| `layout-wrapper main-page` | 메인 홈 (`MFC001`) |
| `layout-wrapper form-page signup-page` | FC 회원가입(`MFC008*`), FC 회원정보/프로필 관리(`MFC005_L01_01*`) 등 단계형 폼 |

`signup-page`는 **`content.css`**에서 단계 탭(`.c-tabs`), 단계 리드(`.signup-step-lead`), 프로필 썸네일 등 회원가입 전용 간격·테두리를 한꺼번에 적용합니다.

### 4.2 메인 컬럼 폭

| 클래스 | max-width (개요) |
|--------|------------------|
| `.page-inner` | 1024px — 목록·넓은 콘텐츠 |
| `.page-inner-narrow` | 500px — 로그인·회원가입·설정 등 단일 폼 |

검색 등 일부 페이지는 `content.css`에서 섹션별로 폭을 한 번 더 좁히거나 넓힙니다 (예: 검색 폼 560px).

---

## 5. 헤더 (`layout.css` + `common.css`)

### 5.1 공통 구조

```html
<header class="site-header guest|member">
  <div class="header-mo">…</div>   <!-- 모바일 전용: 뒤로가기 등 — 해당 페이지만 -->
  <div class="header-inner">
    <div class="logo"><a href="MFC001.html">…</a></div>
    <nav class="gnb"><ul>…</ul></nav>
  </div>
</header>
```

| 클래스 | 의미 |
|--------|------|
| `.site-header.guest` | 비로그인용 GNB(회원가입·로그인 버튼 표시 등) |
| `.site-header.member` | 로그인 후(아바타·알림 등) |

GNB 아이콘·버튼 클래스 예: `.gnb-search`, `.gnb-bell`, `.gnb-btn-line`, `.gnb-avatar`.

메인 등에서는 **`data-profile-toggle` / `data-profile-menu` / `data-profile-drawer`** 로 프로필 드롭다운·모바일 드로어가 연결되며, 초기화는 **`assets/js/common.js`** 의 `initProfileMenu()` 에서 처리합니다.

---

## 6. 푸터 (공통 블록)

완성 페이지에서 반복되는 구조입니다.

```html
<footer class="site-footer">
  <div class="footer-inner">
    <div class="footer-top">… footer-logo, footer-links …</div>
    <div class="footer-info">…</div>
    <p class="copyright">…</p>
  </div>
</footer>
```

스타일은 **`layout.css`** 의 `.site-footer` 계열을 따릅니다.

---

## 7. 폼 공통 패턴 (`common.css` + `content.css`)

### 7.1 폼 래퍼

```html
<form class="form-box" method="post">
```

### 7.2 필드 행

```html
<div class="form-field">
  <label class="form-label" for="…">레이블</label>
  <input class="form-input" id="…" />
</div>
```

- 라벨과 입력을 한 줄에 두고 라디오만 오른쪽에 두는 경우: **`form-field form-field--label-inline`** + `span.form-label` (마케팅 동의 등).

### 7.3 입력 + 부가 버튼

```html
<div class="combo">
  <input class="form-input" … />
  <button type="button">재설정</button>
</div>
```

### 7.4 하단 액션

단일 제출:

```html
<div class="form-actions">
  <button type="submit" class="btn btn-primary">…</button>
</div>
```

2열 스플릿(보조 링크 + 주 버튼) — 마이페이지·회원가입 단계(이전/다음) 등 **같은 마크업·같은 스타일**이며, 라벨·`href`·버튼 `type`만 화면에 맞게 바꿉니다.

```html
<div class="form-actions form-actions-split">
  <a href="…" class="btn">보조(예: 이전 · 내 프로필로)</a>
  <button type="submit" class="btn btn-primary">주요 액션(예: 다음 · 수정 완료)</button>
</div>
```

### 7.5 단계형 탭 (FC 회원가입·회원정보)

```html
<nav class="c-tabs">
  <a href="…" aria-current="page">현재 단계</a>
  <a href="…">…</a>
</nav>
```

현재 단계는 **`aria-current="page"`** 로 표시합니다. 스타일은 **`.signup-page .c-tabs`** (`content.css`)에서 처리합니다.

### 7.6 단계 설명 문구

```html
<p class="signup-step-lead">…</p>
```

### 7.7 약관 블록

```html
<div class="signup-agree">
  <label class="all c-check">…</label>
  …
</div>
```

체크·라디오 베이스: `.c-check`, `.c-radio` (`common.css`).

---

## 8. 모달·바텀시트 (`popup.js`)

중앙 모달 마크업 패턴과 데이터 속성은 **`assets/js/popup.js`** 상단 주석에 정리되어 있습니다.

요약:

| 훅 | 역할 |
|----|------|
| `data-popup-target="#팝업id"` | 해당 `#id` 모달 열기 |
| `data-popup-sync="#hidden"` | 선택 값을 hidden 입력 등과 동기화 |
| `data-popup-close` | 닫기 |
| `data-popup-confirm` | 확인 |
| `.c-modal-option[data-value="…"]` | 옵션 행 |
| 팝업 루트에 `data-popup-multiselect` | 복수 선택 모달 |

토스트 등은 같은 파일의 `MyFC` API를 통해 동작합니다.

---

## 9. JavaScript 로드 순서 (일반 서브 페이지)

```html
<script src="../assets/js/ui.js"></script>
<script src="../assets/js/popup.js"></script>
<script src="../assets/js/common.js"></script>
```

| 파일 | 역할 |
|------|------|
| `ui.js` | `window.MyFC` — Swiper 초기화, 칩 드래그 등 UI 헬퍼 |
| `popup.js` | 모달·토스트 |
| `common.js` | 프로필 메뉴 등 부트스트랩 |

페이지 전용 스크립트는 위 세 파일 **아래**에 인라인 또는 별도 블록으로 둡니다.

---

## 10. 화면 메타데이터 · 목록 (`guide/data.js`, `index.html`)

- **`guide/data.js`** 의 `TABLE_DATE` 객체에 화면 단위 row를 둡니다.
- 필드: `no`, `id`, `type`, `d1`~`d5` (최대 5 depth), `path`, `status`, `note` 등.
- **`guide/list.js`** 가 데이터를 평탄화·정렬해 `index.html` 의 `#listBody` 테이블을 채웁니다.
- depth 중복 행은 목록에서 빈 칸으로 접히도록 렌더링됩니다 (`prefixSame` 로직).

신규 화면 추가 시 **HTML 파일 생성 + `data.js` 행 추가**가 세트입니다.

---

## 11. 파일·ID 네이밍 (관례)

| 패턴 | 예시 | 설명 |
|------|------|------|
| 접두어 `MFC` | `MFC005_L01_01.html` | 사용자 앱 화면 ID와 파일명 동기 |
| 언더스코어 depth | `MFC005_L01`, `MFC005_L01_01` | IA 단계에 따른 분기 |

랜딩·운영 페이지와 구분이 필요하면 프로젝트 내 기존 규칙(예: `AMFC*`)을 따릅니다.

---

## 12. 신규 화면 체크리스트

1. `html/` 에 파일 추가, `<title>` 및 본문 제목 일치 검토  
2. CSS 4종 링크 순서 유지 (메인 예외 시 `main.css` 등만 추가)  
3. 레이아웃: `layout-wrapper` + 페이지 타입 클래스 (`form-page`/`signup-page` 등)  
4. 헤더: `guest` / `member` 및 모바일 `header-mo` 필요 여부  
5. 폼: `.form-box` → `.form-field` / `.combo` / `.form-actions`  
6. 모달 사용 시 `popup.js` 훅 준수  
7. 스크립트: `ui.js` → `popup.js` → `common.js` 순  
8. `guide/data.js` 에 행 추가 후 `index.html` 에서 경로·depth 표시 확인  

---

## 13. 참고 소스 위치 (빠른 점프)

| 내용 | 파일 |
|------|------|
| 색·타이포·폼 베이스 | `assets/css/common.css` |
| 헤더·푸터·`.page-inner` | `assets/css/layout.css` |
| 회원가입/회원정보 단계 UI | `assets/css/content.css` (`.signup-page`, `.c-tabs` 검색) |
| 모달 동작 상세 | `assets/js/popup.js` 주석 |
| 프로필 메뉴 | `assets/js/common.js` |
| IA 데이터 | `guide/data.js` |

---

*본 문서는 저장소 현재 상태를 기준으로 작성되었으며, 신규 패턴 추가 시 이 파일을 함께 갱신하는 것을 권장합니다.*
