---
layout: project
library: session
version: 4.x
title: About
description: Getting started with Opis Session
lib: 
    name: opis/session
    version: 4.0@dev
---

**Opis Session** is a session manager library with support for multiple backend storage types, 
providing developers with an API which allows them to handle session related information in a standardised way.

## License

**Opis Session** is licensed under [Apache License, Version 2.0][apache_license].

## Requirements

* PHP ^7.0

## Installation

**Opis Session** is available on [Packagist] and it can be installed from a 
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