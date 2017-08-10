---
layout: post
title: 'Generic Functional Interfaces and Functional Interface API for Lambda Expressions: Java 8'
categories: [Programming, Functional-Programming]
tags: [Java-8, Generic-Functional-Interfaces, Functional-Interface-API]
comments: true
---

Lambda expressions come with a standard rule that they can only be assigned to a reference variable which is a Functional Interface i.e. an interface having one and only one abstract method.

Besides, a Lambda expression does not care about the name of a function and/ or any access specifier as it tries to infer this information from the abstract method itself.

The purpose of stating the above is that if we use generics in abstract methods, we could have generic functional contracts. Generic functional interfaces allow declaring a similar type of functions having different implementations (think about a generic class which could accept any parameter type or an instance variable.)

Suppose you want to write a program to add two integers of types double and float or two string or even two objects which could be same or different. (Taking the sum of integer and string into consideration as of now.) A very traditional way could be creating overloaded methods for each implementation. Another could be to create an interface and provide its multiple implementations using inner classes as shown below:

	public class GenericFunctionalInterface<T,U,V> {
		public static void main(String[] args) {
	 
			Sum<Integer,Integer,Long> addTwoIntegers  = new Sum<Integer, Integer, Long>() {
				@Override
				public Long sum(Integer number1, Integer number2) {
					return number1 + number2;
				}
			};
	 
			Sum<String,String,String> addTwoStrings  = new Sum<String, String, String>() {
				@Override
				public String sum(String string1, String string2) {
					return string1.concat(string2);
				}
			};
	 
			System.out.println(addTwoIntegers.sum(10,30));
			System.out.println(addTwoStrings.sum("Hello!","World"));
	 
		}
	}
 
	interface Sum<T,U,V>{
		 abstract V sum(T number1, U number2);
	}
	
We can refine the above piece of code by converting the anonymous inner class implementations into Lambda expressions as:


	public class GenericFunctionalInterface<T, U, V> {
		public static void main(String[] args) {
			Sum<Integer, Integer, Long> addTwoIntegers = (number1, number2) -> number1.longValue() + number2;
			Sum<String, String, String> addTwoStrings = (string1, string2) -> string1.concat(string2);
			System.out.println(addTwoIntegers.sum(10,30));
			System.out.println(addTwoStrings.sum("Hello!","World"));
		}
	}
	 
	interface Sum<T, U, V> {
		abstract V sum(T number1, U number2);
	}

Both the ways (using an anonymous inner class or Lambda expressions) of creating different implementations work fine. The latter being a representation of the Generic functional interface that could have any number of implementations.
However, the question is, why do we need a new functional interface, Sum? Can’t we use any existing interface having only one abstract method? For sure, we could think about having a package containing such possible interfaces.
Java 8 understands this need and tries to provide most generic implementations under the java.util.function package which lies under rt.jar. Refer the screenshot below:

![_config.yml]({{ site.baseurl }}/images/2017-7-28-fi-api-generic-fi/rt.jar.png)


You can notice that this package contains many functional interfaces which we can use on a regular basis in the codebase. Hence, it is recommended, to first look up this package for an existing functional interface. The Java compiler provides with these interfaces in advance to avoid unnecessary duplication/creation.

Here is a brief description of a few existing functional interfaces.

First one in my list is a BiConsumer. As the name suggests it takes two parameters of any type and just consumes and not produce any return data which is evident from its declaration as below:
	
	/*
	 * Copyright (c) 2012, 2013, Oracle and/or its affiliates. All rights reserved.
	 * ORACLE PROPRIETARY/CONFIDENTIAL. Use is subject to license terms.
	 */
	package java.util.function;
	 
	import java.util.Objects;
	 
	/**
	 * Represents an operation that accepts two input arguments and returns no
	 * result.  This is the two-arity specialization of {@link Consumer}.
	 * Unlike most other functional interfaces, {@code BiConsumer} is expected
	 * to operate via side-effects.
	 *
	 * <p>This is a <a href="package-summary.html">functional interface</a>
	 * whose functional method is {@link #accept(Object, Object)}.
	 *
	 * @param <T> the type of the first argument to the operation
	 * @param <U> the type of the second argument to the operation
	 *
	 * @see Consumer
	 * @since 1.8
	 */
	@FunctionalInterface
	public interface BiConsumer<T, U> {
	 
		/**
		 * Performs this operation on the given arguments.
		 *
		 * @param t the first input argument
		 * @param u the second input argument
		 */
		void accept(T t, U u);
	 
		/**
		 * Returns a composed {@code BiConsumer} that performs, in sequence, this
		 * operation followed by the {@code after} operation. If performing either
		 * operation throws an exception, it is relayed to the caller of the
		 * composed operation.  If performing this operation throws an exception,
		 * the {@code after} operation will not be performed.
		 *
		 * @param after the operation to perform after this operation
		 * @return a composed {@code BiConsumer} that performs in sequence this
		 * operation followed by the {@code after} operation
		 * @throws NullPointerException if {@code after} is null
		 */
		default BiConsumer<T, U> andThen(BiConsumer<? super T, ? super U> after) {
			Objects.requireNonNull(after);
	 
			return (l, r) -> {
				accept(l, r);
				after.accept(l, r);
			};
		}
	}

Suppose we need to concatenate two strings and then print the resultant string. Here’s how we can implement the above:

	import java.util.function.BiConsumer;
	 
	public class UsingBiConsumer {
		public static void main(String[] args) {
			BiConsumer printer = (s1, s2) -> System.out.println(s1 + " " + s2);
			printer.accept("Hello!","Functional Interface API");
		}
	}

This code is as crisp as one could write using the existing functional interfaces.

__BiFunction__ is another interface similar to BiConsumer with the only difference that it returns.

__Consumer__, as is apparent, takes only one parameter and performs some operations without returning anything.

__Function__ is similar to Consumer but returns a value of some generic type.

Another important functional interface is __Supplier__. As the name suggests, it accepts no parameter but returns a value of some type.

Below is an example of its implementation:

	import java.util.function.Supplier;
	public class SupplierImplementation {
		public static void main(String[] args) {
			Supplier ageSupplier = ()-> 20;
			System.out.println("Minimum age for applying for this job is:"+ageSupplier.get());
		}
	}

__Predicate__ is an interface that takes an input of some type and always outputs either true or false. Hence, the name.

Remember to be __DRY__ (Don’t Repeat Yourself) and check the java.util.function package for any existing functional interface. Create a new one only when it is not available.

Hope it helps. My recommendation is that you go through java.util.function package once to see all available functional interfaces.
