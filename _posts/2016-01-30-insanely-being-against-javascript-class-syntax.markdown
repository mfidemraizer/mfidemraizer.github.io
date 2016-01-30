---
layout: post
title:  "Are you insanely against JavaScript class syntax?"
date:   2016-01-30 17:30:00 +0100
categories: javascript
excerpt: "What's behind these JavaScript developers arguing that JavaScript class syntax is a wrong language design decision?"
---


<img src="/img/chair.JPG" style="width: 20vw;height: 20vw; margin: 0 auto;display: block;margin-bottom: 40px">

Actually it's not the first time I've heard about people arguing against JavaScript class syntax. I mean this:

	class SomeClass {
		someProperty = "hello world";

		constructor() {
		}

		doStuff() {

		}
	}

But, why people hate classes? 

Let's summarize the argument against JavaScript class-based object-oriented programming syntax:

> **JavaScript has prototypal inheritance, thus, class syntax is just syntactic sugar for these dumb developers that don't want to learn how JavaScript works**. 

There's an urban legend that tells old-school developers coming from classical languages like C#, Java or C++ (*just to mention 3 of them*) don't want to scratch their minds, and this is the main reason to evolve JavaScript to class-based object-oriented programming.

In my case, I would say that I'm as expert of C# as I'm also on JavaScript, and I've already developed using prototypal inheritance and, unless you use some library to *sugarize* your code, a simple *B inherits A* inheritance might look as follows:

	function A() {}
	
	function B() {}
	B.prototype = Object.create(A.prototype);

First of all, you may argue: *this is how JavaScript works: if you want to take full advantage of language's features, you need to go this way*. But just take a look at the following code and let me know if you don't feel that same thing *looks more readable*:

	class A {}
	class B extends A {}

Other might argue: readability is subjective. It depends on your own background. Absolutely true, **but isn't easier to explain to someone that `B` inherits `A` because the code is self-documented by its own syntax?**

Current generation languages are considered *high-level languages*. That is, **they're ultimately designed to be closer to human language rather than machine language**. Class syntax, after all, was conceived to match what, in human language, we know as *achetype*.

For example, when we look at some chair, do our brain see the concrete chair or...

1. Catalogs/identifies what's being observed as an **object** *(it could be a person, couldn't be?)*.
2. Once our mind know that what's being observed is an *object*, it tries to find a *template* to what's being observed. It has that form, four legs...
3. Yes, it's a chair! 
4. Let's see its details... Yes, it's a *stately chair* (*StatelyChair is Chair*).

Do you really think that the following prototypal reasoning matchs how our mind actually works?

1. I see something called `Chair` and I need to know how to build one, so I define a function to build chairs...
2. Oh, it's a `StatelyChair`! I need to know how to build one, but since it's a chair, I'm going to copy all chair attributes to the concrete stately chair explicitly, and I'm going to copy these attributes to a detail that mixes `Chair` and concrete chair details.

Now let's see a prototypal code based on the chair inheritance:

	function Chair() {
	}
	
	function WoodenChair() {
	}
	WoodenChair.prototype = Object.create(Chair.prototype);
	
	function StatelyChair() {
	}
	StatelyChair.prototype = Object.create(Chair.prototype);

And now let's see a class-based sample code:

	class Chair {}
	class WoodenChair extends Chair {}
	class StatelyChair extends WoodenChair {}

If you would need to teach how to code in JavaScript to newbies in programming, probably they wouldn't understand what's inheritance at all in terms of programming, but you could explain the mind process to identify and object, and later show a code sample based on **classes**, and undoubtely, even my grandma would read the code and she would be able to understand that the whole code is expressing an inheritance.


### But classes are just syntactic sugar: you're not defining types, you're just defining prototypes using an alternate approach (class syntax)

At the end of the day, *who cares?* High-level programming languages like JavaScript are meant to improve your productivity: do more with less. What if you can define a class and inherit it without using class syntax?
	
	class A {}
	
	function B() {}
	B.prototype = Object.create(A.prototype);

Once ECMA-Script 2015 and above have been already implemented in Web browsers and runtimes like NodeJS with full native support and runtimes like NodeJS , **I expect that low-level inheritance (i.e. *that one done using imperative code instead of declarative code*) may be required when you might need to glue a JavaScript framework/library from 2000-2020 with most newer code bases that are going to be exclusively developed using pure class-based syntax**.

There're other *pros* when using *class syntax*: **static code analyzers are going to become more powerful** since classes homogenizes JavaScript code (actual prototypal inheritance can be coded in many ways and it's more error prone than class sytnax, which is developed using an unique and predictable approach).

**And yes: definitively old-school developers entering to JavaScript world may encounter an easy path to migrate a lot of knowledge from other platforms and languages**. Isn't this a *pro* insead of a *con*?

