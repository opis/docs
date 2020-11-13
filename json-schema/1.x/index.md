---
layout: project
library: json-schema
version: 1.x
title: About
description: About Opis JSON Schema validation in PHP
keywords: opis, json, schema, validation, php, about
lib: 
    name: opis/json-schema
    version: 1.0
---

**Opis JSON Schema** is a PHP implementation for the [JSON Schema](http://json-schema.org/){:target="_blank"}
standard (draft-07 and draft-06), that will help you validate all sorts of JSON documents, 
whether they are configuration files or a set of data sent to an RESTful API endpoint.

## Features

- Fast validation
- Support for custom [filters](filters.html)
- Supports relative and absolute [json pointers](pointers.html)
- Support for local and global [variables](variables.html)
- Advanced schema reuse with [mappers](mappers.html)
- Support for custom formats
- Support for custom media types
- Support for default value
- And, of course, all the json schema keywords

## License

**Opis JSON Schema** is licensed under [Apache License, Version 2.0][apache_license].

## Requirements

* PHP 7.0 or higher

## Installation

**Opis JSON Schema** is available on [Packagist] and it can be installed from a 
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

## Documentation

On this site you can find documentation about json schema itself and about [the API of this library](php-intro.html).

### JSON Schema keywords

- Document keywords:
[$schema](structure.html#schema-keyword),
[$id](structure.html#id-keyword)
- Metadata keywords:
[title](structure.html#title),
[description](structure.html#description),
[examples](structure.html#examples),
[$comment](structure.html#comment)
- General keywords:
[type](generics.html#type),
[const](generics.html#const),
[enum](generics.html#enum),
[format](formats.html),
[default](default-value.html),
[definitions](definitions.html),
[$ref](ref-keyword.html)
- Conditionals: 
[not](conditional-subschemas.html#not), 
[if-then-else](conditional-subschemas.html#if-then-else),
[anyOf](multiple-subschemas.html#anyof), 
[oneOf](multiple-subschemas.html#oneof), 
[allOf](multiple-subschemas.html#allof) 
- String keywords:
[minLength](string.html#minlength),
[maxLength](string.html#maxlength),
[pattern](string.html#pattern),
[contentEncoding](string.html#contentencoding),
[contentMediaType](string.html#contentencoding) 
- Number/Integer keywords:
[minimum](number.html#minimum),
[exclusiveMinimum](number.html#exclusiveminimum),
[maximum](number.html#maximum),
[exclusiveMaximum](number.html#exclusivemaximum),
[multipleOf](number.html#multipleof)
- Object keywords:
[properties](object.html#properties),
[required](object.html#required),
[dependencies](object.html#dependencies),
[minProperties](object.html#minproperties),
[maxProperties](object.html#maxproperties),
[propertyNames](object.html#propertynames),
[patternProperties](object.html#patternproperties),
[additionalProperties](object.html#additionalproperties)
- Array keywords:
[minItems](array.html#minitems),
[maxItems](array.html#maxitems),
[uniqueItems](array.html#uniqueitems),
[contains](array.html#contains),
[items](array.html#items),
[additionalItems](array.html#additionalitems)
- Extra keywords:
[$vars](variables.html),
[$filters](filters.html) (including [$func](filters.html))
[$map](mappers.html) (including [$each](mappers.html#mapping-arrays-using-each))

### Schema structure

- [json types](structure.html#data-types)
- [document structure](structure.html#document-structure)
- [URI templates](uri-template.html)
- [json pointers](pointers.html)


[apache_license]: http://www.apache.org/licenses/LICENSE-2.0 "Project license" 
{:rel="nofollow" target="_blank"}
[Packagist]: https://packagist.org/packages/{{page.lib.name}} "Packagist" 
{:rel="nofollow" target="_blank"}
[Composer]: http://getcomposer.org "Composer" 
{:ref="nofollow" target="_blank"}