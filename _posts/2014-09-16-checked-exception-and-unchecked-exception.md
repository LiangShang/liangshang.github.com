---
layout: post
title: "Checked Exception and Unchecked Exception In Java"
description: ""
category: Interview Question
tags: [Exception, Interview, Java]
---
{% include JB/setup %}
Using Checked Exception and Unchecked Exception is a very interesting topic and is very likely to be asked during an interview. In Java, 
>Checked exceptions consist of the `java.lang.Exception` class and all of its subclasses; except for `java.lang.RuntimeException` and its subclasses\[1\].

Unchecked exceptions, meanwhile, includes the runtime exceptions and the errors. For the checked exceptions, we need to expliticly handle it, by using <code>try ... catch ...</code> or <code>throws ...</code> whilist for the unchecked exceptions, we need not to do that. 

There is a problem that when shall we use a checked exception and when use an unchecked one. From the book <i>Effective Java</i>, the author says that the main principle is as:
>use checked exceptions for conditions from which the caller can reasonably be expected to recover.

By thrown or propagated outward, each checked exception is a potent indication that there is a possible outcome to invoke the corresponding method. It is also a mandate for the callee to recover from the conditions that causes the exception.

The great majority of runtime exceptions, on the other side, indicate the <i>precondition violations</i>, such as the <code>OutOfBoundException</code>.




####Reference####
\[1\] Java checked exceptions VS runtime exceptions. http://johnpwood.net/2008/04/21/java-checked-exceptions-vs-runtime-exceptions 
