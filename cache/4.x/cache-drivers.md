---
layout: project
library: cache
version: 4.x
title: Provided cache drivers
description: Cache drivers provided by Opis Cache
keywords: opis, cache, driver, php, file, memory
---

## Memory driver

This driver is mostly used for tests, it doesn't persist the data
to a storage.

```php
use Opis\Cache\Drivers\Memory;

$cache = new Memory();

$cache->write("key-a", "some content", 3);

echo $cache->read("key-a"); //> some content

// wait for cache to expire
sleep(4);

var_dump($cache->read("key-a")); //> bool(false)
```

## File driver

The file driver persists cached content to disk. The
file contents for a cache key is the following:

```
{timestamp}\n{serialized_content}
```

The serialized content is the result of applying 
[serialize](http://php.net/manual/en/function.serialize.php){:target="_blank"} function.
Not all data structures from PHP are serializable.
{:.alert.alert-warning}

You can configure the directory where to save the cached files and
the file extension. Also, you can have a prefix so you can use the
same directory for multiple cache storages.

```php
use Opis\Cache\Drivers\File;

$dir = "/my/path/to/dir";

$cache1 = new File($dir, 'cache1', 'txt');
$cache2 = new File($dir, 'cache2', 'txt');

$cache1->write('key-a', 'some content');
$cache2->write('key-a', 'other content');

// The files from /my/path/to/dir will be:
// - cache1.key-a.txt
// - cache2.key-a.txt

echo $cache1->read('key-a'); //> some content
echo $cache2->read('key-a'); //> other content

```

As you can see, the file name for a cache key follows the folowing pattern:

```
{prefix}.{cache-key}.{extension}
```

If the prefix is an empty string, the file name will look like

```
{cache-key}.{extension}
```


## PHP file driver

The PHP file driver is similar to [File driver](#file-driver)
but the file contents are different. Also, the file extension
is by default `php`.

```php
<?php
return array(
    'ttl' => 1234567890, // timestamp (int)
    'data' => '...' // serialized content (string)
);
```


