---
layout: post
title: 'Type Inference in Lambda Expressions : Java 8'
categories: [Programming]
tags : [Java 8, Lambda Expressions, Type Inference]
comments: true
---

Type Inference means that the data type of any expression (e.g. method return type or parameter type) can be deduced automatically by the compiler. Groovy language is a good example of programming languages supporting Type Inference.

Similarly, Java 8 Lambda expressions also support Type inference. Let’s understand how it works with a few examples.

Suppose we have to calculate the sum of two numbers. As you know, we need an interface method that consumes two parameters of say Number type and returns the output of the same type. Let’s create an interface and provide its inline implementation using Lambda expressions.


{% highlight java linenos %}
    public class LambdaTypeInferenceExample {
    public static void main(String[] args) {
            BiFunction<Integer,Integer,Integer> summer = (Integer number1,Integer number2) ->  { return number1 + number2;};
     
            Integer number1 = 10;
            Integer number2 = 20;
     
            System.out.println(number1+" + "+number2 +" = "+summer.apply(number1,number2));
        }
    }
 
    interface BiFunction<K,V,R>{
        R apply(K k ,V v);
    }
{% endhighlight %}

Observe that we have declared an interface called BiFunction which has a method, ‘apply’ accepting two parameters of any type K & V respectively and returns another type of value i.e. R. These types could be same or different as you provide their types during declaration.Here, we are summing up two integers and returning the output as an integer. For now, don’t worry about any edge cases like sum of the maximum of two integers can’t be equal to the integer as it exceeds the maximum limit. By using type inference feature, you could omit, the type of number1 and number2. The compiler would now automatically deduce the type, on the basis of input values as:

Here, we are summing up two integers and returning the output as an integer. For now, don’t worry about any edge cases like sum of the maximum of two integers can’t be equal to the integer as it exceeds the maximum limit. By using type inference feature, you could omit, the type of number1 and number2. The compiler would now automatically deduce the type, on the basis of input values as:

1. If you haven’t specified generic types during reference variable declaration, type of number1 and number2 would be deduced as Object class instances.
2. If specified the generic types, it will automatically deduce the type and operation available for that type could be used.
The above code thus looks crisper after removing types from parameters:

{% highlight java linenos %}
  public class LambdaTypeInferenceExample {
      public static void main(String[] args) {
          BiFunction<Integer,Integer,Integer> summer = (number1,number2) ->  { return number1 + number2;};
          Integer number1 = 10;
          Integer number2 = 20;
          System.out.println(number1+" + "+number2 +" = "+summer.apply(number1,number2));
      }
  }
   
  interface BiFunction<K,V,R>{
      R apply(K k , V v);
  }
{% endhighlight %}

This is an example of type inference here. Let’s go one more step ahead. We are saying return the following:

{% highlight java %}
 number1 + number 2
{% endhighlight %}

Here, we could omit ‘return from statement’ as there is only one statement and hence could easily be inferred by the compiler. As you know, if there is only one statement, we don’t need parenthesis around lambda expression statements.

Hence, the above code will finally look like:

{% highlight java linenos %}
    public class LambdaTypeInferenceExample {
        public static void main(String[] args) {
            BiFunction<Integer,Integer,Integer> summer = (number1,number2) ->  number1 + number2;
            Integer number1 = 10;
            Integer number2 = 20;
            System.out.println(number1+" + "+number2 +" = "+summer.apply(number1,number2));
        }
    }
     
    interface BiFunction<K,V,R>{
        R apply(K k , V v);
    }
{% endhighlight %}

Hope you now have an understanding of how Type Inference works in lambda expressions. Though Java supports this, it is still a strict type-checking language.