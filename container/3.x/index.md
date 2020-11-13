---
layout: project
library: container
version: 3.x
title: About
description: Getting started with Opis Container
lib: 
    name: opis/container
    version: 3.0
---

**Opis Container** provides a fully serializable dependency injection container that will allow you to avoid being 
tightly coupled to a specific implementation of a dependency.

## License

**Opis Container** is licensed under [Apache License, Version 2.0][apache_license].

## Requirements

* PHP ^7.0
* [Opis Closure] ^3.0
* [PSR Container] ^1.0

## Installation

**Opis Container** is available on [Packagist] and it can be installed from a 
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
[Opis Closure]: https://opis.io/closure "Opis Closure"
{:target="_blank"}
[PSR Container]: https://github.com/php-fig/container "PSR Container"
{:ref="nofollow" target="_blank"}