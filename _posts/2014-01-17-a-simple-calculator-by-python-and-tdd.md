---
layout: post
title: "A Simple Calculator by Python and TDD"
description: ""
category: 
tags: [python, TDD]
---
{% include JB/setup %}

In this article, I will implement a very simple calculator using python and TDD. if you do not know what is TDD, please refer to [here](http://en.wikipedia.org/wiki/Test-driven_development). BTW, my python version is 2.7

First of all, I write a failed test using unittest module in python, which is just like this:<br/>
<code>
import unittest
<br/><br/>
class CalculatorTest(unittest.TestCase):<br/>
    calculator = Calculator()<br/>
    def test_add(self):<br/>
        self.assertEqual(4, self.calculator.add(2,2))<br/>
if __name__ == "__main__":<br/>
    unittest.main()<br/>
</code>
