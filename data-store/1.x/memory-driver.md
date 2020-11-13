---
layout: project
library: data-store
version: 1.x
title: Memory data store driver
description: Using the opis data store with an in-memory driver.
keywords: opis, data store, driver, memory
---

Use this driver if you have data that doesn't require persistence, or
if you need to test your app. You can initialize this driver with predefined
data.

```php
use Opis\DataStore\Drivers\Memory;

$dataStore = new Memory([
    'settings' => [
        'language' => 'en'
    ]
]);

echo $dataStore->read("settings.language"); //> en
echo $dataStore->read("settings.currency", "$"); //> $
```