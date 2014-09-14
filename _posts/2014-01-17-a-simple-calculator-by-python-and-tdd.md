---
layout: post
title: "A Simple Calculator by Python and TDD"
description: ""
category: 
tags: [python, TDD]
excerpt: A article about writing TDD code in Python with a tiny example
---
{% include JB/setup %}

In this article, I will implement a very simple calculator using python and TDD. if you do not know what is TDD, please refer to [here](http://en.wikipedia.org/wiki/Test-driven_development). BTW, my python version is 2.7

First of all, I write a failed test using unittest module in python, which is just like this:
{% highlight ruby %}
import unittest

class CalculatorTest(unittest.TestCase):
    calculator = Calculator()
    def test_add(self):
        self.assertEqual(4, self.calculator.add(2,2))

if __name__ == "__main__":
    unittest.main()
{% endhighlight %}

Only write one failed test once and then run it. Of course it will fail, Calculator class is not defined yet. So I create an empty file Calculator.py and declare the class Calculator. 
{% highlight python3 %}
class Calculator:
    pass
{% endhighlight %}

Then update the test file to
{% highlight python3 %}
import unittest
from Calculator import Calculator

class CalculatorTest(unittest.TestCase):
    calculator = Calculator()
    def test_add(self):
        self.assertEqual(4, self.calculator.add(2,2))

if __name__ == "__main__":
    unittest.main()
{% endhighlight %}
Run the test again and then it fails again, saying add function is not defined. Well, next step is of course create that function. I will skip some steps here and if some more specificated detail about TDD is required, please refer to the example writen by [Beck(2002)](http://www.amazon.com/Test-Driven-Development-By-Example/dp/0321146530/ref=sr_1_1?ie=UTF8&qid=1390001488&sr=8-1&keywords=test+driven+development+by+example). So the Calculator.py turns to 
{% highlight python3 %}
class Calculator:

    def add(self, x, y):
        return x + y
{% endhighlight %}

Then add some more functions to the Calculator like the minus, multiply and devide. So the test file is like:
{% highlight python3 %}
import unittest
from Calculator import Calculator

class CalculatorTest(unittest.TestCase):
    calculator = Calculator()
    def test_add(self):
        self.assertEqual(4, self.calculator.add(2,2))
        self.assertEqual(3.2, self.calculator.add(1,2.2))

    def test_minus(self):
        self.assertEqual(2, self.calculator.minus(3,1))
        self.assertEqual(-2, self.calculator.minus(1,3))

    def test_multiple(self):
        self.assertEqual(12, self.calculator.multiple(3,4))
        self.assertEqual(13.5, self.calculator.multiple(3,4.5))

    def test_devide(self):
        self.assertEqual(3, self.calculator.devide(9,3))

if __name__ == "__main__":
    unittest.main()
{% endhighlight %}
And the Calculator.py
{% highlight python3 %}
class Calculator:

    def __init__(self):
        pass

    def add(self, x, y):
        return x + y

    def minus(self, x, y):
        return x - y

    def multiple(self, x, y):
        return x * y

    def devide(self, x, y):
        return x/y

{% endhighlight %}
DO we miss something? What would the calculator do if the y parameter is 0 when deveide() is called? the ZeroDivisionError should be raised, shouldn't it? As a result, the test file should be like
{% highlight python3 %}
import unittest
from Calculator import Calculator

class CalculatorTest(unittest.TestCase):
    calculator = Calculator()
    def test_add(self):
        self.assertEqual(4, self.calculator.add(2,2))
        self.assertEqual(3.2, self.calculator.add(1,2.2))

    def test_minus(self):
        self.assertEqual(2, self.calculator.minus(3,1))
        self.assertEqual(-2, self.calculator.minus(1,3))

    def test_multiple(self):
        self.assertEqual(12, self.calculator.multiple(3,4))
        self.assertEqual(13.5, self.calculator.multiple(3,4.5))

    def test_devide(self):
        self.assertEqual(3, self.calculator.devide(9,3))

        # How to deal with Exception in TDD?
        self.assertRaises(ZeroDivisionError, self.calculator.devide(3,0))

if __name__ == "__main__":
    unittest.main() 
{% endhighlight %}
Notice the assertRaises() function. Then run the test and there is a error saying that there is a ZeroDivisionError...It seems that we are not using the assertRaises() right, let's change it into like this:
{% highlight python3 %}
    def test_devide(self):
        self.assertEqual(3, self.calculator.devide(9,3))

        # How to deal with Exception in TDD?
        with self.assertRaises(ZeroDivisionError):
            self.calculator.devide(3,0) 
{% endhighlight %} 
With using a "with" clause, run the test again and then it passes!

But WHY is the "fantacy with" working? Well, it is next topic.