---
layout: project
library: closure
version: 4.x
title: Quick start
description: Learn how to serialize and unserialize arbitrary data
---

## Serialize anything

This library is to be used as a replacement for PHP's 
[`serialize`](https://www.php.net/manual/en/function.serialize.php) & 
[`unserialize`](https://www.php.net/manual/en/function.unserialize.php) functions.

It supports serializing arbitrary objects, arrays, closures, enums, it handles circular references, and also provides an 
API for [custom object serialization](./objects.html).

```php
use Opis\Closure\Serializer;

$data = [
    "func" => fn() => "it works",
];

// serialize arbitrary data (including closures)
$serialized = Serializer::serialize($data);

// unserialize data
$unserialized = Serializer::unserialize($serialized);

// see the deserialized data
var_dump($unserialized);

// test if closure works
var_dump($unserialized["func"]());
```

## Signing data

We often send serialized data to other services we control,
and at deserialization we want to make sure the data is safe to use.
Signing data is very easy with **Opis Closure**, just use a security provider
at initialization.

```php
use Opis\Closure\Serializer;
use Opis\Closure\Security\{
    DefaultSecurityProvider,
    SecurityProviderInterface,
};

// Use the DefaultSecurityProvider (shortcut)
Serializer::init("my-secret-key");

// Use the DefaultSecurityProvider
Serializer::init(new DefaultSecurityProvider("my-secret-key"));

// Use a custom security provider
Serializer::init(new class() implements SecurityProviderInterface {
    public function sign(string $data): string {
        // ...
    }
    
    public function verify(string $hash, string $data): bool {
        // ...
    }
});
```

Read more about [signing data](./security.html).
