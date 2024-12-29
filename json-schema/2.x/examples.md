---
layout: project
library: json-schema
version: 2.x
title: Opis JSON Schema examples
description: various examples using opis json schema
keywords: opis, json, schema, examples
---

## Validating form data

JsonSchema is used to validate json data, but you can also validate form data.
You just have to convert the `$_POST` or `$_GET` to a JSON decoded value 
by using `Opis\JsonSchema\Helper::toJSON()`.

```php
use Opis\JsonSchema\{
    Validator,
    ValidationResult,
    Errors\ErrorFormatter,
};

$schema = <<<'JSON'
{
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "minLength": 2
        },
        "email": {
            "type": "string",
            "format": "email"
        }
    },
    "required": ["name", "email"]
}
JSON;

// assuming data is coming from $_POST
// you can also use $_GET, or a custom array
$data = Helper::toJSON($_POST);

// Create a new validator
$validator = new Validator();

/** @var ValidationResult $result */
$result = $validator->validate($data, $schema);

if ($result->isValid()) {
    echo "Valid", PHP_EOL;
} else {
    // Print errors
    print_r((new ErrorFormatter())->format($result->error()));
}

```

When converting to JSON, we cannot distinguish an indexed array from an associative array
when the array is empty, so you might need to manually apply a cast to object if you want the empty array 
to be treated as an empty object.
{:.alert.alert-warning data-title="Important"}

Due to the nature of JsonSchema, you cannot get all errors for a form, usually the validation will stop at first error
for performance reasons. See [max errors](php-validator.html#max-errors) for more details.
{:.alert.alert-info data-title="Info"}

## Custom error messages

You can add custom error messages inside schema. The schema from the [above example](#validating-form-data) can
be augmented with [`$error` keyword](errors.html)

```json
{
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "minLength": 2,
            "$error": {
              "minLength": "Name must have at least two letters",
              "*": "Invalid name value"
            }
        },
        "email": {
            "type": "string",
            "format": "email",
            "$error": "Invalid email"
        }
    },
    "required": ["name", "email"],
    "$error": {
      "required": {
        "name": "The name field is mandatory",
        "email": "We really need an email address"
      }
    }
}
```