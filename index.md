---
layout: default
title: Alex Nixon's blog
---
You've found your way to the blog of Alex Nixon, a London-based software engineer. More information on me and the blog can be found on the [about](/about/) page.
 
All content is below. No, there isn't much yet.

<div class="post-list">
  {% for post in site.posts %}
    <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
      {{ post.date | date_to_string }}
      <p><i>{{post.teaser}}</i></p>
  {% endfor %}
</div>