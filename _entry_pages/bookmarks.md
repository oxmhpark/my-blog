---
title: "#즐겨찾기"
slug: bookmarks
meta: null
---
{% assign events = site.archive_events | default "" %}
{% if events == "" %}{% assign events = "" | split: "" %}{% endif %}
{% assign posts = events | where_exp: "post", "post.bookmark == true" | sort: "title" %}
{% include list-posts-as-links.html %}
