---
layout: post
title: "Notes on Effective Java(1) - Common Methods to all Objects"
description: "This post covers the third chapter of Effective Java, talking about equals, hashCode, toString and so on"
category: Effective Java
tags: [Java]
---
{% include JB/setup %}

##Concerns when overriding equals( )##

First of all, 5 rules to follow when overriding `equals()` function:

1. Reflexive, ie `a.equals(a)`
2. Symmetric, ie `a.equals(b) => b.equals(a)`. This may be violated when comparing with subclasses
3. Transitive, ie `a.equals(b) & b.equals(c) => a.equals(c)`
4. Consistent, if `a.equals(b)` then we always have `a.equals(b)` if the compared fields of `a` and `b` are not changed.
5. Non-nullity, that `a.equals(null) == false`

Secondly, there are some guidance to override `equals()`

1. Using `==` to check whether the parameter is the reference of `this`, which is an optimization.
2. Using `instanceof` to check whether the parameter is of the same type. Pay attention here that `null` value can be chekced here as well by `instanceof`
3. Casting the parameter to the correct type.
4. Checking every “significant” field in the class. Especially, 
    + to avoid `NullPointerException` when checking the `null` valued fields, we can use `field == null? o.field == null: field.equals(o.field)`
    + for the optimization, check the fields that are mostly unlike to equal first.
5. When it is finished, testing the 5 rules mentioned above.


<br/>

##Always **override hashCode( )** when overriding equals##
  
As a matter of fact, we **must** override hashCode in every class that overrides equals. There is a contract of `hashCode()`:

> Whenever it is invoked on the same object more than once during an execution of an application, the hashCode method must consistently return the same integer, provided no information used in equals comparisons on the object is modified. This integer need not remain consistent from one execution of an application to another execution of the same application.>If two objects are equal according to the `equals(Object)` method, then calling the hashCode method on each of the two objects must produce the same integer result.>It is not required that if two objects are unequal according to the `equals(Object)` method, then calling the hashCode method on each of the two objects must produce distinct integer results. However, the programmer should be aware that producing distinct integer results for unequal objects may improve the performance of hash tables.

All in all, a good `hashCode()` function tends to produce equal hash codes for the objects that are equal and unequal ones for unequal objects. Although achieving this ideal can be diffcult, fortunately we have a recipe to achieve a fair approximation from [Effective Java](http://www.amazon.co.uk/Effective-Java-Edition-Joshua-Bloch/dp/0321356683). 
1. Store some constant nonzero value, say, 17, in an int variable called result. 
2. For each significant field f in your object (each field taken into account by the equals method, that is), do the following:    1. Compute an int hash code c for the field:        - If the field is a boolean, compute `(f ? 1 : 0)`.        - If the field is a byte, char, short, or int, compute `(int) f`.        - If the field is a long, compute `(int)(f^(f>>>32))`.        - If the field is a float, compute `Float.floatToIntBits(f)`.        - If the field is a double, compute `Double.doubleToLongBits(f)`, and then hash the resulting long as in step above.        - If the field is an object reference and this class’s equals method compares the field by recursively invoking equals, recursively invoke hashCode on the field. If a more complex comparison is required, compute a “canonical representation” for this field and invoke hashCode on the canonical representation. If the value of the field is null, return 0 (or some other constant, but 0 is traditional).        - If the field is an array, treat it as if each element were a separate field. That is, compute a hash code for each significant element by applying these rules recursively, and combine these values per step 2.2. If every element in an array field is significant, you can use one of the `Arrays.hashCode` methods added in release 1.5.	
      	2. Combine the hash code c computed in step 2.1 into result as follows: `result = 31 * result + c;`3. Return result.4. When you are finished writing the hashCode method, ask yourself whether equal instances have equal hash codes. Write unit tests to verify your intuition! If equal instances have unequal hash codes, figure out why and fix the problem.


##Conclusion##
