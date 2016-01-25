---
layout: post
title:  "JavaScript decorators aren't metadata but they're still useful!"
date:   2016-01-28 17:30:00 +0100
categories: javascript
excerpt: "When you check some ESNext code and you see a decorator, if you're a C# or Java developer, you might think that finally meta-programming has arrived to JavaScript. Actually not. But decorators are still very useful!"
---

JavaScript is evolving very fast. Newer ECMA-Script specifications have finally added class-based object-oriented programming (sadly, I don't see any intention to implement actual encapsulation with access modifiers like `private` or `public`...).

While class-based object-oriented programming is just a layer on top of regular ECMA-Script 5.x (I mean *classes* are just syntactic sugar over regular prototypal inheritance), there's still no way to extend the language using metadata programming like you would find in C#:
	
    // Attributes in C# let you introspect types during run-time to 
    // perform actions based on them or they can be used to configure 
    // convention engines and many other use cases...
	[Serializable]
	public class A {}

Some time ago I checked that JavaScript had a new feature called *decorators* and I thought they were like C# attributes. Just check the following code:

	@serializable
	class A {
	}

It seems some way inspired by Java annotations... But they're absolutely different *monsters*.

### So what's a decorator in JavaScript?

It's just a declarative way of adding properties and behavior to both prototypes, constructors or even object functions.

They're implemented as follows:

	// In ECMA-Script 6 and above using module syntax...
	export const serializable = function() {
		return function(ctor) {
			ctor.prototype.serializable = true;
		};
	};

What does this mean? Well, when you apply the whole decorator, a property `serializable` will be added to constructor's prototype because you do it manually (*see the sentence `ctor.prototype.serializable = true;` in the above code sample*).

You can also implement decorators with parameters:


	export const serializable = function(format) {
		return function(ctor) {
			ctor.prototype.serializable = true;
			ctor.prototype.serializationFormat = format;
		};
	};

And your class code would look as follows:

	@serializable("json")
  	class A {
	}

Actually, what's the difference with doing it without decorators? See how the code would look like:

	class A {
		constructor() {
			// It's not being added to the prototype, but it's almost the same as
			// using the "serializable" decorator, isn't it?
			this.serializable = true;
			this.serializationFormat = true;
		}
	}

#### You can also decorate properties/functions

Decorators can also decorate properties and functions. For example:

	export const readOnly = function(target, key, descriptor) {
		descriptor.writable = false;
	}

	class A {
		@readOnly
		text = "hello world";
		
		@readOnly
		doStuff() { 
		}
	}

Now if you try to reuse the `a` and `doStuff` identifier, your Web browser or JavaScript runtime will throw an error:

	var a = new A();
	a.text = "hey"; // ERROR
	a.doStuff = () => console.log("whaaat?"); // ERROR

IMHO, there's a big limitation here: **a decorator can't access some function `this` value, thus, when you decorate a function, you're just changing its descriptor, but you can't inject things to `this` or its scope.**

### Decorators are just what their name says: they decorate functions and properties 

Even when they've some limitations, don't hesitate to learn and use them, since declarative programming is easier to understand and your code will be simpler. 

Just a final and very very simple example. Let's say you're developing an UI view engine and you need to know which view is the *main one*:

	export const mainView = function() {
		return function(ctor) {
			ctor.isMainView = true;
		};
	};

	@mainView
	class MyView {

	}

Now you can check what view is the main one easily. If you've an array of views...:

	var views = [new MyView(), new MyView2(), new MyView3()];

...you can obtain the main view using `array.filter`:

	var results = views.filter(view => view.isMainView === true);
	var mainView = null;

	if(results.length == 1)
		mainView = results[0];