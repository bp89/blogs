---
layout: post
title: Functional Programming in Java 8 Using Lambda Expressions
categories: [Functional-Programming, Lambda-Calculas, Programming]
tags: [Java-8, Lambda-Expressions]
comments: true
---


Lambda expressions are the most talked feature of Java 8. Lambda expressions drastically change the way we write the code in Java 8 as compared to what we used to, in older versions. 

Let’ understand what lambda expression is and how it works.

At first, you could think about lambda expressions as a way of supporting functional programming in Java. Functional programming is a paradigm that allows programming using expressions i.e. declaring functions, passing functions as arguments and using functions as statements (rightly called expressions in Java8). If you want to learn more about why it is called Lambda expressions, you could try to understand what Lambda Calculus is.
If you have worked on groovy and any other programming language which supports functional programming, comprehending Lambda expressions shouldn’t be a big deal. However, there is more about Lambda expressions than just being methods getting passed along with other methods.
Consider the following example to know more about Lambda Expressions. 
Suppose you have to write a program that welcomes you into a different world of programming language.

A simple implementation could be as:

	public class WelcomeToProgrammingWorld {
		public static void main(String[] args) {
			System.out.println("Welcome to world of Java Programming Language!");
		}
	}

Now, what if I also need to welcome you in Groovy and Python. A simple code could be as below:

	public class WelcomeToProgrammingWorld {
		public static void main(String[] args) {
			System.out.println("Welcome to world of Java Programming Language!");
			System.out.println("Welcome to world of Groovy Programming Language!");
			System.out.println("Welcome to world of Python Programming Language!");
		}
	}

It’s not a correct approach. You shall be thinking about writing some generic implementation. You either create some method as below:

	public class WelcomeToProgrammingWorld {
		public static void main(String[] args) {
			welcomeToALanguage("Java");
			welcomeToALanguage("Groovy");
			welcomeToALanguage("Scala");
		}
		static void welcomeToALanguage(String language) {
			System.out.println("Welcome to world of " + language + "Programming Language");
		}
	}

Though the code looks concise, we could write it as follows:

	public class WelcomeToProgrammingWorld {
		public static void main(String[] args) {
			WelcomeToALanguage welcomeToALanguage = new WelcomeToALanguage();
			welcomeToALanguage.welcome("Java");
			welcomeToALanguage.welcome("Groovy");
			welcomeToALanguage.welcome("Scala");
		}
	}
	 
	interface Welcome {
		abstract void welcome(String string);
	}
	 
	class WelcomeToALanguage implements Welcome {
		@Override
		public void welcome(String language) {
			System.out.println("Welcome to world of " + language + "Programming Language");
		}
	}

Notice that we now have an interface, Welcome which could have multiple implementations for different ways to welcome. We have created a concrete class WelcomeToALanguage implementing the Welcome interface. Still, the output remains same, but the code is more generic and allows more ways of welcoming. 

Here, if you have to welcome something differently, you need to provide another implementation and so on.

Another solution here could be to use Anonymous inner class to avoid creating multiple concrete classes for various implementations.

	public class WelcomeToProgrammingWorld {
		public static void main(String[] args) {
			Welcome welcomeToALanguage = new Welcome() {
				@Override
				public void welcome(String language) {
					System.out.println("Welcome to world of " + language + "Programming Language");
				}
			};
			welcomeToALanguage.welcome("Java");
			welcomeToALanguage.welcome("Groovy");
			welcomeToALanguage.welcome("Scala");
		}
	}
	 
	interface Welcome {
		abstract void welcome(String string);
	}
So, now we have a robust and generic code which allows declaring and using different implementations with anonymous inner class.
Now, let us understand Lambda expression and its implementation in the above example. 

An implementation for a method say :

	public void welcome(String language) {
		  System.out.println("Welcome to world of " + language + "Programming Language");
	}

will look like below in a lambda expression:

	(String language) -> {
		   System.out.println("Welcome to world of " + language + "Programming Language");
	}

You may want to forget about the assignment to some reference variable for now (and compilation too).

If you notice the difference:-
1. We have removed access specifier and return type as is intuitive for compiler by method definition. 
2. We have a new operator -> which is called Lambda expression i.e. it’s the separator between input and output.

Rest of the code remains the same.

*Rule 1:* If you have only one statement inside the expression, then you don’t need curly braces. Hence, the above code would look like:

>(String language) -> System.out.println("Welcome to world of " + language + "Programming Language");

It is better than the actual method call. 
As you have only one parameter, you could remove brackets and input parameter type in the input argument which is very brief.

> language -> System.out.println("Welcome to world of " + language + "Programming Language");

Remember till now we have not discussed any reference variable to reference a Lambda expression.
So, Java probably might have provided some new class like Function, but that’s not the case. Java says that to assign a lambda expression you need to have an interface with only one abstract method and the compiler will automatically detect and assign it to that Interface reference variable.
An interface with a single method is known as a Functional Interface. 
Let us now try to modify our code for the problem that we initially solved using interface Welcome.

	public class WelcomeToProgrammingWorld {
		public static void main(String[] args) {
			Welcome welcomeToALanguage =  language -&gt; System.out.println("Welcome to world of " + language + "Programming Language");
			welcomeToALanguage.welcome("Java");
			welcomeToALanguage.welcome("Groovy");
			welcomeToALanguage.welcome("Scala");
		}
	}
	interface Welcome {
		abstract void welcome(String string);
	}

You can see that we haven’t declared any anonymous inner class, but declared only a Lambda expression assigned to Welcome’s reference variable, and output is the same, but the code is super crisp.
Several people on the web, hold an opinion that Lambda expression is an Anonymous inner class which is untrue. Anonymous inner class is an utterly different concept. A simple way to prove this is to print the object welcomeToALanguage’s reference when we are using an inner class and when we are using the lambda expression. The output will be for example;

>java8.lambda.expressions.WelcomeToProgrammingWorld$1@14ae5a5 

for inner class and;

>java8.lambda.expressions.WelcomeToProgrammingWorld$$Lambda$1/1096979270@682a0b20

for the lambda expression.

You can see a significant difference here. Though an anonymous inner class with one method is closer to a lambda expression and these expressions, appear like a syntactic sugar, but there is more underneath. 
One thing is sure that “Lambda expression seamlessly binds itself with the existing Java ecosystem.”
Before winding up this blog, have a look at some examples of Lambda expression:
A lambda expression with no parameters

>() -> System.out.println("Hi!Lambda Expressions is great."); 

A lambda expression with one parameter

>(String s) -> System.out.println("Hi!Lambda Expressions is great."+s);

Another lambda expression with one parameter

>s -> System.out.println("Hi!Lambda Expressions is great."+s);

A lambda expression with 2 parameters (Note parantesis is required for 2 or more params)

>(s1, s2) -> System.out.println("Hi!Lambda Expressions is great." + s1 + s2);

Some of its advantages are enlisted below: 
1. Less boilerplate and more concise code.
2. Enables functional programming. You could read more about Functional programming, Lambda Calculus and Monads to understand Lambda expressions and how these enable functional programming.
3. Binds seamlessly with stream API & existing Java ecosystem where implementation varies for different streams.
Hope, you will now be able to understand a basic use and syntax of Lambda expressions. I would be writing about Type Inference and Java Functional Interface API for Lambda expressions soon. 