---
layout: project
library: http
version: 3.x
title: HTTP Messages
---

An HTTP message can be either a request sent from a client to a server, or a response sent by the server back to the client.
The parts that are common to both request messages and response messages (protocol version, headers, and message body)
are implemented in the `Opis\Http\Message` abstract class.

The constructor of the `Message` class has the following signature:

```php
public function __construct(
    ?\Opis\Stream\IStream $body = null, 
    array $headers = [], 
    string $protocolVersion = '1.1'
);
```

* `$body` - the message body. It must be an instance of a stream or `null`
* `$headers` - a list a zero or more headers. The key represents the header's name and the value represents the actual
header value. You can pass the header names in a cas insensitive manner, since they are all normalised internally.
* `$protocolVersion` - the protocol version number

## Protocol version

The protocol version number can be obtained by using the `getProtocolVersion` method.

```php
$version = $message->getProtocolVersion();
```

## Message body

The message body is represented with the help of streams. You can get the message body by using the
`getBody` method. If the message doesn't have a body, `null` is returned.

```php
$body = $message->getBody();
```

## Headers

Getting the full list of headers is done by using the `getHeaders` method.

```php
$headers = $message->getHeaders();
```

Checking if a particular header exists is done by using the `hasHeader` method.

```php
if ($request->hasHeader('Content-Type')) {
    // do something
}
```

Reading the value of a header is also very easy: just pass the name
of the header (case insensitive) to the `getHeader` method.

```php
$value = $request->getHeader('Content-Type');
```

By default, if the header doesn't exist, than `null` is returned. You can specify which value to be returned, by passing
a second argument to the `getHeader` method.

```php
$value = $request->getHeader('Content-Type', 'text/html');
```