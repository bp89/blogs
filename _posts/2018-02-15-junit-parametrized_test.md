---
layout: post
title: Writing Parametrized test cases using junit-5
categories: [Programming]
tags: [Java, Junit-5,Unit-testing]
comments: true
---

Hope you a little bit familiar with how junit works. How you write a test case using it. Given that as a developer you should definitely have faced situations
where you felt like repeating something inside the test cases. 

If you have felt that and also felt you are cluttering your test file with lots of boiler plate code
and loosing focus from actually testing and giving upto writing a test case.

Think about you just have a matrix of data and you are just writing multiple test cases for covering this matrix. Well junit saves you by giving parametrized
test cases.

You simply need to use `Parametrized` runner for your test case and plugin few nuts and bolts as below sample test class:

	package com.softiventure.dtos;
	
	import org.junit.Test;
	import org.junit.runner.RunWith;
	import org.junit.runners.Parameterized;
	
	import java.util.Arrays;
	import java.util.Collection;
	
	import static org.junit.Assert.assertEquals;
	
	@RunWith(Parameterized.class)
	public class ParametrizedTest {
	
		@Parameterized.Parameter(0)
		public int param1;
	
		@Parameterized.Parameter(1)
		public int param2;
	
		@Parameterized.Parameter(2)
		public int output;
	
		@Parameterized.Parameters(name = "{index} : sum({0},{1})={2}")
		public static Collection<Object[]> parameters() {
			return Arrays.asList(new Object[][]{
					{1, 2, 3},
					{1, 4, 5}
			});
		}
	
		@Test
		public void testSum() {
	
			assertEquals(output, param1 + param2);
		}
	
	}

Let's understand what actually happened:

1. `@RunWith(Parameterized.class)` helps junit in deciding how the test cases need to be run.
2. `parameters()` method created a collection of parameters needed. Observe how we created the list from array of objects to achieve required input and output parameters.
3. `@Parameterized.Parameters(name = "{index} : sum({0},{1})={2}")` annotation tells junit to pick the parameters collection from `parameters` method.
4. `@Parameterized.Parameter(0)` sets 0th index array value to param1. Similar for rest of the fields. **Note:** these should be public otherwise junit fails to populate the field values.
Also, if the index is not specified then by default index 0 is taken by this annotation.


Also, #3 has name attribute for annotation which actually is output when the test is run `{}` is used to specify parameters for which the value needs to be printed.

Now run the test cases. It will run 2 test cases respectively and output would be something like below:-

	0 : sum(1,2)=3
	1 : sum(1,4)=5

Cheers! Hope it helps!







