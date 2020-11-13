---
layout: project
library: data-store
version: 1.x
title: File based data store drivers
description: Using the opis data store with json and php files.
keywords: opis, data store, driver, json, php, serialize
---

These drivers allow you to store and fetch data from filesystem. The first
key from the path indicates the filename (without extension) where the data resides.


## Provided drivers

| Driver | Extension | Readability | Data types | Mentions |
|---|---|---|---|---|
| [JSONFile](#jsonfile-driver) | json | Best | JSON types (int, float, string, bool, null, stdClass object, array) |  - |
| [PHPFile](#phpfile-driver) | php | Good |  PHP standard types (int, float, bool, null, stdClass object, array) |  can use opache |
| [File](#file-driver) | conf |  Bad |  Any serializable data |  - |
{:.table.table-striped}

### JSONFile driver

You can use this driver if you want to store JSON compatible data types.

```php
use Opis\DataStore\Drivers\JSONFile;

$dataStore = new JSONFile("/path/to/dir");

$dataStore->read("config.memory.max");
/*
Attempts to read the value from /path/to/dir/config.json file
using ["config", "memory", "max"] as path
*/

$dataStore->write("settings.defaults.language", "en");
/*
After this write, we will have the file /path/to/dir/settings.json
containing a json like this:
{
  "settings": {
    "defaults": {
      "language": "en"
    }
  }
}
*/
```

### PHPFile driver

If your app is using the data store as a configuration manager,
you can boost the speed by using this driver and enabling opcache.
You'll also keep your configuration files easy to read.

```php
use Opis\DataStore\Drivers\PHPFile;

$dataStore = new PHPFile("/path/to/dir");

$dataStore->read("config.memory.max");
/*
Attempts to read the value from /path/to/dir/config.php file
using ["config", "memory", "max"] as path
*/

$dataStore->write("settings.defaults.language", "en");
/*
After this write, we will have the file /path/to/dir/settings.php
containing somthing like this:
<?php return array(
  "settings" => array(
      "defaults" => array(
          "language" => "en",
      ),
  ),
);
*/
```

### File driver

If you don't care about readability and you need to store any (serializable)
kind of data, then this driver is what you are looking for.

```php
use Opis\DataStore\Drivers\File;

$dataStore = new File("/path/to/dir");

$dataStore->read("config.memory.max");
/*
Attempts to read the value from /path/to/dir/config.conf file
using ["config", "memory", "max"] as path
*/

$dataStore->write("settings.defaults.language", "en");
/*
After this write, we will have the file /path/to/dir/settings.conf
containing somthing like this:
a:1:{s:8:"settings";a:1:{s:8:"defaults";a:1:{s:8:"language";s:2:"en";}}}
*/
```

## Adding a new filesystem based driver

Adding a new filesystem based driver is very easy, just extend the 
`Opis\DataStore\Drivers\AbstractFile` class and implement the abstract methods:
[import](#import) and [export](#export).

### import

Returns the decoded data.

**signature**

```php
/**
 * @param string $data
 * @return mixed
 */
abstract protected function import(string $data);
```

### export

Returns the encoded data.

**signature**

```php
/**
 * @param mixed $data
 * @return string
 */
abstract protected function export($data): string;
```

### Example of custom filesystem based driver

```php
use Opis\DataStore\Drivers\AbstractFile;

class Base64SerializedDriver extends AbstractFile 
{
    /**
     * @param string $dir
     * @param string $prefix Optional
     */
    public function __construct(string $dir, string $prefix = '')
    {
        // We want to use base64conf as file extension
        parent::construct($dir, $prefix, 'base64conf');
    }
    
    /**
     * @inheritdoc
     */
    protected function import(string $data)
    {
        return unserialize(base64_decode($data));
    }
    
    /**
     * @inheritdoc
     */
    protected function export($data): string
    {
        return base64_encode(serialize($data));
    }
}
```