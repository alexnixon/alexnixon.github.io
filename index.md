---
layout: default
title: Alex on Code
---
This is Alex on Code: the website of Alex Nixon, a London-based technologist.

Occasionally I have thoughts which I think are worth sharing. When that happens I'll write them down and they'll appear here. Usually, but not always, they will be related to some aspect of programming, technology, or the craft of software engineering.

If you find anything I've written interesting, immoral, informative, ignorant or any other adjective, then [let me know](/contact/)! If you'd like to be alerted when I post something new then [sign up](http://eepurl.com/gNIAJ5).

<div class="post-list">
  {% for post in site.posts %}
  <a href="{{ post.url }}">
  <div class="post-item">
    <div class="post-header">
      <img alt="Cover image for blog post" class="post-cover" src="{{ post.image }}"/>
      <div class="post-title"> {{ post.title }}</div>
      <div class="post-author"> {{ post.date | date_to_string }}</div>
    </div>
    <div class="post-description"><p>{{ post.teaser }}</p></div>
  </div>
  </a>
  {% endfor %}
</div>