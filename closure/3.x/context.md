---
layout: project
library: closure
version: 3.x
title: Serialization context
description: Learn about serialization context
---

Sometimes, especially when we are working with serializable classes, it is desirable 
that an instance of a closure to be wrapped only by a single `Opis\Closure\SerializableClosure`
class, in order to avoid multiple serialization of the same closure instance. 
Let's take the following example: 

```php
use Opis\Closure\SerializableClosure;

$function = function(){
    return "Hello World";
};

$collection = array(
    'a' => new SerializableClosure($function),
    'b' => new SerializableClosure($function),
);

//Serialize
$collection = serialize($collection);
//Unserialize
$collection = unserialize($collection);

$a = $collection['a']->getClosure();
$b = $collection['b']->getClosure();

//Prints FALSE
print $a === $b ? 'TRUE' : 'FALSE';
```

As we can see in the above example, even though the same closure instance
was serialized, after the deserialization two different instances of the 
same closure were created.

This happened because the same closure was wrapped by two different 
`Opis\Closure\SerializableClosure` objects and therefore, when those objects 
were deserialized and the closures extracted, resulted two distinct closure instances.

Avoiding this kind of situations is done by serializing closures in context: 

```php
use Opis\Closure\SerializableClosure;

$function = function(){
    return "Hello World";
};

SerializableClosure::enterContext();

$collection = array(
    'a' => SerializableClosure::from($function),
    'b' => SerializableClosure::from($function),
);

SerializableClosure::exitContext();

//Serialize
$collection = serialize($collection);
//Unserialize
$collection = unserialize($collection);

$a = $collection['a']->getClosure();
$b = $collection['b']->getClosure();

//Prints TRUE
print $a === $b ? 'TRUE' : 'FALSE';
```

### The *enterContext* method 
{: #the-entercontext-method }

If a *serialization context* wasn't allready created by a previous call to this method, 
then a new *serialization context* is created. Each *serialization context* has an associated
list of serialized closures and an internal counter which is incremented each time this method is called.

Each call to this method must have a matching call to `Opis\Closure\SerializableClosure::exitContext` method. 

### The *from* method 
{: #the-from-method }

Wraps a given closure inside a `Opis\Closure\SerializableClosure` object, 
keeping a record of all closures that were wrapped in the current *serialization context*. 
When calling this method, if a closure was already wrapped, the coresponding 
`Opis\Closure\SerializableClosure` object will be returned.

This method can be used even if a *serialization context* wasn't created at call time. 

### The *exitContext* method 
{: #the-exitcontext-method }

Decrements the internal counter of the current *serialization context*, if any. 
When the counter reaches to zero, the current *serialization context* is destroyed. 