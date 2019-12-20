---
layout: default
title: Alex Nixon's blog
---
You've found your way to the website of Alex Nixon, a London-based technologist. I use my technical skills to solve problems for companies and people. More information about me and the blog can be found on the [about](/about/) page.

Occasionally I have thoughts which I think are worth sharing. When that happens I'll write them down and they'll appear here. Usually, but not always, they will be related to some aspect of programming, technology, or the craft of software engineering.

If you find anything I've written interesting, informative, imorral, impeccable or any other adjective, then [let me know](/contact/)! If you'd like to be alerted when I post something new then [sign up](http://eepurl.com/gNIAJ5).

<div class="post-list">
  {% for post in site.posts %}
    <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
      {{ post.date | date_to_string }}
      <p><i>{{post.teaser}}</i></p>
  {% endfor %}
</div>