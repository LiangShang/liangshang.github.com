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

for example as the code 

    package org.elasticsearch.plugin.analysis.pinyin;

    import org.elasticsearch.index.analysis.AnalysisModule;
    import org.elasticsearch.index.analysis.PinyinAnalysisBinderProcessor;
    import org.elasticsearch.plugins.AbstractPlugin;

    
    public class AnalysisPinyinSegmentationPlugin extends AbstractPlugin {

        @Override
        public String name() {
            return "analysis-pinyin-segmentation";
        }

        @Override
        public String description() {
            return "Chinese to Pinyin convert support";
        }

        public void onModule(AnalysisModule module) {
            module.addProcessor(new PinyinAnalysisBinderProcessor());
        }
    }

The class extends the AbstractClass `AbstractPlugin` and fullfill the following methods, altogether with the most important one named `onModule` , the method that add our custom plugin to the ES. 

From the file, we found that we need to write the class named `PinyinAnalysisBinderProcessor`. Let's find what is in that class.

    package org.elasticsearch.index.analysis;


    public class PinyinAnalysisBinderProcessor extends AnalysisModule.AnalysisBinderProcessor {

        @Override
        public void processTokenFilters(TokenFiltersBindings tokenFiltersBindings) {
            tokenFiltersBindings.processTokenFilter("pinyin_segment", PinyinTokenFilterFactory.class);
        }
    }

So this BinderProcessor is more like a register that binds the token filter to ES instance by registering a Factory.

TO BE CONTINUED....

 
