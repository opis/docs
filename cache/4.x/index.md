---
layout: project
library: cache
version: 4.x
title: About
description: Getting started with Opis Cache
lib: 
    name: opis/cache
    version: 4.0
---

**Opis Cache** is a library that helps you work with cached content, 
which can be stored and retrieved by using one of the provided cache drivers. 

You can create your own cache driver by simply implementing an interface.

The currently supported cache drivers are: File, Memory, and PHPFile,
but we also provide an adapter class so you can use any PsrSimpleCache (PSR16) implementation as a cache driver.

## License

**Opis Cache** is licensed under [Apache License, Version 2.0][apache_license].

## Requirements

* PHP ^7.0

## Installation

**Opis Cache** is available on [Packagist] and it can be installed from a 
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