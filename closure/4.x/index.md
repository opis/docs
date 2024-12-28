---
layout: project
library: closure
version: 4.x
title: About
description: A mature and stable PHP library that can be used to serialize closures and arbitrary data
lib: 
    name: opis/closure
    version: 4.0
---

**Opis Closure** is a PHP library that allows you to serialize closures and arbitrary data.

**The library's key features:**

- Serialize arbitrary objects
- Handles circular references
- Custom serialization/deserialization integrations
- Implementation for SPL Classes (ArrayObject, SplObjectStorage, etc.)
- Supports cryptographically signed data
- Serialize closures (anonymous functions)
  - PHP 8+ syntax
  - namespace and imports
  - attributes, parameter types, return type
  - use variables (including self-references)
  - bound object (`$this`) and scope
  - static and short closures
  - anonymous classes inside closures
  - magic constants like `__FILE__`, `__DIR__`, `__LINE__`, `__NAMESPACE__`, `__CLASS__`,
  `__TRAIT__`, `__METHOD__` and `__FUNCTION__`.
  - track residing source by using the `#trackme` directive
  - any error or exception, that might occur when executing an unserialized closure, can be caught and treated properly
  - simple and very fast parser
  - you can serialize/unserialize any closure unlimited times, even those previously unserialized
    (this is possible because `eval()` is not used for unserialization)
  - provides a reflector that can give you information about the serialized closure
  - the serialized source code is now almost identical to original

## License
**Opis Closure** is licensed under the [MIT License (MIT)][mit_license].

## Requirements

* PHP 8.0 or higher

## Migrating from 3.x

If your projects needs to support PHP 7.x you can continue using the `3.x` version
of **Opis Closure**. Otherwise, see the [migration guide](./migrate.html).
 
## Installation

**Opis Closure** is available on [Packagist] and it can be installed from a 
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

[mit_license]: http://opensource.org/licenses/MIT "Project license" 
{:rel="nofollow" target="_blank"}
[Packagist]: https://packagist.org/packages/{{page.lib.name}} "Packagist" 
{:rel="nofollow" target="_blank"}
[Composer]: http://getcomposer.org "Composer" 
{:rel="nofollow" target="_blank"}
