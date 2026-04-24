NEMORIUM.NET
============

이 [저장소](https://github.com/oxmhpark/my-homepage)는 [개인 홈페이지](https://nemorium.net) 개발 프로젝트이다. 이 문서는 개발 및 운영에 대한 지침을 담고 있으며 챗봇의 편집을 금한다.

## 적용 기술

| 항목   | 제품            | 링크                                          |
|--------|-----------------|-----------------------------------------------|
| 플랫폼 | 깃허브 페이지스 | <https://pages.github.com>                    |
| 블로그 | 지킬            | <https://jekyllrb.com>                        |
| 댓글   | giscus          | <https://giscus.app>                          |
| 검색   | GPSE            | <https://programmablesearchengine.google.com> |
| 글꼴   | Freesentation   | <https://freesentation.blog>                  |
| 도메인 | Cloudflare      | <https://cloudflare.com>                      |

## 컨텐츠 작성 흐름

- `_entry_posts` 폴더에서 포스트 템플릿을 복사한다.
- `title`·`slug`·`date`·`lastmod`(옵션)·`excerpt` 프론트매터를 설정한다.[^slug-policy]
- 적절한 분류(`projects`·`events`·`years` 프론트매터)를 설정한다.[^no-overflow]
- 설정한 분류에 대응하는 아카이브 페이지가 존재하는지 확인한다.
- 외부에서 탈고하는 것을 원칙으로 삼는다.[^no-published-flag]
- 탈고 후 곧바로 원격 저장소에 커밋한다.[^no-local-build]
- 깃허브 페이지스 자동 빌드 액션을 통해 출판한다.[^no-lint]

[^slug-policy]: 제목을 반영해서 한글로 설정하되 공백을 대시(`-`)로 치환한다.
[^no-overflow]: 각 분류는 그 자체로 하나의 프로젝트임을 명심하고 남용하지 않도록 주의할 것.
[^no-published-flag]: `published` 프론트매터를 사용하지 않는다.
[^no-local-build]: `Gemfile`·`package.json` 등의 로컬 빌드용 파일이 필요없으며 `_site/` 폴더도 산출하지 않는다.
[^no-lint]: 현재로서는 평가·테스트 파이프라인을 마련할 실익이 크지 않다.

## 컨텐츠 아키텍처

### 고정 페이지

관리 목적이 아니라면 추가하지 않는다.

| 항목      | 기능              | 소스 경로                                                                                                   |
|-----------|-------------------|-------------------------------------------------------------------------------------------------------------|
| 첫 페이지 | 최근 포스트 출력  | [`/index.md`](https://github.com/oxmhpark/my-homepage/tree/main/index.md)                                   |
| 아카이브  | 검색 및 분류 탐색 | [`/_entry_pages/archives.md`](https://github.com/oxmhpark/my-homepage/tree/main/_entry_pages/archives.md)   |
| 즐겨찾기  | 외부 링크 모음    | [`/_entry_pages/bookmarks.md`](https://github.com/oxmhpark/my-homepage/tree/main/_entry_pages/bookmarks.md) |
| 소개      | 웹사이트 소개     | [`/_entry_pages/about.md`](https://github.com/oxmhpark/my-homepage/tree/main/_entry_pages/about.md)         |
| RSS 피드  | 구독 기능 제공    | [`/feed.xml`](https://github.com/oxmhpark/my-homepage/tree/main/feed.xml)                                   |
| 사이트맵  | 검색 최적화       | [`/sitemap.xml`](https://github.com/oxmhpark/my-homepage/tree/main/sitemap.xml)                             |
*※소스 경로 기술 방침 참고*[^linking-source]

[^linking-source]: 주 브랜치의 현재 커밋이 기준이므로 절대경로를 링크한다.

### 프로젝트·이벤트·연도 분류

- 프로젝트·이벤트·연도 분류는 복수 지정이 가능하다.

| 범주     | 기준                                      | 설정                       | 분류 페이지 경로                                                                              |
|----------|-------------------------------------------|----------------------------|-----------------------------------------------------------------------------------------------|
| 프로젝트 | 계획에 따른 실행                          | `projects` 프론트매터 지정 | [`/_archive_projects/`](https://github.com/oxmhpark/my-homepage/tree/main/_archive_projects/) |
| 이벤트   | 외부 일정 또는 사건사고에 대한 대응       | `events` 프론트매터 지정   | [`/_archive_events/`](https://github.com/oxmhpark/my-homepage/tree/main/_archive_events/)     |
| 연도     | 포스트 작성일이 아닌 내용과 관계있는 연도 | `years` 프론트매터 지정    | [`/_archive_years/`](https://github.com/oxmhpark/my-homepage/tree/main/_archive_years/)       |
*※소스 경로 기술 방침 참고*[^linking-source]

## 운영 방침

- 이미지 업로드·글감 수집·토막글 저장은 [마스토돈 계정](https://mastodon.social/@oxmhpark)을 이용한다.
- 본문 작성은 [옵시디언](https://obsidian.md)을 이용하고, 저장소에는 최종본만 반영한다.
- 논거를 추가하는 정도라면 서두에 주요 갱신 이력을 남기고, 논조에 변화가 있다면 별도의 포스트로 분리한다.
- 중복 링크를 만들지 않는다.[^no-link-duplication]
- 텍스트 제목·제품 이름·사건 이름·대사 인용에는 겹낫표(『』)를, 소제목·지문 인용에는 홑낫표(「」)를 쓴다.
- 인명·국가명·지명은 기호로 감싸거나 강조하지 않는다.
- 앱 이름·용어·API·코드 조각은 코드 블록으로 감싼다.
- 고유명사는 한글 표기를 기본으로 삼되,[^localization-policy] 첫 인용에는 원문을 병기한다.
- `<h3>` 이하의 섹션 사용을 자제한다.
- 반복 응답 또는 장문의 응답이 필요한 경우에는 그 내용을 별도의 포스트로 승격시킨다.

[^no-link-duplication]: 프로젝트·이벤트·연도 분류로 지정된 단어가 본문에 재등장하는 경우에 특히 주의할 것.
[^localization-policy]: 순우리말로 대체할 필요는 없다.