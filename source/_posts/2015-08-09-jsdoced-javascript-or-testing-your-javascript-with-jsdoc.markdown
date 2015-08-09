---
layout: post
title: "Testing your Javascript with jsdoc with jsdoced javascript"
date: 2015-08-09 15:21
comments: true
categories: 
published: false
---

### Start with the problem

Sometimes Javascript may be frustrating, specially when it comes to type checking or private visibility. 

Here is a example that happened to me last week. I had a large browser application, around 30 000 lines of Javascript. I wanted to change the way a given function is called from ```myFunction(a,b)``` to ```myFunction(a,b,c)```. I scanned the code and updated all the calls of ```myFunction()``` I could find.
But it was such a large piece of code, I wasn't certain... Javascript is so dynamic! Jsdoced.js helped in this case by making sure for me. 

Another issue I frequently see is respecting private visibility. Suppose you want to refactor a given class... You will take care to respect its public API, thus other parts of your code don't have to be changed. But, for your private properties or functions, no such requirement exists. You would be able to refactor your code faster if you knew which functions are public and which ones are private.

Javascript doesn't support this feature unfortunately. But jsdoced.js can handle that for you. Jsdoced.js will make sure that private functions remain private.

When you compile, you can choose to privatize your classes. It will apply the convention *"any name starting with underscore is private"*, e.g. ```this._color``` is a private property. It is a usual convention you can [find in python](http://www.diveintopython.net/object_oriented_framework/private_functions.html) and others. Thus when a function is written to be private, and somebody is calling it from outside this class, you will be warned immediately.

Issues like these bothered me so much that I started working on them. First, I produced [Better.js](http://betterjs.org), a library to do type checking and private visibility in javascript. It worked well but it required modifying your source by hand to experience the benefits. This quickly became clumsy. In order to scale, and to be usable on very large source base, I thought about using jsdoc to produce better.js code...Jsdoced.js compiler was born.

---

### Possible Solutions 

#### About others creating non standard languages 
Before starting to work on the compiler, I looked around for other solutions. Some people are designing new languages in order to fix the problems of Javascript. There is Dart by Google, TypeScript by Microsoft and so on. I am not saying that those specific languages are bad. I just don't like the idea of designing a new language. It means that people will have to learn this specific language, and be trained to do so. This costs time and money. Plus, all your tools will need to be reimplemented for this specific language. The interaction with your existing code base may not be obvious. Most of all, those languages are not standard. If you code in Dart, Google may stop maintaining it, then you are in trouble. The same thing could happen with TypeScript and Microsoft. With Javascript, you don't have this issue. It is a standard and will always be implemented because of that.
Javascript is the most used language in the world. This is not because it is great and perfect. Javascript is [not perfect](http://wtfjs.com/) Javascript is the most used language in the world because this is an open standard. This value resonated in me. 

#### How do we choose to fix the problem using Javascript ?
So I tried to figure out how to fix the problems of Javascript without creating a new language. For example, how we can say that a given property or argument is of this type or of this other type? Or, how we can say that this function is private to this class or if it is public? We are going to start by introducing jsdoc and explaining how it will help us figure it out. 

---

### Introducing JSdoced Javascript 

#### Using jsdoc 
I looked at jsdoc with interest. You know the type of small comment that you see above functions like this one:

```
/**
 * measure the length of a string
 *
 * @param  {String} aString the string to measure
 * @return {Number} the length of the string
 */
var stringLength  = function(aString){
    return aString.length
}
```

For details, see [usejsdoc](http://usejsdoc.org/) and [jsdoc3](https://github.com/jsdoc3/jsdoc). It already contains plenty of information about your code.  It got the type of each arguments with [@params](http://usejsdoc.org/tags-param.html). It shows which function is a constructor with [@class tag](http://usejsdoc.org/tags-class.html) or which function is private with [@private tag](http://usejsdoc.org/tags-private.html) or [@access tag](http://usejsdoc.org/tags-access.html). It can even define the type of the return value with [@returns tag](http://usejsdoc.org/tags-returns.html). 

So jsdoc already provides all the information that we need to fix our problem. Best of all, jsdoc is already used, so we don't need to design a new language for it. Jsdoc is the standard way to document your Javascript at the moment. So why not use it?

So we got the founding principle of Jsdoced Javascript : "Take the advantages of jsdoc to improve Javascript". Let's take a closer look.

#### What is JSdoced Javascript? 
As we have seen, JSdoced Javascript was born from a mariage between jsdoc - the defacto standard to document Javascript, and Better.js - a Javascript library that improves the language by enabling features like strong typing and private visibility. So jsdoc has very valuable information about the code source, and Better.js uses it to actually test the code during execution. We put the 2 of them together and the result is JSdoced Javascript.

JSdoced Javascript or ```jsdoced.js``` is a new way to interpret current javascript. It is a way to test your javascript code with jsdoc. ```jsdoced.js``` will automatically extract jsdoc from your Javascript files, produce a Better.js version based on your jsdoc and run this improved version. 

---

### About the better.js workflow 
Now let's see how to actually include it and use it in a real project.  
First, we need a workflow to integrate jsdoced.js into our existing project, to know where and when to use jsdoced.js in it.

For that we use a workflow named *better.js cache directory*. In short, this workflow takes a cache directory, says ```.betterjs``` and puts all compiled files within this directory. It uses the same folder structures as the source files, as shown below

```
myproject/foo/bar.js           # your javascript files
myproject/.betterjs/foo/bar.js # the compiled files
```

So ```/.betterjs/foo/bar.js``` will be the compiled version of ```/foo/bar.js```. [better.js cache directory](https://github.com/jeromeetienne/jsdoced.js/blob/master/WORKFLOW.md) has been designed to be non-obstrusive. Note that this workflow is not mandatory to use jsdoced.js compiler. You may use this one or your own, it is totally up to you.

### When to run jsdoced.js ? 
Now you know how to easily integrate ```jsdoced.js``` in your project. Software writing is usually split into 3 phases: development, test and production. Lets see when to run jsdoced.js in this context. 

Because the goal of jsdoced.js is to test your code with jsdoc, it is run during the test phase. That way we are sure that if the test passed, there are no jsdoc errors in our code. You can even automatize that to run jsdoced.js compilation at every pull request or even commit. Usually it is run during development as well, thus the developer detects those errors immediately and can fix them while they are still fresh in his mind.

Be sure not to run it in production tho, all the checks done by jsdoced.js got some performance cost.

---

### How to install jsdoced.js ?

Jsdoced.js compiler is written node.js so it is distributed via [npm](http://npmjs.com). To install it, simply use:

```
$ sudo npm install -g jsdoced.js
```
---

### How to use jsdoced.js to compile a file ?

If you want to compile a given Javascript file, just use the following command

```
$ jsdoced.js yourFile.js -o yourFile.jsdoced.js 
```

jsdoced.js compiler has a lot of option. Use ```-h``` to get the full list of options. Here are some interesting ones:

- Use ```-d dirname``` to generate inside a [better.js cache directory](https://github.com/jeromeetienne/jsdoced.js/blob/master/WORKFLOW.md)
- Use ```-s``` to treat anything undefined in jsdoc as a requirement e.g. not specifying @return is handled as ```@returns undefined```. The same for ```@params```
- Use ```--log``` to display informations during the compilation.

---

### Conclusion 

#### Benefits of using JSdoced Javascript

In this post we talked about JSdoced Javacript and how it can improve the major problems in Javascript. We have seen that there are a lot of benefits to using it. Let's review them: 

First, there is no need to learn a new language, which takes time, and if you have to train your whole team, costs money. This gives it a major advantage. You can use your existing code and your existing doc. Since it is 100% javascript the integration is easy and smooth. And, it runs on browser and node.js 

As you know, Javascript is a dynamic language, so it has to be tested execution. JSdoced ensures that these tests are done when they need to be. It also helps you expand test coverage. Because jsdoc provides plenty of useful information you can use it to test your code and even detect a new family of errors. For example, ut can detect typing errors in Javascript and it can also detect private errors: Unauthorised private access is detected immediately. 

So documenting your code becomes a clear advantage. It makes your code safer. It makes sure that it is well tested, and that all the information that you need is in order. 
