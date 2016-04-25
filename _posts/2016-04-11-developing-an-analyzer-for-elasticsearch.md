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

Besides overriding the `processTokenFilters()`, we can also override the `processCharFilters()` and `processTokenizers()`, which represent the [three parts of ES analyzer](https://www.elastic.co/guide/en/elasticsearch/guide/current/custom-analyzers.html) respectively.


From the `BinderProcessor` we find that a `PinyinTokenFilterFactory` is registered, which provides a `TokenFilter` by fullfilling the `create(TokenStream ts)` method.

    package org.elasticsearch.index.analysis;

    import org.apache.lucene.analysis.TokenStream;
    import org.elasticsearch.common.inject.Inject;
    import org.elasticsearch.common.inject.assistedinject.Assisted;
    import org.elasticsearch.common.settings.Settings;
    import org.elasticsearch.index.Index;
    import org.elasticsearch.index.settings.IndexSettings;


    public class PinyinTokenFilterFactory extends AbstractTokenFilterFactory {
        @Inject public PinyinTokenFilterFactory(Index index, @IndexSettings Settings indexSettings, @Assisted String name, @Assisted Settings settings) {
            super(index, indexSettings, name, settings);
        }

        @Override public TokenStream create(TokenStream tokenStream) {
            return
                    new PinyinSegmentationTokenFilter(
                            new PinyinTokenFilter(tokenStream));
        }
    }


TO BE CONTINUED....

 
