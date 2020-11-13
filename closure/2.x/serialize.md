---
layout: project
library: closure
version: 2.x
title: Serialize closures
description: Learn how to wrap a closure and make it serializable
canonical: /closure/3.x/serialize.html
---

## Serialize closures

If you ever used closures then you probably know that closures are not serializable. 
Trying to serialize a closure will result into an exception: 

    Fatal error: Uncaught exception 'Exception' with message 'Serialization of 'Closure' is not allowed'

The solution for overcoming this problem is to wrap the closure into an `Opis\Closure\SerializableClosure`
object, then serialize the wrapper object using the standard `serialize`. 

```php
use Opis\Closure\SerializableClosure;

// Recursive factorial closure
$factorial = function ($n) use (&$factorial) {
  return $n <= 1 ? 1 : $factorial($n - 1) * $n;
};

// Wrap the closure
$wrapper = new SerializableClosure($factorial);
// Now it can be serialized
$serialized = serialize($wrapper);
```

## Unserialize closures

Closure unserialization is done using the standard `unserialize` function. 
Once the wrapper was unserialized, you can directly invoke the wrapper, 
or you can extract the serialized closure by calling the `getClosure` method. 

```php
// Unserialize the closure
$wrapper = unserialize($serialized);

// You can directly invoke the wrapper...
echo $wrapper(5); //> 120

// Or, the recommended way, extract the closure object
$closure = $wrapper->getClosure();

echo $closure(5); //> 120
```

Due to the fact that **Opis Closure** doesn't use `eval` in the serialization-deserialization process,
all closures can be serialized and unserialized an infinite amount of times. 

```php
// Once again, but this time using the previously unserialized closure
$wrapper = new SerializableClosure($closure);
$serialized = serialize($wrapper);
$wrapper = unserialize($serialized);
$closure = $wrapper->getClosure();

// Now watch this...
echo $closure(5); //> 120
// It worked!
```

### The *unserializeData* method 
{: #the-unserializedata-method }

If you are planning to support PHP 5.3, then you should be aware that there 
is a [bug](https://bugs.php.net/bug.php?id=36424) (fixed in PHP 5.4.0) 
that will prevent **Opis Closure** to work properly in [certain situations](context.html).

This problem can be overcome by using the `unserializeData` static method of the 
`Opis\Closure\SerializableClosure` class, which is just a wrapper for the `unserialize`
function, that will somehow fix the previously mentioned bug. 

```php
// Unserialize the closure
$wrapper = SerializableClosure::unserializeData($serialized);

// Extract the closure object
$closure = $wrapper->getClosure();

echo $closure(5); //> 120
```
