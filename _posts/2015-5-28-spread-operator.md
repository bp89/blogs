---
layout: post
title: Spread Operator in groovy
tag:  Groovy, Spread Operator, operators
comments: true
---

The Spread Operator is used to invoke an action on all items of an aggregate object. 
It is equivalent to calling the action on each item and collecting the result into a list:

	class Person {
		String name
		String description
	}
	def persons = [
		   new Person(name: 'RajKumar', description:'Good Person'),
			new Person(name: 'Mahesh', description:'Good Person')]
	def names = persons*.name
	 assert names == ['RajKumar', 'Mahesh']  
  
Here *. is the spread operator. Calling the spread operator on the list, access the name property of each item and returns a list of strings corresponding to the collection of name.

__Good Part:-__

The spread operator is null-safe, meaning that if an element of the collection is null, it will return null instead of throwing a NullPointerException:

The spread operator can be used on any class which implements the Iterable interface:

	class Component {
		Long id
		String name
	}
	class CompositeObject implements Iterable<Component> {
		def components = [
			new Component(id: 1, name: 'Foo'),
			new Component(id: 2, name: 'Bar')]
		@Override
		Iterator<Component> iterator() {
			components.iterator()
		}
	}
	def composite = new CompositeObject()
	assert composite*.id == [1,2]
	assert composite*.name == ['Foo','Bar']

__Spreading method arguments__

There may be situations when the arguments of a method call can be found in a list that you need to adapt to the method arguments. In such situations, you can use the spread operator to call the method. For example, imagine you have the following method signature:

	int function(int x, int y, int z) {
		x*y+z
	}

then if you have the following list:

>def args = [4,5,6]

you can call the method without having to define intermediate variables:

>assert function(*args) == 26

It is even possible to mix normal arguments with spread ones:
	
	args = [4]
	assert function(*args,5,6) == 26
	Spread list elements

When used inside a list literal, the spread operator acts as if the spread element contents were inlined into the list:

	def items = [4,5]                     
	def list = [1,2,3,*items,6]           
	assert list == [1,2,3,4,5,6]           

`items` is a list

we want to insert the contents of the items list directly into list without having to call addAll
the contents of items has been inlined into list

__Spread map elements__``

The spread map operator works in a similar manner as the spread list operator, but for maps. It allows you to inline the contents of a map into another map literal, like in the following example:

	def m1 = [c:3, d:4]                 
	def map = [a:1, b:2, *:m1]           
	assert map == [a:1, b:2, c:3, d:4]    

m1 is the map that we want to inline
we use the *:m1 notation to spread the contents of m1 into map
map contains all the elements of m1

The position of the spread map operator is relevant, like illustrated in the following example:

	def m1 = [c:3, d:4]                 
	def map = [a:1, b:2, *:m1, d: 8]     
	assert map == [a:1, b:2, c:3, d:8]    

m1 is the map that we want to inline

we use the :m1 notation to spread the contents of m1 into map, but redefine the key d *after spreading
map contains all the expected keys, but `d` was redefined.

Hope it helps!!