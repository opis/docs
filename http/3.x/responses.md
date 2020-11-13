---
layout: project
library: http
version: 3.x
title: HTTP Responses
---

## Instantiation

An HTTP response is represented with the help of the `Opis\Http\Response` class, 
or by using a class that inherits from the `Opis\Http\Response` class. 
The constructor of the class has the following signature.

```php
public function __construct(
    int $statusCode = 200, 
    array $headers = [], 
    \Opis\Stream\IStream $body = null, 
    string $protocolVersion = '1.1'
);
```

* `$statusCode` - response status code
* `$headers` - a key-value mapped array of headers, where the key represents the header's name and the value
represents the actual value of the header
* `$body` - the response body, if any
* `$protocolVersion` - the protocol version number

## Immutability

By default, all response instances are designed to be immutable. 
This design choice was made in order to prevent an HTTP response for being unintentionally modified.
If you want to modify the response (e.g. add a cookie), then you must use the `modify` method.
This method returns a cloned version of the original response object, that reflects the changes you've made.

```php
use Opis\Http\Response;

// ..

$response = $response->modify(function(Response $response){
    // add headers, set status code, add new cookies, etc.
});
```

## Adding new headers

Adding a new header is done with the help of the `setHeader` method.
Because of the immutable nature of the response object, trying to call this method
directly, will result into an exception being thrown. Instead, you must use the `modify` method.

```php
// will throw exception
$response->setHeader('X-Foo', 'Bar');

// The right way to do it
$response = $response->modify(function(Response $response){
    $response->setHeader('X-Foo', 'Bar');
    $response->setHeader('X-Bar', 'Baz');
});
```

Setting multiple headers at the same time can be accomplished by using the `addHeaders` method.

```php
$response = $response->modify(function(Response $response){
    $response->addHeaders([
        'X-Foo' => 'Bar',
        'X-Bar' => 'Baz',
    ]);
});
```

## Cookies

Setting a new cookie is done with the help of the `setCookie` method.

```php
$resposne = $response->modify(function(Response $response){
    // set a cookie named 'foo', having a value 'bar'
    $response->setCookie('foo', 'bar');
});
```

The better explain the `setCookie` method, let's analise its signature.

```php
public function setCookie(
    string $name,
    string $value = '',
    int $expire = 0,
    string $path = '',
    string $domain = '',
    bool $secure = false,
    bool $http_only = false
): self;
```

* `$name` - the name of the cookie
* `$value` - the value of the cookie
* `$expire` - a UNIX timestamp that tells when this cookie is set to expire. 
* `$path`  - tells on which path this cookie is valid
* `$domain` - 
* `$secure` -
* `$http_only` - 


Getting the full list of cookies is done by calling the `getCookies` method.

```php
$cookeis = $response->getCookies();
```

Reading an individual cookie is done by using the `getCookie` method. You can specify
a custom path or domain on which the cookie was set.

```php
if ($response->getCookie('foo')) {
    // do something
}

if ($response->getCookie('foo', '/some/path/')) {
    // do something
}

if ($response->getCookie('foo', '', 'www.domain.com')) {
    // do something
}

if ($response->getCookie('foo', '/some/path/', 'www.domain.com')) {
    // do something
}
```

If you want to check if a cookie was set or not, simply use the `hasCookie` method.
You can optionally specify a domain and a path, and check if a cookie was set for the
specified domain and path. 

```php
if ($response->hasCookie('foo')) {
    // do something
}

if ($response->hasCookie('foo', '/some/path/')) {
    // do something
}

if ($response->hasCookie('foo', '', 'www.domain.com')) {
    // do something
}

if ($response->hasCookie('foo', '/some/path/', 'www.domain.com')) {
    // do something
}
```

Removing a cookie is done by using the `clearCookie` method.

```php
$response = $response->modify(function(Response $response){
    $response->clearCookie('foo');
    $response->clearCookie('foo', '/some/path/');
    $response->clearCookie('foo', '', 'www.domain.com');
    $response->clearCookie('foo', '/some/path/', 'www.domain.com');
});
```

You can remove all cookies by using the `clearCookies` method;

```php
$response = $response->modify(function(Response $response){
    $response->clearCookies();
});
```

## Miscellaneous

You can set the response body by calling the `setBody` method.

```php
$response = $response->modify(function(Response $response){
    $response->setBody(null);
    // or
    $response->setBody(new \Opis\Stream\Stream("php://temp", "wb+"));
});
```

You can modify the protocol version number by calling the `setProtocolVersion` method.

```php
$response = $response->modify(function(Response $response){
    $response->setProtocolVersion('1.0');
});
```

Modifying the response status code is done by using the `setStatusCode` method.
You can read the current status code with the help of the `getStatusCode` method.

```php
$code = $response->getStatusCode();

$response = $response->modify(function(Response $response){
    $response->setStatusCode(200);
});
```


