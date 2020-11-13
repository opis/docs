---
layout: project
library: http
version: 3.x
title: URIs
---

URIs are represented with the help of the `Opis\Http\Uri` class.
The role of this class is to decompose an URI into its basic components and provide developerds various methods that
allow them to read the value of these individual components.

Constructing an URI is supper-easy and can be accomplished just as simple as bellow:

```php
use Opis\Http\Uri;

$uri = new Uri("http://example.com");
echo $uri->getHost(); //> example.com
```

An URI object can be converted back to string through a simple type casting.

```php
use Opis\Http\Uri;

$uri = new Uri('http://example.com');

// explicit type casting
$value = (string) $uri; 
echo $value; //> http://example.com

// implicit type catsing
echo $uri; //> http://example.com
```

## Components

You can obtain an array of components by calling the `getComponents` method. The method will return
a key-value mapped array where the key represents the component's name and the value represents the component's value.

```php
$components = $uri->getComponents();
```

The following components are available for the following URI `http://user:pass@example.com:8080/foo?bar=baz#qux`:

* `scheme` - *http*. 
* `authority` - *user:pass@example.com:8080*
* `user_info` - *user:pass*
* `host` - *example.com:8080*
* `port` - *8080*. When the port is not explicitly specified, the value of this components is derived from the *scheme*
component: *80* when the scheme is *http*, *443* when the scheme is *https*, and `null` otherwise.
* `path` - */foo*
* `query` - *bar=baz*
* `fragment` - *qux*

The components of an URI can be accessed individually by using the following methods:

* `getScheme` - read the `scheme` component
* `getAuthority` - read the `authority` component
* `getUserInfo` - read the `user_info` component
* `getHost` - read the `host` component
* `getPort` - read the `port` component
* `getPath` - read the `path` component
* `getQuery` - read the `query` component
* `getFragment` - read the `fragment` component

You can also construct an URI by passing an array of components to its constructor.

```php
use Opis\Http\Uri;

$components = [
    'scheme' => 'https',
    'host' => 'example.com',
    'path' => '/test'
];

$uri = new Uri($components);

echo $uri->getPort(); //> 443
echo $uri->getPath(); //> /test
echo $uri; //>https://example.com/test
```

