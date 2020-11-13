---
layout: project
library: orm
version: 1.x
title: About
lib: 
    name: opis/orm
    version: 1.0
---

This documentation is not yet 100% completed, 
but it manages to cover the most important topics
{:.alert.alert-info}

**Opis ORM** is a library, that provides an implementation for the Data Mapper pattern,
built with the following goals in mind: simplicity, productivity, and smooth learning curve.

We wanted to build something that is easy to grasp and easy to use: no annotations, no XML configurations; 
just plain, comprehensible PHP code - and we believe *Opis ORM* delivers that.

## License
**Opis ORM** is licensed under the [Apache License, Version 2.0][apache_license].

## Requirements
* PHP ^7.0
* [Opis Database] ^4.0

## Installation

**Opis ORM** is available on [Packagist] and can be installed using [Composer]. 

```bash
composer require {{page.lib.name}}
```

Or directly in your `composer.json` file

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
[Opis Database]: /database  "Opis Database ^4.0" 
{:data-toggle="tooltip"}
