---
layout: project
library: closure
version: 4.x
title: Caveats
description: A list of known limitations
---

Even though **Opis Closure** provides a ton of great features, there are some known limitations about which we consider
you must be warned of.

## Multiple closures per line

Due to PHP's own limitations, the library is unable to distinguish between two or more closures when they are
on the same line.

```php
// This will NOT work!
$first = function() {return "1st";}; $second = function() {return "2nd";};
// This will also NOT work!
$first = fn() => "1st"; $second = fn() => "2nd";
// This will NOT work, either.
$first = fn() => fn() => "2nd";

// This will work!
$first = function() {return "1st";};
$second = function() {return "2nd";};
```

## Multiple anonymous classes per line

```php
// This will not work
$first = new class() extends A {}; $second = new class() extends B {};
```

In the future, we plan to implement a heuristic to select the appropriate anonymous class. 
Until then, this should have minimal impact on most codebases.
