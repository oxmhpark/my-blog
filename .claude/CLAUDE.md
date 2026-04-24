# 챗봇용 저장소 취급 가이드라인

이 저장소는 깃허브 페이지스를 이용한 지킬 기반 개인 블로그의 서비스 소스이다. 프로젝트에 대한 메타 정보와 운영 목적·방침은 [README.md](/README.md) 문서를 참조할 것.

## 제한
- `/README.md` 파일을 직접 수정하지 말 것.

## 빌드 환경
- 빌드는 전적으로 깃허브 페이지스가 수행한다. 로컬 빌드 도구(`Gemfile`, `node_modules`, `_site/`) 를 새로 추가하지 말 것.
- 빌드 검증 수단이 없으므로, 템플릿 수정 시 문법 오류·미설정 변수를 특히 주의할 것.
- `jekyll-sitemap`·`jekyll-feed`·`jekyll-seo-tag` 플러그인은 사용하지 않는다. [/sitemap.xml](/sitemap.xml)과 [/feed.xml](/feed.xml)은 수작업 템플릿이다.
- [/.drafts/](/.drafts/) · [/.audits/](/.audits/) 폴더는 `.gitignore`에 등록된 로컬 전용이다.
- [`_config.yml.sample`](/_config.yml.sample) 은 현재 저장소 구조(콜렉션·`defaults`·플러그인 비활성)와 동기화돼 있지 않다. 참고 자료로 읽지 말고, 운영자가 재작성을 지시하지 않는 한 건드리지 말 것.

## 템플릿 구조

### 레이아웃
- [`_layouts/`](/_layouts/) 의 모든 레이아웃은 `base-header.html` → 본문 → `base-footer.html` 패턴을 따른다. 이 껍데기를 바꾸지 않는 한 `<head>`·`<header>`·`<footer>` 구조는 일관된다.
- 레이아웃은 용도에 따라 세 부류로 나뉜다.
  - 엔트리용: [`entry-page.html`](/_layouts/entry-page.html), [`entry-post.html`](/_layouts/entry-post.html) — 본문을 `post-format-fulltext`로 출력하고 `component-comments`를 붙인다(`entry-page`만 조건부).
  - 아카이브용: [`archive-project.html`](/_layouts/archive-project.html), [`archive-event.html`](/_layouts/archive-event.html), [`archive-year.html`](/_layouts/archive-year.html), [`archive-type.html`](/_layouts/archive-type.html) — 아카이브 본문을 먼저 렌더한 뒤 `site.entry_posts` 콜렉션을 필터·정렬해 `list-posts`로 나열한다.
  - 기타: [`error.html`](/_layouts/error.html)([/404.md](/404.md) 전용), [`raw.html`](/_layouts/raw.html)(콘텐츠만 그대로 출력).
- 각 레이아웃은 `body_class`를 지정해 `<body>`의 CSS 훅을 만든다. 새 레이아웃을 추가할 때 이 관례를 따를 것.

### 아카이브 레이아웃의 매칭 규칙
- `archive-project.html`·`archive-event.html`: `archiveName = post.title`. 즉 분류 페이지의 `title` 프론트매터 값과 포스트의 `projects`·`events` 항목 문자열이 정확히 일치해야 한다.
- `archive-year.html`: `archiveName = post.title | to_i`. 연도 페이지 `title`은 숫자여야 한다.
- `archive-type.html`: `site["entry_" + page.slug]` 로 콜렉션을 뽑는다. 현재 엔트리 콜렉션이 `entry_posts` 하나로 통합돼 있어 실질적으로 `slug: posts`([`/_entry_pages/posts.md`](/_entry_pages/posts.md))만 의미가 있다. 장래에 엔트리 콜렉션을 추가하려면 새 아카이브 페이지의 `slug` 프론트매터가 콜렉션 접미사와 일치하도록 설정할 것.

### 인클루드
- [`_includes/`](/_includes/) 는 네이밍으로 역할이 드러난다: `base-*`(HTML 뼈대), `layout-*`(영역 단위), `component-*`(독립 위젯), `parts-*`(원자 단위), `list-posts*`(반복 출력), `post-format-*`(엔트리 출력 양식).
- 대부분의 인클루드는 `post` 변수를 기대한다. 레이아웃은 `{% assign post = page %}` 로 현재 페이지를 주입한 뒤 인클루드를 호출한다.
- [`parts-metadata.html`](/_includes/parts-metadata.html) 은 `post.meta` 배열에 특정 키(`refs`·`queries`·`date`·`lastmod`·`years`·`projects`·`events`·`links`)가 포함된 경우에만 해당 블록을 렌더한다. 이 배열은 `_config.yml` 의 `defaults` 에서 콜렉션별로 주입된다.
- [`parts-url.html`](/_includes/parts-url.html) 은 `bookmark: true` 이고 `refs` 가 있으면 `refs[0]` 로, 아니면 `post.url` 로 링크한다(외부 북마크 특수 처리).

### 콜렉션과 `defaults`
- [`_config.yml`](/_config.yml) 의 `collections` 가 `entry_pages`·`entry_posts`·`archive_projects`·`archive_events`·`archive_years` 를 정의한다. `permalink` 규칙이 여기 고정돼 있다(`project/:slug`, `event/:slug`, `year/:slug`, 엔트리는 `:slug`).
- 엔트리 본문은 `entry_posts` 콜렉션([`_entry_posts/`](/_entry_posts/))에 통합돼 있고, `entry_pages`([`_entry_pages/`](/_entry_pages/))는 홈·아카이브·소개·포스트 분류 페이지 같은 고정 페이지용이다. 전체 포스트 아카이브(`/posts`)는 `entry_pages` 폴더에 있는 [`posts.md`](/_entry_pages/posts.md) 가 `archive-type` 레이아웃으로 `entry_posts` 콜렉션을 조회하는 구조다.
- `defaults` 블록이 콜렉션별로 `layout` 과 `meta` 목록을 자동 지정하므로, 개별 문서에서 굳이 재지정할 필요가 없다. 새 콜렉션·레이아웃을 도입할 때는 이곳에도 기본값 엔트리를 추가할 것.
- 각 콜렉션 폴더의 `__TEMPLATE__.md` 는 빌드에서 `exclude` 로 배제돼 있다. 새 템플릿 파일을 늘리면 `exclude` 목록도 갱신할 것.

### 포스트 작성 시 분류 검증
- 포스트의 `projects`·`events`·`years` 프론트매터 값은 각각 [`_archive_projects/`](/_archive_projects/)·[`_archive_events/`](/_archive_events/)·[`_archive_years/`](/_archive_years/) 에 실재하는 아카이브 페이지의 `title` 과 정확히 일치해야 한다. 불일치하면 분류 페이지에서 해당 포스트가 누락된다.
- [`_entry_posts/__TEMPLATE__.md`](/_entry_posts/__TEMPLATE__.md) 의 `projects`·`events` 샘플 값은 현재 아카이브 구성과 일치하지 않는 예시다. 포스트를 새로 만들 때 샘플 값을 그대로 두지 말고 실재 아카이브 중에서 선택하거나, 필요하면 먼저 해당 아카이브 페이지를 생성할 것.

### 데이터
- [`_data/`](/_data/) 의 YAML 은 템플릿에서 `site.data.<파일명>` 으로 참조된다.
  - `nav.yml` — 상단 네비게이션 링크([`layout-nav.html`](/_includes/layout-nav.html)).
  - `queries.yml` — 엔트리의 `queries` 프론트매터가 생성하는 외부 검색 링크 목록([`parts-metadata.html`](/_includes/parts-metadata.html)). 새 서비스를 추가하려면 여기에 `label`·`class`·`href` 를 등록하고 `parts-metadata.html` 의 `{% if post.queries.<key> %}` 블록도 복제해야 한다.
  - `keywords.yml`·`profile.yml`·`badges.yml`·`license.yml`·`links.yml` — 문자열 상수·UI 요소.

### 스타일
- 엔트리포인트는 [`assets/style.scss`](/assets/style.scss) 하나뿐이다. [`_sass/`](/_sass/) 파일을 추가하면 여기 `@import` 를 추가해야 컴파일된다.
- SCSS 네이밍도 인클루드와 유사하다: `app-variables`·`mixins` → `base-*` → `layout-*` → `entry-*`·`entry-body-*`·`entry-format-*` → `context-*` → `component-*`.
- Kramdown 문법 하이라이터는 `rouge`, 래퍼 클래스는 `highlight` 로 고정돼 있다.