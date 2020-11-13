---
layout: project
library: closure
version: 3.x
title: Features
description: A list of library's main features
---

## Resolve class names

All class names referenced by a closure are automatically resolved at serialization time, 
allowing you to write a cleaner code. 

```php
use Some\Namespace\SomeClass;
use Some\Namespace as X;
use Other\Namespace\SomeClass as OtherClass;
use Some\Namespace\CaseInsensitiveClass;

$closure = function(SomeClass $a, OtherClass $b, X\SomeClass $c){
    return [
        new CASEInsenitiveCLASS(),
        new X\SomeClass(),
        new SomeClass(),
        new OtherClass(),
    ];
};

//Class names are resolved
$closure = unserialize(serialize(new SerializableClosure($closure)))->getClosure();
```

## Resolve function and constant names
{:#resolve-functions-constants}

The library also supports referencing functions and constants.

```php
use function Some\Namespace\f;
use function Other\Namespace\f as a;
use function Some\Namespace\CaseInsensitive;
use const Some\Namespace\C;
use const Other\Namespace\C as U;

$closure = function(){
    caseInsensitive();
    f(U);
    a(C);
};

//Names are resolved
$closure = unserialize(serialize(new SerializableClosure($closure)))->getClosure();
```

## PHP 7.x support
{:#php-7-support}

**Opis Closure** fully support the new PHP 7 syntax for `use` statements.

```php
use Some\Namespace{
    Class1,
    SubNamesace\Class2,
    Class3 as T
};
use function OtherNS{
    f1,
    Sub\f2 as function2
}

$closure = function(Class1 $a, Class2 $b, T $c){
    f1();
    function2();
};

//Names are resolved
$closure = unserialize(serialize(new SerializableClosure($closure)))->getClosure();
```

Type hints are supported as well

```php

use Some\Namespace as N;

$closure = function(string $a, int $b = null, $c): N\SomeClass{
    return $c instanceof self;
}

//Names are resolved
$closure = unserialize(serialize(new SerializableClosure($closure)))->getClosure();
```

## Resolve closure's scope and $this object
{:#resolve-scope-this}

**Opis Closure** automatically detects when the scope and the `$this` object of a closure needs to be serialized 
in order for the closure to work properly after deserialization.

```php
use Opis\Closure\SerialiazbleClosure;

class A
{
    function a()
    {
        // scope & $this are not serialized
        serialize(new SerializableClosure(function(){
            echo 123;
        }));
    }
    
    function b()
    {
        // scope is serialized
        serialize(new SerializableClosure(function(){
            echo self::class;
        }));
    }
    
    function c()
    {
        // scope & $this are serialized
        serialize(new SerializableClosure(function(){
            $this->a();
        }));
    }
}
```
