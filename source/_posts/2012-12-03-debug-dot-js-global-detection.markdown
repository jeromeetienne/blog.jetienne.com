---
layout: post
title: "Debug.js: Globals Detection"
date: 2012-12-03 07:43
comments: true
published: false
categories: [debugjs]
---

This post is about detecting global variables in javascript and eradicating them :)
This is a first post about [debug.js](https://github.com/jeromeetienne/debug.js),
a library to make javascript easier to debug.
Global Detection is one of the features of this library.
I came up with the idea back in May 2012 at 
[Web Rebels](https://twitter.com/web_rebels) at oslo.
So i was quite enthousiatic and started coding right away in the plane while coming back.
Now, [debug.js](https://github.com/jeromeetienne/debug.js) has now quite a bit of features. 
In future posts, we will details them and explains how it can be usefull in your own code. 

<iframe width="420" height="315" src="http://www.youtube.com/embed/dZzs4q3NFu8" frameborder="0" allowfullscreen></iframe>

<!-- more -->

## Debug.js Overview
[debug.js](https://github.com/jeromeetienne/debug.js) addresses various known issues which makes javascript hard to debug.
For example, [debug.js](https://github.com/jeromeetienne/debug.js) implements strong type checking, 
for object properties and for function parameters. So you can check that a function is 
called the way to expect, with the precise number of parameters, for each parameters you
can specify the allowed types: number, string.. or instance of your own classes 
like ```THREE.Vector3```.

It implements value bound checking, to ensure a value of variable remains in the range 
you expect ([read more](http://en.wikipedia.org/wiki/Bounds_checking)).
It implements NaN checking to be sure your math computation 
doesn't end up with [NaN](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/NaN).
With plain js, it is silently ignored, with [debug.js](https://github.com/jeromeetienne/debug.js), you are warned immediatly. 
Many other feature like that. 

Oh and an *important feature*, 
[debug.js](https://github.com/jeromeetienne/debug.js)
is javascript. 
It works in browser and node.js.
It isnt another language which is compiled to javascript.
You can keep your current code, your current developpers and their current knowledge.
Now that you got an better idea of what is [debug.js](https://github.com/jeromeetienne/debug.js).
Let's go deeper on a specific part: the global detection


# Globals Detection 

Ok, so let's detect some globals.
You first need to get the files which contains the code.
As for the rest of debug.js, the global detection works in browsers and in node.js.
If you are in node.js, you do the usual ```require()```
(see [more](http://nodejs.org/api/globals.html#globals_require)).

```javascript
var GlobalDetector = require('globaldetector.js');
```

If you are in a browser, just add a ```<script>```

```html
<script src='debug-bundle.js'></script>
```

Now that we got the code, let's get started!

## Start Detecting Globals
The principle behind global detection is simple. 
The global object is monitored, so ```window``` in a browser and ```global``` in node.js.
The monitoring is tuned to detect any new property attached to it.
So any new global defined will be attached to the global object and detected on the next run 
the global detector.
To start the global detector, just the following lines. 

```javascript
// instanciate the object
var globalDetector = new GlobalDetector();
// start monitoring globals 
globalDetector.start();
```

By defaults, it monitor the globals once per second
and report any new ones via ```console.warn()```.
If you wish another behavior, see the [API documentation](http://jeromeetienne.github.com/debug.js/docs/jsdocs/)
and tune the parameters. 
If the variable ```foo``` is detected, you will see something like that.

```
Thu Dec 06 2012 19:10:15 GMT+0100 (CET) -- Warning Global Detected!!! window['foo'] === bar 
```

## How To Allow Globals
Nevertheless, sometime it is ok to have globals.
For example, if you use [jQuery](http://example.com), a ```jQuery``` global will be defined.
No need for GlobalDetector to report this one. 
Just use the following line to warn the library to ignore this particular property.

```javascript
GlobalDetector.ignoreList.push('jQuery');
```

# Globals Removal
To detect the presence and name of globals variables is nice, but it doesn't fix the issue.
The globals are still here.
debug.js goes further and helps you track where those globals are used.
Thus you can change your code to avoid global and be sure you don't miss any spot.

Tracking the usage of globals is a 2 steps process: you have to launch the application twice.
First you detect the globals as described above and you generate the code needed
for the usage tracker, then second, you relaunch to collect where those globals are used.

## Step 1: Generating the Code Needed for Usage Tracker

So on the first run, you include and start the global detector as described above.
You let it run for a while, and then you generate the code for the usage tracker.
Just use the follow line.
It will dump the code you need to include in the second pass.
It is as simple as that.

```javascript
globalDetector.usageTrackerCodeConsole();
```

This one will output the code in the javascript console. 
If you want to output the code in another window, just use ```.usageTrackerCodeWindow()```.
Include this tracking code in your application, and you are all
set for the second pass.

```javascript
PropertyAttr.define(window, 'foo').trackUsage('window.foo');
```

## Step 2: Actually Tracking Usage of Globals

So on the second run, you included the tracking code. 
So you let your application run, and the usage location of each global will be recorded.
To know where your globals are used, just use this line in your javascript console.

```javascript
globalDetector.usageTrackerDump();
```

It will dump where it is used and how many time. 
You will see something quite similar to the following.
It is possible to fine tune the report, see ```Stacktrace.Tracker``` in the
[API documentation](http://jeromeetienne.github.com/debug.js/docs/jsdocs/).

```
window.foo: total 2 times
	aClass.aMethod@http://example.com/index.html:30 - 1 times
	aClass.anotherMethod@http://example.com/index.html:32 - 1 times 
```

So you got where the globals are used and can start fixing your code to remove 
the globals.

## Why global are bad

Wait why this is needed ? A legitimate question to ask. 
So Why global are bad ?
Because **Globals are considered harmfull** by most people and it is so any language. 
Globals are bad in javascript, global are bad in C++, globals are bad in perl etc... 
You got the picture.
Why that ? Because globals got:

**Non-locality** Source code is easiest to understand when the scope of its individual elements are limited. Global variables can be read or modified by any part of the program, making it difficult to remember or reason about every possible use.

**No Access Control** A global variable can be get or set by any part of the program, and any rules regarding its use can be easily broken or forgotten. (In other words, get/set accessors are generally preferable over direct data access, and this is even more so for global data.)

**Implicit coupling** A program with many global variables often has tight couplings between some of those variables, and couplings between variables and functions. Grouping coupled items into cohesive units usually leads to better programs.

**Namespace pollution** Global names are available everywhere. You may unknowingly end up using a global when you think you are using a local (by misspelling or forgetting to declare the local) or vice versa.

Those rules are quoted directly from a very good paper on this exact subject 
"[why globals are bad](http://c2.com/cgi/wiki?GlobalVariablesAreBad)".

## Conclusion

This post is the first on 
[debug.js](https://github.com/jeromeetienne/debug.js).
It explains a specific part of it
: the global detection.
We first explained how to detect the presence of globals, and their names. 
Then we detailed how to track their usage to better remove them.
So with [debug.js](https://github.com/jeromeetienne/debug.js)
, you can find out the globals used in your application and then remove it.

We think this feature is quite important when you have a large 
codebase and need to maintain it over a long time, like 
it is usual in software companies.
In this case, it is best to monitor such a thing and keep it under control.
Companies got to have processes for that... things people call
[Quality Assurance or Q/A](http://en.wikipedia.org/wiki/Quality_assurance).
debug.js and its global detection has been designed to fit nicely in a Q/A process.

In our next posts about debug.js, we will see other important parts of the library,
such as
how to reduce garbage collection via monitoring+object pooling,
how to have private/public members in your javascript classes,
or 
strong type checking.
Additionaly, debug.js works real well when coupled with jsdoc information.
Stay tuned.

That's all folks, have fun :)







