<% {% assign title = "Enter Blog Post Title:" | prompt %}

{% if title == "" %}
  {{ "Please enter a valid title." | message('error') }}
  {% return %}
{% endif %}

{% assign blogFolder = "/content/post/all_posts" %}

{% if not blogFolder | file_exists %}
  {{ "The specified folder path doesn't exist." | message('error') }}
  {% return %}
{% endif %}

{% assign dirPath = blogFolder | append: "/" | append: title %}

{% if not dirPath | file_exists %}
  {{ dirPath | create_folder }}
{% endif %}

{% assign filePath = dirPath | append: "/index.md" %}

{% capture content %}---
title: {{ title }}
date: {{ "now" | date: "%Y-%m-%d" }}
tags: []
---

Write your blog post content here.
{% endcapture %}

{{ content | create: filePath }}

{{ filePath | open }} %>
