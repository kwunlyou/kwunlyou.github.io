---
layout: post
title:  "Call Overridden Methods from __init__ in Python"
categories: [Programming]
tags: [Python, Pattern Design]
excerpt: ""
---

#### Python Rules in [Semmle](https://semmle.com/){:target="_blank"}
- [\_\_init\_\_ method calls overridden method](https://help.semmle.com/wiki/display/PYTHON/__init__+method+calls+overridden+method){:target="_blank"}

#### Two related questions in Stack Overflow
- [Is it safe to call an overridden method from \_\_init\_\_()?](https://stackoverflow.com/questions/6858842/is-it-safe-to-call-an-overridden-method-from-init){:target="_blank"}
- [python: calling super().\_\_init\_\_ too early in the \_\_init\_\_ method?](https://stackoverflow.com/questions/5786441/python-calling-super-init-too-early-in-the-init-method){:target="_blank"}


#### A similar problem in C++:
- C++ Super-FAQ: [When my base class’s constructor calls a virtual function on its this object, why doesn’t my derived class’s override of that virtual function get invoked?](https://isocpp.org/wiki/faq/strange-inheritance#calling-virtuals-from-ctors){:target="_blank"}

#### Constructor and Template Method Pattern
- [Java Anti-Pattern: Constructors and Template Method Pattern](http://novyden.blogspot.co.uk/2011/08/java-anti-pattern-constructors-and.html){:target="_blank"}

