---
layout: post
title:  "블로그 첫글!"
categories: [blog]
tags: [daily, blog]
---

# 안녕하세요!

**Hello world**, 제 첫번째 블로그 포스트입니다.

여러 일들을 열심히 보고 듣고 느낀 후  
시간나는 대로 포스팅 하겠습니다.  

I hope you like it!

{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}

{% for category in site.tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}
