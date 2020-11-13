---
layout: project
library: events
version: 5.x
title: About
description: Documentation for Opis Events library
keywords: opis, events, library, dispatch, documentation, docs
lib: 
    name: opis/events
    version: 5.0
---

**Opis Events** is library used to dispatch and intercept events.

## License
**Opis Events** is licensed under the [Apache License, Version 2.0][apache_license].

## Requirements
* PHP ^7.0
* [Opis Routing] ^5.0

## Installation

**Opis Events** is available on [Packagist]
 and can be installed from a command line interface by using [Composer].

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
{:rel="nofollow" target="_blank"}
[Opis Routing]: /routing  "Opis Routing ^5.0" 
{:data-toggle="tooltip"}
