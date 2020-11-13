---
layout: project
library: data-store
version: 1.x
title: How to use
description: Learn how to use Opis Data Store library
keywords: opis, data store, how to use
---

For the next examples, we will use the [memory driver](memory-driver.html), which
is initialized as below

```php
use Opis\DataStore\Drivers\Memory;

$dataStore = new Memory([
    "admin" => [
        "username" => "admin",
        "extra" => [
            "fullname" => "John Doe"
        ]
    ]
]);
```

## Reading a value

You can read values by using the [read method](drivers.html#read).

```php
echo $dataStore->read("admin.username"); // admin
echo $dataStore->read("admin.language", "en"); // en
```

## Checking if a value is present

You can check for data presence by using [has method](drivers.html#has).

```php
if ($dataStore->has("admin.extra.fullname")) {
    // ...
}
```

## Adding or changing values

You can write new values, or overwrite existing ones, by using the [write method](drivers.html#write).

```php
$dataStore->write("admin.language", "en-US");
echo $dataStore->read("admin.language"); // en-US

$dataStore->write("admin.extra", [
    "fullname" => "Anonymous"
]);

echo $dataStore->read("admin.extra.fullname"); // Anonymous
```

## Removing values

You can remove values by using the [delete method](drivers.html#delete).

```php
$dataStore->delete("admin.extra");

if ($dataStore->has("admin.extra")) {
    // This should never execute, because we just deleted the value
}
```



