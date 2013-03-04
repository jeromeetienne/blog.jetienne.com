---
layout: post
title: "console4Worker - console API for worker"
date: 2011-09-12 09:17
published: true
comments: true
categories: 
---

[console4Worker](https://github.com/jeromeetienne/console4Worker) is console API for WebWorkers.
WebWorkers are hard to debug. On top of that, [console API](http://getfirebug.com/wiki/index.php/Console_API)
is unavailable
in WebWorkers. It doesn't help.

[console4Worker](https://github.com/jeromeetienne/console4Worker)
is a simple library which tries to fill this gap.
It provides the [console API](http://getfirebug.com/wiki/index.php/Console_API)
, the same api you are used to.
It implemented in most browser, node.js, even part of 
[commonjs](http://wiki.commonjs.org/wiki/Console).
See the [demo](https://github.com/jeromeetienne/console4Worker/blob/master/examples/index.html)
in the examples/ directory. 

## On the Worker Side

First you include the script with this line.

```javascript
    importScripts('console4Worker-worker.js');
```

And you are **done**! Now you can console API as you would normally do, even if you are in a
[webworker](http://www.whatwg.org/specs/web-apps/current-work/complete/workers.html).
Lets say something like

```javascript
    console.log("console call made from inside a webworker");
```

<!-- more -->

## On the Page Side

On the page side, you need to add 3 lines.
*First* to include the script, simply do

```javascript
    <script src="console4Worker-page.js"></script>
```

*Then* you bind console4Worker to your worker with another line.

```javascript
    // init the worker
    var worker	= new Worker("worker.js");
    // bind the console4Worker to get console API from worker
    console4Worker.bind(worker);  
```

*If* you got your own message event listener, add ```console4Worker.filterEvent()``` in it.
This line will filter out messages coming from console4Worker.
So you should have something like

```javascript
    worker.addEventListener('message', function(event){
        // filter this event if it is from console4Worker
        if( console4Worker.filterEvent(event) )	return;

        // ... here handle your own events
    }, false);
```

## Motivation

Another project of mine is [pacmaze](http://pacmaze.com).
It uses webworkers to provide smoother animations.
This is important for high fps game.
The worst which can happen is to have 50fps most of the time
but with short period at 30fps.
People want the animation to be smooth.

Webworker allows to spread the load on 2 cpu core.
Thus the pick of grabage collection is distributed, and you got twice more
cpu power. So all in all, i found myself spending quite some time debugging webworker, so i
wrote console4Worker to spend less time next time :)

## Conclusion

How does it work ? a console object is declared in the worker. It intercepts
all console calls and pass them to the page thread. a direct approach.
I plan to add stacktrace and thus provide more accurate informations about the
location of the call.

The code is available on [github](https://github.com/jeromeetienne/console4Worker)
under MIT license.
If you hit bugs, fill issues on github. Feel free to fork, modify and have fun with it :)






