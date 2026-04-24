---
layout: entry-page
meta: nil
---
<!-- 자유형식 -->

<!-- 최근 글 -->
{% assign posts = site.entry_posts | default: "" %}
{% if posts == "" %}{% assign posts = "" | split: "" %}{% endif %}
{% assign posts = posts | sort: "updated" | reverse | limit:7 %}
{% include list-posts.html %}
