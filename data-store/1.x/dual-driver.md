---
layout: project
library: data-store
version: 1.x
title: Dual data store driver
description: Multiple data store drivers for migration
keywords: opis, data store, multiple, driver, fallback
---

Dual driver is pretty handy when you want to migrate from one data store
driver to another, or when you need a fallback. It features an auto-sync feature
to copy the data from fallback (secondary) driver to primary driver.

The below example illustrates how to smoothly migrate from [PHPFile driver](file-drivers.html#phpfile-driver) 
to [JSONFile driver](file-drivers.html#jsonfile-driver), without manually converting the data.

```php
use Opis\DataStore\Drivers{
    Dual,
    PHPFile,
    JSONFile
};

$primary = new JSONFile("/path/to/json/dir");
$secondary = new PHPFile("/path/to/php/dir");

$dataStore = new Dual($primary, $secondary, true);

// Use data store here ...

```

