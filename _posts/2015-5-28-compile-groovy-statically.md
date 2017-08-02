---
layout: post
title: Compiling groovy code statically
tag:  Groovy, compilation
comments: true
---

Groovy by default uses dynamic dispatch. Dynamic Dispatch could be defined as the capability of adding or replacing units of code at run time. For example, we can change the definition of a method or we can add new methods to a class at run time using metaprogramming in Groovy.

Even after being a powerful feature of Groovy, there may come requirements where we want to avoid or restrict Groovy to use Dynamic Dispatch and opt in for Static Dispatch.

One way to do that is using CompileStatic annotation which could be implemented at class level or at method level or as a combination of both the methods and class level implementation.

Adding annotation @groovy.transform.CompileStatic will allow code to be compiled statically and will ensure type safety.

*Note:-* Following features will be gone once enabling static dispatch for a class or method.
 1. No dynamic finders would work (if using gorm / grails)
 2. def wont work in cases where there is a non matching type assigned like in example below
 3. Every variable would be checked for its declaration at compile time and not at run time like it was happening in regular Groovy code
 
     import groovy.transform.CompileStatic
	
	 @CompileStatic
	 class Test{
		 def test(){
			def name = ["test":"test2"]
			name.substring(0,3)
			//throw a compile time error as name is not string
			//at compile time
		 }
	  
	 }
	 println "Code in this file is statically compiled"
 
If we run code above in say groovyConsole, it will throw compile time errors at test method as below:
	
	1 compilation error:
	 
	[Static type checking] - Cannot find matching method java.util.LinkedHashMap#substring(int, int). Please check if the declared type is right and if the method exists.
	 at line: 10, column: 8

There might come a scenario where you only need to statically compile a method only e.g. run method of your thread (we don’t want it to fail at runtime to be rest assured). To fulfill such requirements, we may try code below

	import groovy.transform.CompileStatic
	class Test{
		def test(){
		   def name = ["test":"test2"]
		   name.substring(0,3)
		   //this will not throw a compile error since not statically compiled
		}
	 
		@CompileStatic
		def test1(){
		   def name = ["test":"test2"]
		   name.substring(0,3)
		   //it will throw compile time errors
		}
	}
	println "Code in this file is statically compiled"

We can observe by running the code that the error is now thrown only for test1 method and not for test method.
Finally, there might come a scenario asking for excluding very few methods of a class from static dispatch.

	import groovy.transform.CompileStatic
	import groovy.transform.TypeCheckingMode
 
	@CompileStatic
	class Test{
	 
		def test(){
		   def name = ["test":"test2"]
		   name.substring(0,3)
		}
	 
	/**
	*by setting the TypeCheckinMode, this method will be skipped for static dispatch / compile
	**/
		@CompileStatic(TypeCheckingMode.SKIP)
		def test1(){
		   def name = ["test":"test2"]
		   name.substring(0,3)
		}
	}


TypeCheckingMode is of two types:
- SKIP – to skip static compilation
- PASS – to compile statically. Default, if not specified for CompileStatic

Though it’s my very own perception but, one good usage of TypeCheckingMode could be to quickly change a method from static compiled to dynamically compiled or vice versa.
Share your thoughts in the comment section below. To know more about our Grails service, get in touch with us. 