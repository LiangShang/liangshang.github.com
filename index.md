---
layout: page
title: Home Page
---
{% include JB/setup %}

## Introduction

Hi, this is my blog, where I share things that I am learning and have learned. I got my Master on Computing in late 2014 and in the blog the main topic is about computing and programming.


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

## Advertisement

<h3>Need A Job</h3> I just graduated as a Master and now am looking for a job as a **Software Engineer** in London. My favourate language is Java and Python and you can find my [Resume](https://github.com/LiangShang/C.V./blob/master/Liang's%20CV.pdf?raw=true). To work in London, I also need a sponsor, so I will really appreciate it if anyone is able to give me an Internal Referral :) 


