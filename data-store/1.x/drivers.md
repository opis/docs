---
layout: project
library: data-store
version: 1.x
title: Create Data Store drivers
description: Creating a new data store driver
keywords: opis, data store, driver, configuration
---

A data store driver is nothing more than an object 
implementing `Opis\DataStore\IDataStore` interface.

## Methods

### read

Returns the value fetched by the path, 
or a default value if the path don't point to a value.

**signature**

```php
/**
 * @param string|string[] $path
 * @param mixed $default Optional
 * @return mixed
 */
public function read($path, $default = null);
```

**usage**

```php
$dataStore->read(["path", "to", "value"], "default value");
$dataStore->read("path.to.value");
```

### write

Sets a new value for specified path.

**signature**

```php
/**
 * @param string|string[] $path
 * @param mixed $value
 * @return bool
 */
public function write($path, $value): bool;
```

**usage**

```php
$dataStore->write(["path", "to", "value"], "new value");
$dataStore->write("path.to.value", [1, 2, 3]);
```

### delete

Removes the value associated with the specified path.

**signature**

```php
/**
 * @param string|string[] $path
 * @return bool
 */
public function delete($path): bool;
```

**usage**

```php
$dataStore->delete(["path", "to", "value"]);
$dataStore->delete("path.to.value");
```

### has

Checks if the path points to a value.

**signature**

```php
/**
 * @param string|string[] $path
 * @return bool
 */
public function has($path): bool;
```

**usage**

```php
if ($dataStore->has(["path", "to", "value"])) {
  // ...
}

if ($dataStore->has("path.to.value")) {
  // ...
}
```

## Paths

The `path` parameter from every [method](#methods) can be a string or an array of string.
When the path is a string, you'll probably convert it to an array of strings that will
help you easily descend to the requested value.


If you want to use "dot notation" for paths you can use `Opis\DataStore\PathTrait`
trait inside your driver class.
{:.alert.alert-info}

However, there are cases when you'll have to use an array. Considering
that we use the dot notation and we have the following data:
```json
{
  "sites": {
    "example.com": {
      "status": "active"
    }
  }
}
```

to get the status for `example.com` we must use an array, because the key also contains
dots.

```php
// Returns "active"
$dataStore->read(["sites", "example.com", "status"]);

// Returns null
$dataStore->read("sites.example.com.status");
```

This is because when converting `sites.example.com.status` to array using
dot notation, we get the following path: `[sites, example, com, status]`.