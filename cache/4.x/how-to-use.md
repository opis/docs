---
layout: project
library: cache
version: 4.x
title: Quick start
description: Working with cached data in php using Opis Cache
keywords: opis, cache, read, write, delete, load, ttl
---

In order to work with cache, you'll first have to create an instance
of the desired cache driver.

```php
$cache = new \Opis\Cache\Drivers\File("/path/to/cache");
```

## Writing content to cache

Writing data to cache can be performed by using the `write` method.

```php
// Persistent cache
$cache->write("key-a", "some content");

// Cache for 10 seconds
$cache->write("key-b", [1, 2, 3], 10);
```

## Fetching cached content

The get the cached content for a specific cache key, you can use the
`read` method.

```php
echo $cache->read('key-a'); //> some content
```

The `read` method returns boolean `false` if no content is present
for the specified key, so in some cases you will first want to
check if the cache key exists by using the `has` method.

```php
if ($cache->has('key-a')) {
  echo $cache->read('key-a'); //> some content
} else {
  echo "key-a is not present";
}
```

However, in most cases you'll want to generate the cached content
if it's missing. Instead of using a combination of `has`, `read` and
`write` you could simply use the `load` method.

```php
$numbers = $cache->load('key-b', function () {
    // this function is invoked only if the cache key is missing
    // the return value of this function is saved in cache, using the
    //    specified ttl, in our case 10 seconds
    return [1, 2, 3];
}, 10);

echo array_sum($numbers); //> 6
```

## Deleting cached content

To remove a cached content by key you can use the `delete` method.

```php
$deleted = $cache->delete("key-a");
```

In order to remove all cached content, regardless of key, you
can use the `clear` method.

```php
$cache->clear();
```