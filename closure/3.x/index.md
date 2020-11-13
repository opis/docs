---
layout: project
library: closure
version: 3.x
title: About
description: A mature and stable PHP library that can be used to serialize closures
lib: 
    name: opis/closure
    version: 3.1
---

**Opis Closure** is a library that aims to overcome PHP's limitations 
regarding closure serialization by providing a wrapper that will make all closures serializable. 

**The library's key features:**

- Serialize any closure
- Serialize arbitrary objects
- Doesn't use `eval` for closure serialization or unserialization
- Works with any PHP version that has support for closures
- Supports PHP 7.0 syntax
- Handles all variables referenced/imported in `use()` and automatically wraps all referenced/imported closures for
proper serialization
- Handles recursive closures
- Handles magic constants like `__FILE__`, `__DIR__`, `__LINE__`, `__NAMESPACE__`, `__CLASS__`,
`__TRAIT__`, `__METHOD__` and `__FUNCTION__`.
- Automatically resolves all class names, function names and constant names used inside the closure
- Track closure's residing source by using the `#trackme` directive
- Simple and very fast parser
- Any error or exception, that might occur when executing an unserialized closure, can be caught and treated properly
- You can serialize/unserialize any closure unlimited times, even those previously unserialized
(this is possible because `eval()` is not used for unserialization)
- Handles static closures
- Supports cryptographically signed closures
- Provides a reflector that can give you information about the serialized closure
- Provides an analyzer for *SuperClosure* library
- Automatically detects when the scope and/or the bound object of a closure needs to be serialized
in order for the closure to work after deserialization

## License
**Opis Closure** is licensed under the [MIT License (MIT)][mit_license].

## Requirements

* PHP 5.4.0 or higher
 
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

### Migrating from 2.x

If your project needs to support PHP 5.3 you can continue using the `2.x` version
of **Opis Closure**. Otherwise, assuming you are not using one of the removed/refactored classes or features(see 
[CHANGELOG](https://github.com/opis/closure/blob/master/CHANGELOG.md)), migrating to version `3.x` is simply a matter
of updating your `composer.json` file. 

### Semantic versioning

**Opis Closure** follows the [semantinc versioning][SemVer] specifications.

### SuperClosure support 

SuperClosure is now abandoned
{:.alert.alert-danger data-title="Warning"}

**Opis Closure** is shipped with an analyzer(`Opis\Closure\Analyzer`) which 
aims to provide *Opis Closure*'s parsing precision and speed to [SuperClosure]. 


[mit_license]: http://opensource.org/licenses/MIT "Project license" 
{:rel="nofollow" target="_blank"}
[Packagist]: https://packagist.org/packages/{{page.lib.name}} "Packagist" 
{:rel="nofollow" target="_blank"}
[Composer]: http://getcomposer.org "Composer" 
{:rel="nofollow" target="_blank"}
[SemVer]: http://semver.org/ "Semantic versioning"
{:rel="nofollow" target="_blank"}
[SuperClosure]: https://github.com/jeremeamia/super_closure "SuperClosure" 
{:rel="nofollow" target="_blank"}