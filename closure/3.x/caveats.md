---
layout: project
library: closure
version: 3.x
title: Caveats
description: A list of known limitations
---

Even though **Opis Closure** provides a ton of great features, there are some known limitations about which we consider
you must be warned of.

## Native classes

**Opis Closure** will automatically wrap every closure, in every object, as long as that object is an instance of a class 
defined in userland code. But when it comes to wrap closures contained by objects that are instances of classes that are 
not defined in userland code, - classes provided by PHP itself or by some native extension - we are unable to check
if those objects contain closures. And this is because PHP doesn't allow us to access a native class' 
internal properties from userland code. 

## Multiple closures per line

Due to PHP's own limitations, the library is unable to distinguish between two or more closures when they are
on the same line.

```php
// This will NOT work!
$first = function() {return "1st";}; $second = function() {return "2nd";};

// This will work!
$first = function() {return "1st";};
$second = function() {return "2nd";};
```

## Multiple namespaces per file

This limitation is a design choice we've made in order to keep the parser logic as simple as possible.
Having multiple namespaces per file is very uncommon, and is generally considered to be a bad practice. 
Therefore, the impact of this decision should be minimal.

```php
// Won't work
namespace A {
}
namespace B {
}
```
