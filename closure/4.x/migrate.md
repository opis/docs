---
layout: project
library: closure
version: 4.x
title: Migration guide
description: How to migrate from v3 to v4
---

The migration from v3 contains two parts:

1. changes to code
2. migrating existing serialized data 

## Migrating code

- `Opis\Closure\SerializableClosure` **was removed** in v4; closures no longer need to be wrapped. Make sure you remove anything related to it, including but not limited to:
  - new instance creation `new SerializableClosure(...)`
  - closure extraction at unserialization using `SerializableClosure::getClosure()` method
  - context related static calls: `SerializableClosure::from(...)`, `SerializableClosure::enterContext()`, `SerializableClosure::exitContext()`
  - security related static calls: `SerializableClosure::setSecretKey()` and `SerializableClosure::addSecurityProvider()`
  Read about [how to sign data](./security.html) in v4
  - `SerializableClosure::createClosure()` can be replaced with `Opis\Closure\create_closure()`
- `Opis\Closure\ISecurityProvider` no longer exists, use `Opis\Closure\Security\SecurityProviderInterface` instead (new signature)
- `Opis\Closure\SecurityProvider` was replaced by `Opis\Closure\Security\DefaultSecurityProvider`
- `Opis\Closure\SecurityException` is now `Opis\Closure\Security\SecurityException`
- `Opis\Closure\ReflectionClosure` has a new API

You should keep any calls to `Opis\Closure\serialize()` and `Opis\Closure\unserialize()` functions. If you didn't use the functions,
you'll now have to use them instead of PHP's `\serialize()` and `\unserialize()`.

## Migrating data

If you already have serialized data saved in database or maybe in a queue, you can still use it in v4.
However, there are certain edge cases you might want to consider.

You can only deserialize data from v3 and serialize it to v4, you cannot serialize it to old v3 format.
{:.alert.alert-warning data-title="Important"}

### Automatic data migration

First, to enable deserialization of data from v3 you have to be explicit at initialization.

```php
// Set second param to true to enable v3 compatibility
\Opis\Closure\init(null, true);
```

Second, if data in v3 used a security provider you have to use the same provider
(you might need to migrate the security provider code as well).
If the security provider is the default one, you can pass the secret key at initialization.

```php
// 1. Set the same secret key you used before
// 2. Set second param to true to enable v3 compatibility
\Opis\Closure\init("my-secret-key", true);
```

### Manual data migration

If, for some reason, you want to use a different security provider for v4 than v3, then you have the option to
unserialize data from v3 using `Opis\Closure\v3_unserialize()` function. 
Also, if your v3 data doesn't contain a serialized closure then you'll have to use this method as well.

You might also want to create a script that migrates your data from v3 to v4 format 
using `Opis\Closure\v3_unserialize()` and `Opis\Closure\serialize()`.

```php
use function Opis\Closure\{init, serialize, v3_unserialize};

// 1. Set the v4 security provider (optional)
// 2. You don't have to enable compatibility
init("my-v3-key");

// Some data from v3
$v3_data = "...";

// If the data is not signed 
$unserialized = v3_unserialize($v3_data);

// If the data is signed, use a security provider object for v3
$unserialized = v3_unserialize($v3_data, $security);

// Once the data is deserialized, you can serialize it again,
// but the format will be v4
$serialized_in_v4 = serialize($unserialized);
```