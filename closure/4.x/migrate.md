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

Version 4 is a full rewrite of the library, so you'll have to use the new `Serializer` to serialize/unserialize data,
instead of directly using PHP's serialize/unserialize. Also, you no longer have to wrap closures.

{% capture new_way %}
```php
use Opis\Closure\Serializer;

// Data to be serialized
$data = function(){
   return "I'm about to be serialized";
};

// Serialize
$serialized = Serializer::serialize($data);

// Unserialize
$unserialized = Serializer::unserialize($serialized);
```
{% endcapture %}
{% capture old_way%}
```php
use Opis\Closure\SerializableClosure;

// Data to be serialized
$data = function(){
   return "I'm about to be serialized";
};

// We have to wrap the closure
$wrapper = new SerializableClosure($data);

// Serialize the wrapper
$serialized = \serialize($wrapper);

// Unserialize and extract the closure
$unserialized = \unserialize($serialized)->getClosure();
```
{% endcapture %}
{% include tabs.html 1="New - v4" 2="Old - v3" _1=new_way _2=old_way %}

## Migrating data

If you already have serialized data saved in database or maybe in a queue, you can still use it in v4.
However, there are certain edge cases you might want to consider.

You can only deserialize data from v3 and serialize it to v4, you cannot serialize it to old v3 format.
{:.alert.alert-warning data-title="Important"}

### Automatic data migration

First, to enable deserialization of data from v3 you have to be explicit at initialization.

```php
use Opis\Closure\Serializer;

// Set second param to true to enable v3 compatibility
Serialize::init(null, true);
```

Second, if data in v3 used a security provider you have to use the same provider
(you might need to migrate the of security provider code as well).
If the security provider is the default one, you can pass the secret key at initialization.

```php
use Opis\Closure\Serializer;

// 1. Set the same secret key you used before
// 2. Set second param to true to enable v3 compatibility
Serialize::init("my-secret-key", true);
```

### Manual data migration

If, for some reason, you want to use a different security provider for v4 than v3, then you have the option to
unserialize data from v3 using `unserialize_v3` method. Also, if your v3 data doesn't contain a serialized closure then
you'll have to use this method as well.

You might also want create a script that migrates your data from v3 to v4 format using `unserialize_v3` and `serialize`.

```php
use Opis\Closure\Serializer;

// 1. Set the v4 security provider (optional)
// 2. You don't have to enable compatibility
Serialize::init("my-v3-key");

// Some data from v3
$v3_data = "...";

// If the data is not signed 
$unserialized = Serialize::unserialize_v3($v3_data);

// If the data is signed, use a security provider object for v3
$unserialized = Serialize::unserialize_v3($v3_data, $security);

// Once the data is deserialized, you can serialize it again,
// but the format will be v4
$serialized_in_v4 = Serializer::serialize($unserialized);
```