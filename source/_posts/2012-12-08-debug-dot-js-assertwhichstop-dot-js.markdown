---
layout: post
title: "Debug.js: assertWhichStop.js"
date: 2012-12-08 15:15
comments: true
published: true
categories: [debugjs]
---

This is post presents *assertWhichStop.js*.
[assertWhichStop.js](https://github.com/jeromeetienne/debug.js/tree/master/src)
is a simple library which provide an ```assert()``` which stops... 
who would have guessed :)
You may find that surprising but in your browser,
javascript's 
[assert()](https://getfirebug.com/wiki/index.php/Console_API#console.assert.28expression.5B.2C_object.2C_....5D.29)
does not stop the execution of the code but it does in other languages.
[assertWhichStop.js](https://github.com/jeromeetienne/debug.js/tree/master/src)
will make your assert stop even in your browser.

[assertWhichStop.js](https://github.com/jeromeetienne/debug.js/tree/master/src)
is a part of 
[debug.js](https://github.com/jeromeetienne/debug.js) library,
which we talked about in [previous posts](/blog/categories/debugjs/).
This post will explain why it is usefull to stop and how to use *assertWhichStop.js*.
This library is 
[very small](https://github.com/jeromeetienne/debug.js/blob/master/src/assertwhichstop.js)
, barely 10 lines full total at the time of writing.
It is based on a little
[gist](https://gist.github.com/2651899)
we did in collaboration with
[jens arp](https://plus.google.com/118110542957297456336/posts). 
Ok now let's see more deeply what is assert.

<!-- more -->


### What Is Assert() ?

[Assert](http://en.wikipedia.org/wiki/Assertion_\(computing\))
is a very usefull tool during developement.
It is done to **explicitly check internal consistency**.
In other words, it is a way to say *"ok here, this MUST be true. And if it isn't, there is a bug"*.
You explicitly describe the assertions you make in your code
and 
get immediatly notified when one happened to be wrong.
So you can fix the bug.

[Assert](http://en.wikipedia.org/wiki/Assertion_\(computing\))
is from
[c](http://en.wikipedia.org/wiki/C_\(programming_language\))
/
[c++](http://en.wikipedia.org/wiki/C%2B%2B).
It is available in many languages: in
[python](http://docs.python.org/3.3/reference/simple_stmts.html#the-assert-statement),
[php](http://php.net/manual/en/function.assert.php),
or even done in 
[perl](http://search.cpan.org/~mschwern/Carp-Assert-0.20/lib/Carp/Assert.pm).
In most language, the execution is stopped immediatly: thru exception for
[python](http://docs.python.org/3.3/reference/simple_stmts.html#the-assert-statement)
or
[php](http://php.net/manual/en/function.assert.php)
, or thru 
[unix signal](http://en.wikipedia.org/wiki/Unix_signal)
+ 
[coredump](http://en.wikipedia.org/wiki/Core_dump)
with
[c](http://en.wikipedia.org/wiki/C_\(programming_language\))
/
[c++](http://en.wikipedia.org/wiki/C%2B%2B).
Thus you get notified of the bugs early,
study the situation and start fixing.


### Let's Get Started
You first need to get the files which contains the code.
As for the rest of debug.js, the global detection works in browsers and in node.js.
If you are in node.js, you do the usual ```require()```
(see [more](http://nodejs.org/api/globals.html#globals_require)).

```javascript
var GlobalDetector = require('assertwhichstop.js');
```

If you are in a browser, just add a ```<script>```

```html
<script src='debug-bundle.js'></script>
```

### How To Use It ?

Now that we got the code included, let's see how to use it.
The typical usage will be like that

```javascript
assertWhichStop(condition, message);
```

If the asserted condition is true, nothing will happen.
If it is false, the message will be displayed and it will try to stop the execution of the programm.
Suppose you got a variable ```foo``` which is supposed to always be a number greater than 0.
You will typically do something like that.

```javascript
assertWhichStop( foo > 0, "foo MUST be greater than 0");
```

This will stop the execution if ```foo``` isn't greater than 0. Simple enougth :)
Sure but how does it try to stop ? Let's look at that.

### How To Stop ?

assertWhichStop can stop in 2 ways:
The first one is by throwing an exception with [Error](http://example.com/mdn)
It is the most natural way. 
So it is the default behavior. 

Additionnaly it is possible to stop by triggering the js debugger itself.
This generate a total halt. Nothing will happen anymore in the page.
It uses [```debugger;``` javascript statement](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Statements/debugger)
It isn't a well known part of javascript but it is
[standard javascript](http://www.ecma-international.org/ecma-262/5.1/#sec-12.15).
Unfortunatly it seems to be implemented only on chrome at the moment.
When the virtual machine hits it, it will popup the
[javascript debugger](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Statements/debugger),
so you can get the call stack, get the local variables,
set up conditions to watch or many other things. Usefull stuff to debug.

### Overloading Console API for Backward Compatibility

If you wish, you can overload the ```console.assert``` with the following line.
Thanks to that, you can easily take advantage of
assertWhichStop.js without changing your code, a nice feature :)

```javascript
assertWhichStop.overloadConsole();
```

After this, any ```console.assert()``` will use assertWhichStop and will be able to stop as 
soon as an assertion is found wrong.

## Conclusion
If this post, we talked about 
why assert is usefull during  developement to keep your code running as you expect. 
We have seen 
how to use [assertWhichStop.js](https://github.com/jeromeetienne/debug.js/tree/master/src)
to immediatly stop when one of your assertion happened to be wrong.
Thus then you can fix the bug as soon as possible.
We got various ways to stop: exception or ```debugger;``` statement, a part of standard javascript. 
All that in a backward compatible fashion... rather cool if you ask me :)

That's all folks, have fun!




