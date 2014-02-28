---
layout: post
title: "WebAudiox.jsfx.js - 8 bit procedural sound generation with Web Audio API"
date: 2014-02-27 07:43
comments: true
published: true
categories: [webaudiox]
---

Last week we started talking about
[webaudiox](https://github.com/jeromeetienne/webaudiox).
In this
[first post](http://blog.jetienne.com/blog/2014/02/18/webaudiox-a-dry-library-for-webaudio-api/),
we talked about the project and started to explain the helpers ```webaudiox.shim.js```
and ```webaudiox.loadbuffer.js```.
This week we are gonna focus on ```webaudiox.jsfx.js```.
This helper provides you an easy way to use
[jsfx.js](http://www.egonelbre.com/js/jsfx/)
with your Web Audio API games.

<a href='http://jeromeetienne.github.io/webaudiox/examples/jsfx.html' target='_blank'><input type="button" value='Try Demo Now' style='font-size:400%;' /></a>

<iframe width="420" height="315" src="http://www.youtube.com/embed/-Lo433Bk2RY" frameborder="0" allowfullscreen></iframe>

<!-- more -->

## webaudiox.jsfx.js

[jsfx.js](https://github.com/egonelbre/jsfx)
is a library to generate procedural sound, very 8-bit kindof sound.
See [jsfx demo page](http://www.egonelbre.com/js/jsfx/) for details on this fun library
by [@egonelbre](https://twitter.com/egonelbre/).
It is useful because you can generate lots of different sound easily without downloading
anything.

#### Show Don't Tell

* [webaudiox.jsfx.js](https://github.com/jeromeetienne/webaudiox/blob/master/lib/webaudiox.jsfx.js)
the source itself.
* [examples/jsfx.html](http://jeromeetienne.github.io/webaudiox/examples/jsfx.html)
\[[view source](https://github.com/jeromeetienne/webaudiox/blob/master/examples/jsfx.html)\] :
It shows a basic usage of this helper
* [examples/jsfx-basic.html](http://jeromeetienne.github.io/webaudiox/examples/jsfx-basic.html)
\[[view source](https://github.com/jeromeetienne/webaudiox/blob/master/examples/jsfx-basic.html)\] :
It shows a basic usage of this helper

## Show It Live

<iframe width="100%" height="480" src="http://jeromeetienne.github.io/webaudiox/examples/jsfx.html" frameborder="0" allowfullscreen></iframe>

#### How To Use It ?

Let's see how to use it. First you create a
[Audio Context](https://dvcs.w3.org/hg/audio/raw-file/tip/webaudio/specification.html#AudioContext-section) like this.

```
var context	= new AudioContext()
```

Now you get the famous ```lib``` parameter from
[jsfx](https://github.com/egonelbre/jsfx). You can generate some on its
[demo page](http://www.egonelbre.com/js/jsfx/).
From ```lib```, you will generate a
[Audio Buffer](https://dvcs.w3.org/hg/audio/raw-file/tip/webaudio/specification.html#AudioBuffer)
.

```javascript
var lib		= ["square",0.0000,0.4000,0.0000,0.3200,0.0000,0.2780,20.0000,496.0000,2400.0000,0.4640,0.0000,0.0000,0.0100,0.0003,0.0000,0.0000,0.0000,0.0235,0.0000,0.0000,0.0000,0.0000,1.0000,0.0000,0.0000,0.0000,0.0000]
// generate the audio buffer from jsfx.js lib
var buffer	= WebAudiox.getBufferFromJsfx(context, lib)
```

Now we are all ready to play a sound! So let's do that.

```javascript
var source	= context.createBufferSource()
source.buffer	= buffer
source.connect(context.destination)
source.start(0)
```

## Conclusion

So now you can easily generate procedural 8bit sound for Web Audio API.
[jsfx.js](https://github.com/egonelbre/jsfx) is a very nice library with
a retro theme that i love :)
Somehow, it seems a bit strange to do retro sounds with a modern web audio api.
So now you can play
[NES](http://en.wikipedia.org/wiki/Nintendo_Entertainment_System)
kind of sound with the dopler and localisation of the webaudio api.
Go For it!

Next week, i will be talk about sound analysis and how your programm can get to
know about what is being played.
so we will talk about
[webaudiox.analyser2canvas.js](https://github.com/jeromeetienne/webaudiox#webaudioxbytetonormalizedfloat32arrayjs)
and
[webaudiox.analyser2volume.js](https://github.com/jeromeetienne/webaudiox#webaudioxanalyser2canvasjs).

That's all for today folks! Have Fun :)
