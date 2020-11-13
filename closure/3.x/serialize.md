---
layout: project
library: closure
version: 3.x
title: Quick start
description: Learn how to wrap a closure and make it serializable
---

## Serialize closures

If you ever used closures then you probably know that closures are not serializable. 
Trying to serialize a closure will result into an exception: 

```text
Fatal error: Uncaught exception 'Exception' with message 'Serialization of 'Closure' is not allowed'
```

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

### Serialize/unserialize arbitrary objects
{: #serialize-unserialize-arbitrary-objects }

**Opis Closure** allows you to serialize arbitrary objects with the help of the `Opis\Closure\serialize` function.
Unserialization is made by using the `Opis\Closure\unserialize` function.

```php
use function Opis\Closure\{serialize as s, unserialize as u}

class A
{
    private $closure;
    
    public function __construct()
    {
        $this->closure = function(){
            echo 'It works!';
        };
    }
    
    public function test()
    {
        //call closure
        ($this->closure)();
    }
}

$obj = new A();

u(s($obj))->test(); // It works
```
