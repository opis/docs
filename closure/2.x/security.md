---
layout: project
library: closure
version: 2.x
title: Signed closures
description: Learn how to cryptographically sign your closures
canonical: /closure/3.x/security.html
---

## Signing closures

While fetching and unserializing closures from a local file or from a local database,
that can not be accessed from outside is totally fine, there might be situations when
you will want to take some precautions before executing a closure. 

For example, if you are fetching serialized closures from a remote server,
you should definitely make sure that the closures were not altered on their way to your server. 

The simplest way of confirming a closure's authenticity is by using the `setSecretKey` method. 

{% capture remote_server %}
```php
use Opis\Closure\SerializableClosure;

SerializableClosure::setSecretKey('secret');

// Here you can serialize closures
$closure = function(){
   return "I'm a cryptographically signed closure";
};

push_to_server(serialize(new SerializableClosure($closure)));
```
{% endcapture %}
{% capture your_server%}
```php
use Opis\Closure\SerializableClosure;

SerializableClosure::setSecretKey('secret');

// Here you can fetch closures from remote and unserialize them
$closure = unserialize(fetch_from_remote())->getClosure();
echo $closure(); //I'm a cryptographically signed closure
```
{% endcapture %}
{% include tabs.html 1="Remote server" 2="Your server" _1=remote_server _2=your_server %}

This method creates an instance of the `Opis\Closure\SecurityProvider` that will cryptographically sign the closure 
when it gets serialized and verify the signature when it's unserialized. 
If the signature is not valid an `Opis\Closure\SecurityException` is thrown.

## Custom security providers

You must make sure that the same security provider,
with the same settings, is used both for serialization and unserialization. 
{:.alert.alert-warning data-title="Important"}

If you are unhappy with the default security provider, you can use your own by creating a class that implements
the `Opis\Closure\ISecurityProvider` interface and passing an instance of that class to `addSecurityProvider` method.

```php
use Opis\Closure\SerializableClosure;
use Opis\Closure\ISecurityProvider;

class MySecurityProvider implements ISecurityProvider
{
   // ...
}

SerializableClosure::addSecurityProvider(new MySecurityProvider);

// Serialize closures here
```
