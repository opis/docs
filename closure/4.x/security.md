---
layout: project
library: closure
version: 4.x
title: Signed data
description: Learn how to cryptographically sign your data
---

## Signing serialized data

While fetching and deserializing data from a local file or from a local database,
that can not be accessed from outside is totally fine, there might be situations when
you will want to take some precautions before executing a deserialized data. 

For example, if you are fetching serialized closures from a remote server,
you should definitely make sure that the closures were not altered on their way to your server. 

The simplest way of confirming data authenticity is by setting a secret key at initialization. 

{% capture remote_server %}
```php
use Opis\Closure\Serializer;

// Set secret key
Serializer::init("secret-key");

// Here you can serialize closures
$closure = function(){
   return "I'm a cryptographically signed closure";
};

push_to_server(Serializer::serialize($closure));
```
{% endcapture %}
{% capture your_server%}
```php
use Opis\Closure\Serializer;

// Use the same secret key
Serializer::init("secret-key");

// Here you can fetch closures from remote and unserialize them
$closure = Serializer::unserialize(fetch_from_remote());

//> I'm a cryptographically signed closure
echo $closure();
```
{% endcapture %}
{% include tabs.html 1="Remote server" 2="Your server" _1=remote_server _2=your_server %}

This method creates an instance of the `Opis\Closure\Security\DefaultSecurityProvider` that will 
cryptographically sign the data when it gets serialized and verify the signature when it's deserialized.
If the signature is not valid an `Opis\Closure\Security\SecurityException` is thrown.

## Custom security providers

You must make sure that the same security provider,
with the same settings, is used both for serialization and unserialization. 
{:.alert.alert-warning data-title="Important"}

If you are unhappy with the default security provider, you can use your own by creating a class that implements
the `Opis\Closure\Security\SecurityProviderInterface` interface and passing an instance of that 
class at initialization or to `setSecurityProvider` method.

```php
use Opis\Closure\Serializer;
use Opis\Closure\Security\SecurityProviderInterface;

class MySecurityProvider implements SecurityProviderInterface
{
    public function sign(string $data): string {
        // you should return a hash for $data
    }
    
    public function verify(string $hash, string $data): bool {
        // verify if the $hash is for $data
    }
}

// Set at init
Serializer::init(new MySecurityProvider);

// Set anytime
Serializer::setSecurityProvider(new MySecurityProvider);

// Get current security provider
$security = Serializer::getSecurityProvider();
```

## Detecting security errors

To detect security errors you have to catch exceptions of class `Opis\Closure\Security\SecurityException`.

```php
use Opis\Closure\Serializer;
use Opis\Closure\Security\SecurityException;

Serializer::init("my-secret-key");

// sample serialized data
$serialized = "...";

try {
    $unserialized = Serializer::unserialize($serialized);
} catch (SecurityException $e) {
    // this is a security error
} catch (\Throwable $e) {
    // any other exception
}
```

Keep in mind that if you don't use a security provider you cannot deserialize signed data,
and if do use a security provider you cannot deserialize unsigned data.
