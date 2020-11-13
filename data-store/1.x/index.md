---
layout: project
library: data-store
version: 1.x
title: About
description: Opis Data Store for managing collections of data
keywords: opis, data store, collection, api, driver
lib: 
    name: opis/data-store
    version: 1.0.0
---

**Opis Data Store** is a library for managing collections of data using an unified API
 to access the data values, no matter where they are stored.
 
To achieve this, you can used one of the existing drivers
 - [JSONFile](drivers.html#jsonfile-driver)
 - [PHPFile](drivers.html#phpfile-driver)
 - [File](drivers.html#file-driver)
 - [Dual](dual-driver.html)
 - [Memory](memory-driver.md)
 
or you can [create your own driver](drivers.html).

## License

**Opis Data Store** is licensed under [Apache License, Version 2.0][apache_license].

## Requirements

* PHP 7.0 or higher

## Installation

**Opis Data Store** is available on [Packagist] and it can be installed from a 
command line interface by using [Composer]. 

```bash
composer require {{page.lib.name}}
```

Or you could directly reference it into your `composer.json` file as a dependency

```json
{
    "require": {
        "{{page.lib.name}}": "^{{page.lib.version}}"
    }
}
```


[apache_license]: http://www.apache.org/licenses/LICENSE-2.0 "Project license" 
{:rel="nofollow" target="_blank"}
[Packagist]: https://packagist.org/packages/{{page.lib.name}} "Packagist" 
{:rel="nofollow" target="_blank"}
[Composer]: http://getcomposer.org "Composer" 
{:ref="nofollow" target="_blank"}
