---
layout: project
library: json-schema
version: 2.x
title: Creating Opis JSON Schema formats
description: the opis json schema validation using custom formats
keywords: opis, json, schema, validation, format
---

A format can be defined in two ways:

- as a callable, with the following signature:
  ```php
  function (mixed $value): bool
  ```
- as class implementing `Opis\JsonSchema\Format` interface. The `validate` method has the following signature:
  ```php
  public function validate(mixed $value): bool
  ```

## Using formats

You should register your formats before you validate any schema.
When you register a format you must specify:
- JSON data type (number, integer, string, array, object)
- name: the name you will use in your schemas
- the format

```php
<?php

use Opis\JsonSchema\Validator;
use Opis\JsonSchema\Resolvers\FormatResolver;

$validator = new Validator();

/** @var FormatResolver $formats */
$formats = $validator->parser()->getFormatResolver();

$isPrime = function (int $value): bool {
    if ($value < 2) {
        return false;
    }
    
    if ($value == 2) {
        return true;
    }
    
    $max = floor(sqrt($value)) + 1;
    
    for ($i = 3; $i < $max; $i += 2) {
        if ($value % $i == 0) {
            return false;
        }
    }
    
    return true;
};

// Register our prime number format
$formats->registerCallable("integer", "prime", $isPrime);

```

Here is an example that uses our prime number format

```json
{
  "type": "integer",
  "format": "prime"
}
```

This schema validates `5`, but does not validate `9` (3 * 3).
