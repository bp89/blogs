---
layout: post
title: Groovy shorthand operators
tag: groovy, operators, shorthand operators
comments: true
---

Every language have shorthand operators that allows multi line operations to be performed in single line. For example ?: is a ternary operator used in scenarios where condition and  the code inside condition is very small and specific to comparison operands.

I'm fan of few groovy shorthand operators mentioning below :-

	**         power operator(varies in different languages. Hence, worth mentioning)

	*.    -  Spread Operator behaves like a collect( invoke an action on all items of an aggregate               object) in groovy. For example you want a list with  name of  person only from a list of            person, then do following. Good thing is this operator is null safe.

	[new Person(name:"Martin",age:26),new Person(name:"Mark",age:21),new Person(name:"Jack",age:35)]*.name

	and you will get all the names in list.

	? if : else      A ternary operator as a replacement of if else condition.
	?.         null-safe operator. Used widely to avoid a !=null ? a.perform() : otherwise situation

	?:        used to avoid  a!=null? a : otherwise situation
	.@      access the field of an object without calling getter(groovy by default calls the getter)
	.&       used to store a reference to a method in a variable e.g
			  def call = "my name is jack".toUpperCase()
			  call() //call like a method
	~        provides a simple way to create a regex pattern
			 ~/abc/ or ~"abc" or ~'def' or ~$/abc/$ all are patterns
	=~   to create a matcher instance java.util.regex.Matcher
	..      Range operator . Shortcut to specify ranges like 1..10 says 1 to 10 including 1 and 10
	..<    Range operator . Shortcut to specify ranges like 1..10 says 1 to 10 including 1 and excluding             10 . A range is an instance of groovy.lang.IntRange.

	 Magic part is range is also applicable to characters
	 e.g,
			   ('a'..'f').collect() returns range from a to f.

	<=>   SpaceShip operator. It delegates to compareTo and hence a short way of calling compareTo

	e.g , 1<=>1 = 0 or 1<=>2=-1

	[]       Subscript Operator. Shorthand notation of getAt and putAt for list
	in       Membership Operator. Used to check membership inside a list and equivalent to isCase
	as      Coercion Operator. This is shorthand for casting(and much better ) and converts one type to another if convertible

	<>     Diamond Operator. Used to delegate generic type from declaration. e.g
			  List<String> a = new List<>();

Hope it helps!