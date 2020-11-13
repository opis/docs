---
layout: project
library: http
version: 3.x
title: Streams
---

An HTTP message body is represented with the help of a stream object that implements the `Opis\Stream\IStream` interface.
A default implementation of this interface is provided by the `Opis\Stream\Stream` class.
The stream interface provides various methods that allow developers to work with native PHP streams 
in an object-oriented manner. Streams are also used when working with uploaded files, being a convenient way of handling
large amount of data.

The capabilities of a stream can be obtained by using the following methods:

* `isRedable` - tells if the stream is readable
* `isWritebale` - tells if the stream is writable
* `isSeekable` - tells if the stream allows random access

```php
if ($stream->isReadeable()) {
    // check if you can read from this stream
}

if ($stream->isWritable()) {
    // check if you can write to this stream
}

if ($stream->isSeekable()) {
    // check if this stream allows random access
}
```

## Stream info

Obtaining the size of a stream (in bytes), is done with the help of the `size` method.

```php
if ($stream->size() > 1024 * 1024) {
    // do something
}
``` 

You can check if a stream was closed or not, by using the `isClosed` method.

```php
if ($stream->isClosed()) {
    // do something
}
```

You can obtain the current cursor position inside the stream by using the `tell` method.

```php
$position = $stream->tell();
```

To check if the cursor position is at the end of the stream, use the `eof` method.

```php
if ($stream->isEOF()) {
    // do something
}
``` 

You can obtain an array of metadata by using the `metadata` method. Obtaining a specific key from de array,
is done by passing the key's name as an argument to the method. 
More information about the supported metadata keys can be found [here][0].

```php
// An array of metadata
$info = $stream->metadata();
// Get stream's URI
$uri = $stream->metadata('uri');
```

## Stream operations

Writing to a stream is done by using the `write` method. This method returns the number of bytes written or `null` if
some error have occurred.

```php
$stream->write('Hello world');
``` 

Reading a chunk of data is accomplished by using the `read` method. 
The default chunk size is 8192 bytes, but you can specify a custom size.

```php
// Read 8192 bytes of data
$data = $stream->read();

// Read 1024 bytes of data
$data = $stream->read(1024);
```

Reading from the current cursor position to the end of the stream, is done by using the `readToEnd` method.

```php
$data = $stream->readToEnd();
```

You can change the cursor's position by using the `seek` method. 

```php
$stream->seek(100);
```

The method takes a second, optional, argument that tells how the offset bytes should be interpreted. 

```php
$stream->seek(100, SEEK_CUR);
```

* `SEEK_SET` - set position equal to offset bytes. This is the default behavior.
* `SEEK_CUR` - set position to current location plus offset.
* `SEEK_END` - set position to end-of-file plus offset.

Moving the cursor back to the beginning of the stream, is done by using the `rewind` method.

```php
$stream->rewind();
```

Reading all stream data as a string, from the beginning to the end of the stream, is simply a matter of 
casting the stream object to the `string` data type.

```php
$data = (string) $stream;
```

Once you've done using the stream, you can close it by using the `close` method.

```php
$stream->close();
```

[0]: http://php.net/manual/ro/function.stream-get-meta-data.php "Stream metadata"
{:rel="nofollow" target="_blank"}