---
layout: project
library: routing
version: 5.x
title: About
description: Getting started with Opis Routing
lib: 
    name: opis/routing
    version: 5.0
---

**Opis Routing** is a framework for building various components that need routing capabilities. 
As a routing framework, it may handle all sorts of paths that are following a pattern, 
not being limited to HTTP request.

## License

**Opis Routing** is licensed under [Apache License, Version 2.0][apache_license].

## Requirements

* PHP 7.0 or higher
* [Opis Closure] ^3.0
* [Opis Pattern] ^3.0

## Installation

**Opis Routing** is available on [Packagist] and it can be installed from a 
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
[Opis Closure]: https://www.opis.io/closure "Opis Closure"
[Opis Pattern]: https://www.opis.io/pattern "Opis Pattern"