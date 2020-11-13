---
layout: project
library: closure
version: 2.x
title: About
description: About this library
canonical: /closure/3.x/
lib: 
    name: opis/closure
    version: 2.4.0
---

**Opis Closure**  is a library that aims to overcome PHP's limitations 
regarding closure serialization by providing a wrapper that will make all closures serializable. 

**The library's key features:**

- Serialize any closure
- Doesn't use `eval` for closure serialization or unserialization
- Works with any PHP version that has support for closures (Yes, even with PHP 5.3)
- Supports PHP 7.0 syntax
- Handles all variables referenced/imported in `use()` and automatically wraps all referenced/imported closures for proper serialization
- Handles recursive closures
- Handles magic constants like `__FILE__`, `__DIR__`, `__LINE__`, `__NAMESPACE__`, `__CLASS__`, `__TRAIT__`, `__METHOD__` and `__FUNCTION__`
- Automatically resolves all class names used inside the closure
- Track closure's residing source by using the `#trackme` directive
- Simple and very fast parser
- Any error or exception, that might occur when executing an unserialized closure, can be caught and treated properly
- You can serialize/unserialize any closure unlimited times, even those previously unserialized (this is possible because `eval()` is not used for unserialization)
- Handles static closures
- Supports cryptographically signed closures
- Provides a reflector that can give you information about the serialized closure
- Provides an analyzer for *SuperClosure* library
- Automatically detects when the scope and/or the bound object of a closure needs to be serialized in order for the closure to work after deserialization

## License
**Opis Closure** is licensed under the [MIT License (MIT)][mit_license].

## Requirements
* PHP 5.3.0 or higher

## Installation

**Opis Closure** is available on [Packagist] and can be installed using [Composer]. 

```json
{
    "require": {
        "{{page.lib.name}}": "^{{page.lib.version}}"
    }
}
```

If you are unable to use [Composer] you can download the [.tar.gz] or the [.zip]
archive file, extract the content of the archive and include de `autoload.php` file into your project. 

```php
require_once 'path/to/{{page.lib.name}}-{{page.lib.version}}/autoload.php';
```

### SuperClosure support 

Starting with version `2.2.0`, **Opis Closure** is shipped with an analyzer(`Opis\Closure\Analyzer`) which 
aims to provide *Opis Closure*'s parsing precision and speed to [SuperClosure]. 

### Note

Due to PHP limitations, the **Opis Closure** library cannot detect the 
correct closure code if there is more then one closure on a single line. 

```php
// This will NOT work!
$first = function() {return "1st";}; $second = function() {return "2nd";};

// This will work!
$first = function() {return "1st";};
$second = function() {return "2nd";};
```

[mit_license]: http://opensource.org/licenses/MIT "Project license" 
{:rel="nofollow" target="_blank"}
[Packagist]: https://packagist.org/packages/{{page.lib.name}} "Packagist" 
{:rel="nofollow" target="_blank"}
[Composer]: http://getcomposer.org "Composer" 
{:rel="nofollow" target="_blank"}
[.tar.gz]: https://github.com/{{page.lib.name}}/archive/{{page.lib.version}}.tar.gz "{{page.lib.version}}" 
{:data-toggle="tooltip"}
[.zip]: https://github.com/{{page.lib.name}}/archive/{{page.lib.version}}.zip "{{page.lib.version}}" 
{:data-toggle="tooltip"}
[SuperClosure]: https://github.com/jeremeamia/super_closure "SuperClosure" 
{:rel="nofollow" target="_blank"}