---
layout: post
title: "Developing an Analyzer for Elasticsearch"
description: "This post introduces the several things need to do when writing a customer analyzer for Elasticsearch"
category: 
tags: [elasticsearch ]

---
{% include JB/setup %}

## Introduction


This post introduces how to write a customer analyzer for Elasticsearch. Firstly, it introcudes how to implement an Elasticsearch plugin so that it can be found at runtime, followed by a list of configuration files and interfaces that need  writing and implemeting respectively in the analyzer for lucene. At last, there is an working code base as an example.

&nbsp;

## Things to do for Elasticsearch Analyzer Plugin

&nbsp;

**Specify the plugin 指定ES去读取哪个插件**

1. create a file named `es-plugin.properties`
* write `plugin={pluginClassName}` in the file, for example `plugin=org.elasticsearch.plugin.analysis.pinyin.AnalysisPinyinSegmentationPlugin`

**Fullfill the plugin class 实现之前指定的插件类**
This plugin class must implement the interface `org.elasticsearch.plugins.AbstractPlugin`.

TO BE CONTINUED....

 
