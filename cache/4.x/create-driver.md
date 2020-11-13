---
layout: project
library: cache
version: 4.x
title: Creating a new cache driver
description: Creating a custom cache driver for Opis Cache
keywords: opis, cache, driver, custom
---

A new driver is simply an implementation for the `Opis\Cache\CacheInterface` interface.

Since the `load` method is just a combination of `has`, `read` and `write` methods,
we also provide the `Opis\Cache\LoadTrait` trait for it. Just remember this if you decide to
create a custom driver.
{:.alert.alert-info}

```php
interface CacheInterface
{
    /**
     * Read from cache
     * @param string $key
     * @return mixed|false
     */
    public function read(string $key);
    
    /**
     * Write to cache
     * @param string $key
     * @param $data
     * @param int $ttl
     * @return bool
     */
    public function write(string $key, $data, int $ttl = 0): bool;
    
    /**
     * Load an item from cache
     * @param string $key
     * @param callable $loader
     * @param int $ttl
     * @return mixed
     */
    public function load(string $key, callable $loader, int $ttl = 0);
    
    /**
     * Delete from cache
     * @param string $key
     * @return bool
     */
    public function delete(string $key): bool;
    
    /**
     * Check if the cache entry exists
     * @param string $key
     * @return bool
     */
    public function has(string $key): bool;
    
    /**
     * Clear cache
     * @return bool
     */
    public function clear(): bool;
}
```

