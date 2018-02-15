---
layout: post
title: Writing multiple Parametrized test cases using junit
categories: [Programming]
tags: [Java, Junit,Unit-testing]
comments: true
---

As you already got some knowledge of junit, I would straight ahead move to how to create more than Parametrized test cases
 in junit when you have more than one methods of same class to be tested.
So, lets take a simple example, `you have to check sum of two numbers` and `you have to check substraction of two numbers`. 
Both the methods to do this operation are in same class.


	public class CalculatorService{
	
		public Integer sum(Integer param1, Integer param2){
			return param1 + param2;
		}
		
		public Integer substract(Integer param1, Integer param2){
			return param1 - param2;
		}
	
	}

Now, if you have to write parametrized test case you can only write one such test case in a class. That's a restriction of junit.

Hence, Junit provides a Runner named `Enclosed`. This runner allows nested classes to have the test cases and you could easily run those.

	import org.junit.Assert;
	import org.junit.Test;
	import org.junit.experimental.runners.Enclosed;
	import org.junit.runner.RunWith;
	import org.junit.runners.Parameterized;
	
	import java.util.Arrays;
	import java.util.Collection;
	
	@RunWith(Enclosed.class)
	public class NestedParametrizedTest {
	
		@InjectMocks
		private static CalculatorService calculatorService;
		
		@RunWith(Parameterized.class)
		public static class sumTest {
	
			@Parameterized.Parameter
			public int arg1;
	
			@Parameterized.Parameter(1)
			public int arg2;
			@Parameterized.Parameter(2)
			public int output;
	
			@Parameterized.Parameters(name = "{index}:sum({0},{1})={2}")
			public static Collection parameters() {
				return Arrays.asList(new Object[][]{
						{1, 2, 3},
						{2, 2, 4}
				});
			}
	
			@Test
			public void sum() {
				Assert.assertEquals(output, calculatorService.sum(arg1, arg2));
			}
		}
		
		 @RunWith(Parameterized.class)
			public static class sumTest {
		
				@Parameterized.Parameter
				public int arg1;
		
				@Parameterized.Parameter(1)
				public int arg2;
				@Parameterized.Parameter(2)
				public int output;
		
				@Parameterized.Parameters(name = "{index}:sum({0},{1})={2}")
				public static Collection parameters() {
					return Arrays.asList(new Object[][]{
							{5, 2, 3},
							{6, 2, 4}
					});
				}
		
				@Test
				public void sum() {
					Assert.assertEquals(output, calculatorService.substract(arg1, arg2));
				}
			}
	}


Here notice the top level class in having `RunWith(Enclosed.class)` annotation. This ensures that it goes to every inner class and runs the test cases.

Hope it helps!







