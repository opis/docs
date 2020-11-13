---
layout: project
library: cache
version: 4.x
title: Using existing cache drivers
description: Using existing PSRSimpleCache (PSR16) drivers with Opis Cache
keywords: opis, cache, psr, psrsimplecache, psr16, driver
---

We know that there are already a ton of existing cache drivers,
so creating a new one will not always make sense.

To use an existing cache driver with Opis Cache all you
have to do is to use the `Opis\Cache\Drivers\PsrSimpleCacheAdapter` adapter class.

```php
use Opis\Cache\Drivers\PsrSimpleCacheAdapter;

$psrCache = new SomeExistingCacheDriver;

$cache = new PsrSimpleCacheAdapter($psrCache);
```

A list will all PSR16 cache drivers can be found [here](https://packagist.org/search/?tags=psr-16){:rel="nofollow" target="_blank"}.



