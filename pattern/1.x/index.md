---
layout: project
library: pattern
version: 1.x
title: About
description: Build regex using Opis Pattern
keywords: opis, pattern, regex, builder 
lib: 
    name: opis/pattern
    version: 1.0.0
---

**Opis Pattern** is a small library which helps you convert patterns to
regex expressions.

**Features**

- optimized and valid regex
- auto escape for [raw data](pattern.html#data)
- highly customizable [patterns](pattern.html)
- [optional](pattern.html#optional-placeholder) placeholders
with [capture](options.html#capture_mode) for adjacent [separators](options.html#separator_symbol)
- [inline regex](pattern.html#inline-regex) for placeholders


## License
**Opis Pattern** is licensed under the [Apache License, Version 2.0][apache_license].

## Requirements

* PHP 7.0.0 or higher

## Installation

**Opis Pattern** is available on [Packagist] and can be installed
  from a command line interface by using [Composer]. 

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
