---
layout: project
library: http
version: 3.x
title: Response types
---

## String response

This type of response is represented with the help of the `Opis\Http\Responses\StringResponse` class and it allows you to
send a string as an HTTP response. 
The constructor of this class has the following signature:

```php
public function __construct(
    string $body, 
    int $status = 200, 
    array $headers = []
);
```

* `$body` - the message body
* `$status` - response status code
* `$headers` - a list of headers

By default, this class automatically adds a `Content-Length` header to the response.

```php
use Opis\Http\Responses\StringResponse;

$response = new StringResponse('Hello');

echo $response->getHeader('Content-Length'); //> 5
```

## HTML response

This type of response allows you the send an HTML string as an HTTP response. These type if responses are represented
with the help of the `Opis\Http\Responses\HtmlResponse` class. 
This class is subclass of `Opis\Http\Responses\StringResponse`.

```php
public function __construct(
    $body, 
    int $status = 200, 
    array $headers = []
);
```

* `$body` - a string or an instance of `Opis\Stream\IStream`
* `$status` - response status code
* `$headers` - a list of headers

By default, this class automatically adds a `Content-Length` and a `Content-Type` header to the response.

```php
use Opis\Http\Responses\HtmlResponse;

$response = new HtmlResponse('<h1>Hello</h1>');

echo $response->getHeader('Content-Length'); //> 14
echo $response->getHeader('Content-Type'); //> text/html; charset=utf-8
```

## JSON response

Sending a JSON string as an HTTP response can be accomplished by using the `Opis\Http\Responses\JsonResponse`, which
is a subclass of `Opis\Http\Responses\StringResponse`.

```php
public function __construct(
    $json,
    int $status = 200,
    array $headers = [],
    int $encodeOptions = JSON_UNESCAPED_SLASHES
);
```

* `json` - a string or an instance of `Opis\Stream\IStream`
* `$status` - response status code
* `$headers` - a list of headers
* `$encodeOptions` - encoding options that will be passed to the `json_encode` method.

By default, this class automatically adds a `Content-Length` and a `Content-Type` header to the response.

```php
use Opis\Http\Responses\JsonResponse;

$response = new JsonResponse('{"foo": "bar"}');

echo $response->getHeader('Content-Length'); //> 14
echo $response->getHeader('Content-Type'); //> application/json; charset=utf-8
```

## File stream

This type of response allows you to stream a file to the user's browser.

```php
public function __construct(
    string $file, 
    string $contentType = null, 
    int $status = 200, 
    array $headers = []
);
```

* `$file` - the path to the file you want to be streamed
* `$contentType` - the content-typeof the file, or `null` if you want that the content-type 
to be automatically determined based on the file's extension
* `$status` - response status code
* `$headers` - a list of headers

By default, this class automatically adds a `Content-Length` and a `Content-Type` header to the response.

```php
use Opis\Http\Responses\FileStream;

$response = new FileStream('/path/to/file.jpg');

echo $response->getHeader('Content-Length'); //> 102400
echo $response->getHeader('Content-Type'); //> image/jpeg
```

## File download

This type of response is very similar to the file stream response, excepting the fact that it will instruct
the browser to download the streamed file instead of trying to display it.

```php
public function __construct(
    string $file, 
    array $options = [], 
    int $status = 200, 
    array $headers = []
);
```
* `$file` - the path to the file you want to be downloaded
* `$options` - an array of options like *file_name*, *attachment*, and *content_type*
* `$status` - response status code
* `$headers` - a list of headers

By default, this class automatically adds a `Content-Length`, a `Content-Type`, and a `Content-Disposition` 
header to the response.

```php
use Opis\Http\Responses\FileDownload;

$response = new FileDownload('/path/to/file.jpg');

echo $response->getHeader('Content-Length'); //> 102400
echo $response->getHeader('Content-Type'); //> image/jpeg
echo $response->getHeader('Content-Disposition'); //> attachment; filename="file.jpg"
```

## Redirect response

Redirecting a user to another page can be easily done with the help of the `Opis\Http\Responses\RedirectResponse`.

```php
public function __construct(
    string $location, 
    int $status = 301, 
    array $headers = []
);
```

* `$location` - the location where you want the user to be redirected
* `$status` - response status code
* `$headers` - a list of headers

By default, this class automatically adds a `Location` header to the response.

```php
use Opis\Http\Responses\RedirectResponse;

$response = new RedirectResponse('http://example.com');

echo $response->getHeader('Location'); //> http://example.com
```

## Empty response

You can use the `Opis\Http\Responses\EmptyResponse` class to send an empty response to the user.

```php
public function __construct(
    int $status = 204, 
    array $headers = []
);
```

* `$status` - response status code
* `$headers` - a list of headers