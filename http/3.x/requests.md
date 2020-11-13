---
layout: project
library: http
version: 3.x
title: HTTP Requests
---

## Instantiation

An HTTP request is represented with the help of the `Opis\Http\Request` class.
The simplest way to instantiate a request object, is by calling the `fromGlobals` static method.

```php
use Opis\Http\Request;

$request = Request::fromGlobals();
```

You could also instantiate the request object yourself, by passing a series of arguments to its constructor.
To better understand what each of the arguments mean, let's take a look to the constructor's signature.

```php
/**
 * Constructor
 */
public function __construct(
    string $method = 'GET',
    string $requestTarget = '/',
    string $protocolVersion = '1.1',
    bool $secure = false,
    array $headers = [],
    array $files = [],
    ?\Opis\Stream\IStream $body = null,
    ?array $cookies = null,
    ?array $query = null,
    ?array $formData = null
);
```

* `$method`  - represents the HTTP method used in this request. Typical values for this argument are: 
`GET`, `POST`, `PUT`, `DELETE`, and `OPTIONS`
* `$requestTarget` - the request URI, usually in the form of an absolute path, optionally followed by a query string
* `$protocolVersion` - the HTTP's version
* `$secure` - tells if the request was made using a secure connection or not
* `$headers` - a key-value mapped array, where the key represents the header name, and the
value represents the actual value of the header. If the `$requestTarget` is a form of an absolute path,
then the list of headers must contain a `Host` entry.
* `$files` - a list of uploaded files. Each uploaded files is represented by using an object that implements the 
`Opis\Http\IUploadedFile` interface. You can use the `Opis\Http\UploadedFile::parseFiles` method to obtain such
a list from the `$_FILES` global variable.
* `$body` - this arguments represents the request's body, if present. If you omit this argument, or pass `null`, 
then its value will be automatically determined.
* `$cookies` - a list of parsed cookies or the `$_COOKIE` global variable itself. If the argument is omitted or
`null` is passed as a value, then the value of `$cookies` will be automatically determined from `$headers`.
* `$query` - a list of query parameters or the `$_GET` global variable itself. If this argument is omitted or
`null` is passed as a value, then the value of `$query` will be automatically determined from `$requestTarget`.
* `$formData` - a list of submitted values or the `$_POST` global variable itself. If this argument is omitted or
`null` is passed as a value, then the value of `$formData` will be automatically determined from `$body`.

## Cookies

You can read the full list of cookies by using the `getCookies` method.

```php
$cookies = $request->getCookies();
```

Checking if a cookies exists is done with the help of the `hasCookie` method.

```php
if ($request->hasCookie('foo')) {
    // do something
}
```

The `getCookie` method can be used to read the value of a cookie. 
If the cookie doesn't exist, `null` is returned instead.

```php
$cookie = $request->getCookie('foo');
```

By default, the value of the cookie is URL-decoded. To suppress this behavior, simply pass `false` as the second
argument to the `getCookie` method.

```php
$cookie = $request->getCookie('foo', false);
```

## User-submitted data

When we are talking about **from submitted data**, we are referring to those requests that were sent using the `POST` 
method, have a body and have a `Content-Type` header whose value is set to`application/x-www-form-urlencoded`. 
{:.alert.alert-info data-title="Remember"}

### Form data

Reading all the fields submitted by a user, with the help of a form, is accomplished by using the `getFormData` method.

```php
$data = $request->getFormData();
```

Reading individual field values is done by using the `formData` method and passing the field's name as an argument.

```php
// Read username
$username = $request->formData('username');

// Read password
$password = $request->formData('password');
```

If the field doesn't exist, `null` is returned instead, as a fallback value. 
You can specify a custom fallback value by passing a second argument to the method.

```php
// Custom fallback value
$website = $request->formData('website', 'https://example.com');
```

### Raw body

If the data submitted by an user was not sent with the help of a form, you can use the `getBody` method to access
the stream object that will help you read the data.

```php
$data = $request->getBody();

$json = json_decode($data->readyToEnd());
```

### Query params

Getting the full list of query params is done with the help of the `getQuery` method.

```php
$query = $request->getQuery();
```

Reading individual query params is done with the help of the `query` method.
If the query param doesn't exist, `null` is returned instead, as a fallback value.

```php
$page = $request->query('page');
```

You can specify a custom fallback value by passing a second argument to the method.

```php
// Custom fallback value
$page = $request->query('page', 1);
```

### File uploads

You can use the `getUploadedFile` method to obtain the full list of uploaded files.

```php
$files = $request->getUploadedFiles();
```

Getting a specific file is done by using the `file` method. The method will return an object
that implements the `Opis\Http\IUploadedFile` interface, or `null` if the file doesn't exist.

```php
$file = $request->file('picture');

if ($file !== null) {
    $file->moveTo('/some/path/picture.jpg');
}
```

## Miscellaneous

Reading the method that was used for a particular request is done by using the `getMethod` method.

```php
if ('GET' !== $request->getMethod()) {
    // do something
}
```

You can use the `isSecure` method to find out if a request was made using a secure connection or not.

```php
if (!$request->isSecure()) {
    // Handle request
}
```

Reading the request target is done with the help of the `getRequestTarget` method.

```php
echo $request->getRequestTarget();
```

You can also obtain a full URI of the request by using the `getUri` method.
The method will return an instance of `Opis\Http\Uri` class.

```php
$uri = $request->getUri();

if ($uri->getPath() === '/hello') {
    // do something
}
```
