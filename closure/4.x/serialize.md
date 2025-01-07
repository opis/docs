---
layout: project
library: closure
version: 4.x
title: Quick start
description: Learn how to serialize and unserialize closures and other arbitrary data
---

## Serialize closures, serialize anything

This library provides an enhanced version of PHP's
[`serialize`](https://www.php.net/manual/en/function.serialize.php){:rel="nofollow" target="_blank"} and 
[`unserialize`](https://www.php.net/manual/en/function.unserialize.php){:rel="nofollow" target="_blank"} functions.
It supports serializing arbitrary objects (arrays, closures, anonymous classes, and enums),  handles circular references, 
and provides an API for [custom object serialization](./objects.html).

```php
use function Opis\Closure\{serialize, unserialize};

$closure = fn() => "it works";

// serialize closure (or arbitrary data)
$serialized = serialize($closure);

// unserialize closure
$unserialized = unserialize($serialized);

echo $unserialized(); //> it works
```

You can serialize anonymous class instances and closures bound to those instances.

```php
use function Opis\Closure\{serialize, unserialize};

// create an anonymous class
$anonymous = new class("it works") {
    public function __construct(private string $message) {}
    
    public function getMessage(): string {
        return $this->message;
    }
    
    public function getClosure(): \Closure {
        return fn() => $this->message;
    }
};

// serialize anonymous classes
$object = unserialize(serialize($anonymous));
echo $object->getMessage(); // it works

// closures bound to anonymous classes just work
$closure = unserialize(serialize($anonymous->getClosure()));
echo $closure(); // it works
```

## Data signing

We often send serialized data to other services we control, and at deserialization, we want to ensure 
that the data has not been tampered with and is safe to use. **Opis Closure** provides a straightforward 
way of signing data and verifying its integrity.

```php
use Opis\Closure\Security\{
    DefaultSecurityProvider,
    SecurityProviderInterface,
};
use function Opis\Closure\init;

// Use the DefaultSecurityProvider (shortcut)
init("my-secret-key");

// Use the DefaultSecurityProvider
init(new DefaultSecurityProvider("my-secret-key"));

// Use a custom security provider
init(new class() implements SecurityProviderInterface {
    public function sign(string $data): string {
        // ...
    }
    
    public function verify(string $hash, string $data): bool {
        // ...
    }
});
```

Read more about [data signing](./security.html).
