---
layout: project
library: json-schema
version: 2.x
title: Creating Opis JSON Schema filters
description: the opis json schema validation using custom filters
keywords: opis, json, schema, validation, filter
---


A filter can be defined in two ways:

- as a callable, with the following signature:
  ```php
  function (mixed $value, array $args = []): bool
  ```
- as class implementing `Opis\JsonSchema\Filter` interface. The `validate` method has the following signature:
  ```php
  public function validate(
    Opis\JsonSchema\ValidationContext $context, 
    Opis\JsonSchema\Schema $schema, 
    array $args = []
  ): bool
  ```

For filters that don't require a validation context you should use a callable - this is in 99.9% of the cases.

## Using filters

You should register your filters before you validate any schema.
When you register a filter you must specify:
- json data type (boolean, number, integer, string, null, array, object)
- name: the name you will use in your schemas
- the filter

```php
<?php

use Opis\JsonSchema\Validator;
use Opis\JsonSchema\Resolvers\FilterResolver;

$validator = new Validator();

/** @var FilterResolver $filters */
$filters = $validator->parser()->getFilterResolver();

$modulo = function (float $value, array $args): bool {
    $divisor = $args['divisor'] ?? 1;
    $reminder = $args['reminder'] ?? 0;
    return $value % $divisor == $reminder;
};

// Register our modulo filter
$filters->registerCallable("number", "modulo", $modulo);

```

Here is an example of schema that uses our modulo filter

```json
{
  "type": "number",
  "$filters": {
    "$func": "modulo",
    "$vars": {
      "divisor": 4,
      "reminder": 3
    }
  }
}
```

This schema validates `7` _(7 % 4 == 3)_ but does not validate `17` _(17 % 4 == 1 != 3)_.

