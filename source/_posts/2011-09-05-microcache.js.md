---
layout: post
published: true
title: MicroCache.js - Cache Micro Library
categories: [microlib, node, browser]
---

**microcache.js** is a micro library to handle in-memory cache. It is less than
20 lines and works in node and browser. 
It is available on github <a href='https://github.com/jeromeetienne/microcache.js'>here</a>
under <a href='https://github.com/jeromeetienne/microcache.js/blob/master/MIT-LICENSE.txt'>MIT license</a>.
If you hit bugs, fill issues on github. Feel free to fork, modify and have fun with it :)

## Install it

To install it on node

```bash
    npm install microcache
```

To install the browser version, download it and include it like that

```html
    <script src="microcache.js"></script>   
```

<!-- more -->

## API

To instanciate a cache, do the following. You can have as many instances you want.

```javascript
    var microcache = new MicroCache();
```

### .get(key)

To get a cached asset which is in the cache. Each cached element has a unique
key to identify it.

```javascript
    microcache.get('foo');
```

### .set(key, value)

To set a element value in the cache.

```javascript
    microcache.set('foo', 'bar');
```

### .contains(key)

To know if a given asset is currently in the cache.

```javascript
    microcache.contains('foo');
```

### .values()

To return all the elements currently in the cache

```javascript
    microcache.values();
```

### .getSet(key, value)

To get an element from the cache, if it isnt already present, store it then return it

```javascript
    microcache.getSet(key, value);
```

## FAQ

  * **Q.** what about plain ```var microcache = {}``` ? isnt this wrapper overengineering ?
  * **A.**  i was thinking so too at first. but the syntax of a .getSet() without wrapper was too ugly for my taste :)
```var a = cache[key] = (cache[key] || value)``` from @floriancargoet

