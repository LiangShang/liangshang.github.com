---
layout: page
title: Home Page
---
{% include JB/setup %}

&nbsp;

## Introduction

Hi, this is my blog, where I share things that I am learning and have learned recently. I will try my best to write English blogs and sometimes also some Chinese ones.


&nbsp;&nbsp;&nbsp;

## Recent Blogs

<ul class="posts">
  {% for post in site.posts %}

    {% if forloop.index == 4 %}
      {% break %}
    {% endif %}
    <li><b><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></b>
    <br/>
    {{ post.excerpt }} 
    </li>
    <br/>
  {% endfor %}
</ul>

&nbsp;&nbsp;

## Who am I?

I am now working in Shanghai, China as a **Software Engineer**. I am writing Java programmes and maintain Elasticsearch in my company. And you can find my [Resume](https://github.com/LiangShang/C.V./blob/master/Liang's%20CV.pdf?raw=true).


