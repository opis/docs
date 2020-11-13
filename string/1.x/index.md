---
layout: project
library: string
version: 1.x
title: About
description: Working with multibyte encoded strings in PHP
keywords: opis, string, multibyte, unicode, utf-8, encoding
lib: 
    name: opis/string
    version: 1.5
---

**Opis String** is a tiny library that allows you to work with multibyte encoded strings in an object-oriented manner. 
The library has no dependencies to *mb_string* or similar PHP extensions.

## License

**Opis String** is licensed under [Apache License, Version 2.0][apache_license].

## Requirements

* PHP ^7.0

## Installation

**Opis String** is available on [Packagist] and it can be installed from a 
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