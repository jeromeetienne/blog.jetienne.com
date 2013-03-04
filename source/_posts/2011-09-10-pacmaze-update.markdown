---
layout: post
title: "Pacmaze Update"
date: 2011-09-10 12:12
published: true
comments: true
categories: [game, three.js, pacmaze]
---


Pacmaze just got an [update](http://pacmaze.com).
it was long overdue, i admit, other matters distracted me from it.
It is good to be back on it tho.
It is version 5 now. It is a major performance update, **5 time faster** in some cases.
The game is now embeddable in external pages.
Additionnaly, Packy has been renamed Pucky as a reference to the original name of pacman,
['Puck-Man'](http://www.google.com/search?tbm=isch&hl=en&source=hp&biw=1280&bih=644&q=puckman&gbv=2&oq=puckman&aq=f&aqi=&aql=&gs_sm=s&gs_upl=0l0l0l4562l0l0l0l0l0l0l0l0ll0l0).
You can read all about the history of pacman in the [pacman dossier](http://home.comcast.net/~jpittman2/pacman/pacmandossier.html).

## Embedded game

See an embedded version below.
Use AWSD
keys to play or play the [full page version](http://pacmaze.com).

<center><iframe src='http://pacmaze.com' width='480px' height='360px'></iframe></center>

<!-- more -->

The game is now embeddable in external page.
To embed the game in your own page, just do the following and tune the css to own taste.

```html
    <iframe src='http://pacmaze.com' width='480px' height='360px'></iframe>
```

## Performance boost

*Pacmaze v5* is a major performance update, especially for macbook.
I got recently got a [macbook](http://www.apple.com/macbookpro/specs-13inch.html).
I quickly noticed [pacmaze](http://pacmaze) wasn't too good on it, to say the least.
So i optimized the WebGL on this plateform.  The performance are now much better.
For Chrome, the performance went from 10fps to more than 50fps.
On Firefox, the old version got bugs and 7fps... ouch. Now known bugs are removed
and the average fps is close to 45fps.

So [fps](http://en.wikipedia.org/wiki/Frame_rate) on macbook is now
5 time faster! Quite a boost! Next update i plan to focus on visual effets.

