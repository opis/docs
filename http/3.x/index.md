---
layout: project
library: http
version: 3.x
title: About
description: Getting started with Opis Http
lib: 
    name: Opis HTTP
    package: opis/http
    version: 3.0
---

**{{page.lib.name}}** is a library that provides an object-oriented representation for HTTP requests and responses.
The library was designed to be small, efficient, and easy to work with.

## License

**{{page.lib.name}}** is licensed under [Apache License, Version 2.0][license].

## Requirements

* PHP ^7.1

## Installation

**{{page.lib.name}}** is available on [Packagist] and it can be installed from a 
command line interface by using [Composer].

```bash
composer require {{page.lib.package}}
```

Or you could directly reference it into your `composer.json` file as a dependency

```json
{
    "require": {
        "{{page.lib.package}}": "^{{page.lib.version}}"
    }
}
```


[license]: http://www.apache.org/licenses/LICENSE-2.0 "Project license" 
{:rel="nofollow" target="_blank"}
[Packagist]: https://packagist.org/packages/{{page.lib.name}} "Packagist" 
{:rel="nofollow" target="_blank"}
[Composer]: http://getcomposer.org "Composer" 
{:ref="nofollow" target="_blank"}