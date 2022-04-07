---
layout: project
library: json-schema
version: 2.x
title: Opis JSON Schema Validator Object
description: the opis json schema validator object
keywords: opis, json, schema, validation, validator
---

Creating a new validator instance is easy, just instantiate the `Opis\JsonSchema\Validator` class.

```php
use Opis\JsonSchema\Validator;

$validator = new Validator();
```

## Data validation

To validate some data, we can use the `validate` method, which has the following signature:

```php
public function validate(
    $data, 
    $schema, 
    ?array $globals = null,
    ?array $slots = null
): Opis\JsonSchema\ValidationResult
```

- `$data` is the data you want to validate. [See more info about data types](#data-types)
- `$schema` the JSON schema to use. This can be:
  - a string representing an uri-reference or a json-encoded schema
  - an instance of `Opis\JsonSchema\Schema` or `Opis\JsonSchema\Uri`
    (note: to load schemas over the network, [register a callable](https://opis.io/json-schema/2.x/php-loader.html#registerprotocol-method) responsible for fetching the JSON.)
  - a boolean, or an object (stdClass)
- `$globals` contains [global variables](variables.html#global-variables)
- `$slots` injects [slots](slots.html)

You should reuse the validator instance because
the parsed schemas are cached.
{:.alert.alert-info data-title="Remember"}

Example

```php
use Opis\JsonSchema\{
    Validator,
    ValidationResult,
    Helper,
};

$validator = new Validator();

// Global variables example
// We recommend to use uppercase letters for global variables
$globals = [
    'GLOBAL-A' => 'a',
    'GLOBAL-B' => 'b',
];

// Slots example
$slots = [
    'slot-1' => json_decode('{"type": "string"}', false),
    // You can also use uris to automatically resolve schemas
    'slot-2' => 'http://example.com/schema.json',
];

// Schema as URI example
$schema = 'http://example.com/schema.json';

// Schema as a JSON string example
$schema = <<<'JSON'
{
    "$id": "http://example.com/schema.json",
    
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "minLength": 1,
            "maxLength": 100
        }
    },
    "required": ["name"]
}
JSON;

// Schema as an stdClass object
$schema = (object) [
    '$id' => 'http://example.com/schema.json',
    'type' => 'object',
    'properties' => (object)[
        'name' => (object)[
            'type' => 'string',
            'minLength' => 1,
            'maxLength' => 100,
        ]
    ],
    'required' => ['name']
];

// Schema using toJSON() helper
// in 99.9% of cases you don't have to cast to object
// The helper cannot convert to object an empty php array

$schema = Helper::toJSON([
    '$id' => 'http://example.com/schema.json',
    'type' => 'object',
    'properties' => [
        'name' => [
            'type' => 'string',
            'minLength' => 1,
            'maxLength' => 100,
        ]
    ],
    'required' => ['name']
]);

// Example of data

// integer
$data = 123;
// number
$data = 123.5;
// string
$data = "opis";
// null
$data = null;
// boolean
$data = true; $data =false;
// array
$data = [1, 2, 3];
// object
$data = (object)['name' => 'opis'];
// decoding a JSON string
$data = json_decode('{"name": "opis"}');
// object (using helper)
$data = Helper::toJSON(['name' => 'opis']);

// Validation

/** @var ValidationResult\ $result */
$result = $validator->validate($data, $schema, $globals, $slots);

// Checking if $data is valid

if ($result->isValid()) {
    // Do something...
}

// Checking if there is an error
if ($result->hasError()) {
    // Get the error
    $error = $result->error();
}

```

## Data types

The data you want to validate must NOT be provided as a JSON-encoded string,
because then it will always be considered a string.
Use the following table to map PHP types to JSON types.
{:.alert.alert-warning}

| PHP | JSON |
|---|---|
| [null](http://php.net/manual/en/language.types.null.php){:target="blank"} | [null](null.html) |
| [bool / boolean](http://php.net/manual/en/language.types.boolean.php){:target="blank"} | [boolean](boolean.html) |
| [int / integer](http://php.net/manual/en/language.types.integer.php){:target="blank"} | [integer](integer.html) or [number](number.html) |
| [float / double](http://php.net/manual/en/language.types.float.php){:target="blank"} | [number](number.html) |
| [string](http://php.net/manual/en/language.types.string.php){:target="blank"} | [string](string.html) |
| [array (indexed - without keys)](http://php.net/manual/en/language.types.array.php){:target="blank"} | [array](array.html) |
| [\stdClass](http://php.net/manual/en/reserved.classes.php){:target="blank"} | [object](object.html) |
{:.table.table-striped}

If you have your data as a JSON encoded string, you can use [json_decode](http://php.net/manual/en/function.json-decode.php){:target="blank"}
function to decode it.
{:.alert.alert-info}

If you want to validate data from `$_POST` you can use a helper method to automatically convert
associative arrays to objects. However, we cannot distinguish an indexed array from an associative array
when the array is empty, so you might need to manually apply a cast to object.

```php
use Opis\JsonSchema\Helper;

$data = Helper::toJSON($_POST);
```

## Max errors

You can set how many errors a keyword can generate. Default is `1`.

```php
$validator->setMaxErrors(5);
```

Not all keywords are interested in `maxErrors`, only the keywords that handle
multiple sub-schema validations, such as [`properties`](object.html#properties).

Let's see an example. Our schema is

```json
{
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "minLength": 3
        },
        "age": {
            "type": "integer",
            "minimum": 18
        }
    }
}
```

and, our data is

```json
{"name":  "ab", "age": 10}
```

Let's set `maxErrors` to 1.
The `properties` keyword will validate `name` property, and because there is an error,
and the `maxErrors` is 1, no other validation occurs (`age` property is ignored here).

Example output

```json
{
    "keyword": "properties",
    "message": "The properties must match schema: name",
    "subErrors": [
        {
            "keyword": "minLength",
            "message": "Minimum string length is 3, found 2"
        }
    ]
}
```

If the `maxErrors` is set to 5, then the `age` is also validated (because there are 4 errors left to check).
The `age` property is invalid,  so the `properties` keywords has now 2 sub-errors (and a max of 3 errors left to check).

Example output

```json
{
    "keyword": "properties",
    "message": "The properties must match schema: name, age",
    "subErrors": [
        {
            "keyword": "minLength",
            "message": "Minimum string length is 3, found 2"
        },
        {
            "keyword": "minimum",
            "message": "Number must be greater than or equal to 18"
        }
    ]
}
```

## Exceptions

If you want to catch exceptions thrown by opis/json-schema you should
catch instances of `Opis\JsonSchema\Exceptions\SchemaException`.

```php
use Opis\JsonSchema\Exceptions\SchemaException;

try {
    $result = $validator->validate($data, $schema);
} catch (SchemaException $e) {
    // ...
}
```

| Exception | Description |
|---|---|
| UnresolvedReferenceException | Cannot resolve an uri-reference or a json pointer. |
| UnresolvedFilterException | [Filter](php-filter.html) is not registered. |
| UnresolvedContentEncodingException | [Content encoding](php-content-encoding.html) is not registered. |
| UnresolvedContentMediaTypeException | [Media type](php-media-type.html) is not registered. |
| ParseException | Schema cannot be parsed. |
| InvalidKeywordException | Some keyword contains invalid value. |
| InvalidPragmaException | Some pragma keyword contains invalid value. |
| DuplicateSchemaIdException | Schema contains duplicates for [`$id` keyword](structure.html#id-keyword) (after resolving to base). |
{:.table.table-striped}
