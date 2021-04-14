---
layout: project
library: json-schema
version: 2.x
title: Opis JSON Schema Migration from 1.x to 2.0
description: migration examples
keywords: opis, json, schema, validation, migration
---

Since 2.0 is a major version release, breaking changes are expected.
These changes affect only the PHP part, you don't have to rewrite your JSON schemas.

## Validator

In 1.x there were 3 methods to validate a schema: 
[dataValidation](/json-schema/1.x/php-validator.html#datavalidation),
[uriValidation](/json-schema/1.x/php-validator.html#urivalidation), and
[schemaValidation](/json-schema/1.x/php-validator.html#schemavalidation).

In 2.0 you should use the [validate](php-validator.html#data-validation) method, which is smart enough to detect
what kind of validation are you doing. This method returns a [`ValidationResult`](#validationresult).

The above 3 methods are still present in 2.0, but they return a `ValidationError` for invalid data, 
or `null` if data is valid. We recommend to stick with the `validate` method.

{% capture v2 %}
```php
use Opis\JsonSchema\{
    Validator,
    Helper,
};

$validator = new Validator();

// Data validation
$schema = Helper::toJSON([
    'type' => 'array',
    'items' => [
        'type' => 'integer'
    ]
]);

$result = $validator->validate([5, 6, 7], $schema);

// Uri validation
$result = $validator->validate(
    "some data", 
    "http://example.com/schema.json"
);

// Schema validation
// We cannot create a schema directly,
// it must be parsed first.
$schema = $validator
    ->loader()
    ->loadObjectSchema((object)['type' => 'string']);
$result = $validator->schemaValidation("some data", $schema);
```
{% endcapture %}
{% capture v1 %}
```php
use Opis\JsonSchema\{
    Validator,
    Schema
};

$validator = new Validator();

// Data validation
$schema = (object) [
    'type' => 'array',
    'items' => (object) [
        'type' => 'integer'
    ]
];

$result = $validator->dataValidation([5, 6, 7], $schema);

// Uri validation
$result = $validator->uriValidation(
    "some data", 
    "http://example.com/schema.json"
);

// Schema validation
$schema = Schema::fromJsonString('{"type": "string"}');
$result = $validator->schemaValidation("some data", $schema);
```
{% endcapture %}
{% include tabs.html 1="2.0" 2="1.x" _1=v2 _2=v1 %}

## ValidationResult

The validation result indicates the validity of the data, and also provides the error.
This class contains only 3 methods:

- `isValid()` - returns `true` if the data was successfully validated
- `hasError()` - returns `true` if the data was invalid
- `error()` - returns `ValidationError` if the data was invalid, or `null` if the data was valid

Version 2.0 also brings a few methods to [format error messages](php-error-formatter.html).

{% capture v2 %}
```php
if ($result->isValid()) {
  // Valid ...
}

if ($result->hasError()) {
  // Get the error (there is only one error)
  $error = $result->error();
}
```
{% endcapture %}
{% capture v1 %}
```php
if ($result->isValid()) {
  // Valid ...
}

if ($result->hasErrors()) {
  // Get first error
  $error = $result->getFirstError();
}
```
{% endcapture %}
{% include tabs.html 1="2.0" 2="1.x" _1=v2 _2=v1 %}

## Schema loader

In 1.x the schema loader was used only to resolve an uri to a schema.
Starting with 2.0, the following were added to the loader:

- a [schema resolver](php-loader.html#resolver), that resolves schema content using an uri
- a [schema parser](php-loader.html#parser), to handle the keywords and various resolvers

`Opis\JsonSchema\Loaders\Memory` and `Opis\JsonSchema\Loaders\File` resolvers were merged into one powerful resolver.
Now, you should use the [default resolver `Opis\JsonSchema\Resolvers\SchemaResolver`](php-loader.html#resolver):

- use [registerRaw](php-loader.html#registerraw-method) - to replace the 1.x [Memory loader](/json-schema/1.x/php-loader.html#memory-loader)
- use [registerFile](php-loader.html#registerfile-method) or [registerPrefix](php-loader.html#registerprefix-method) - to replace the 1.x [File loader](/json-schema/1.x/php-loader.html#file-loader)

{% capture v2 %}
```php
use Opis\JsonSchema\Validator;

$validator = new Validator();

$validator->resolver()->registerPrefix(
    "/path/to/schemas",
    "http://example.com/"
);

$result = $validator->validate(
    "some data", 
    "http://example.com/my-schema.json"
);
```
{% endcapture %}
{% capture v1 %}
```php
use Opis\JsonSchema\Validator;

$validator = new Validator();

$loader = new \Opis\JsonSchema\Loaders\File(
    "http://example.com/", 
    ["/path/to/schemas"]
);

$result = $validator->uriValidation(
    "some data", 
    "http://example.com/my-schema.json"
);
```
{% endcapture %}
{% include tabs.html 1="2.0" 2="1.x" _1=v2 _2=v1 %}

If you ever extend the `Opis\JsonSchema\Resolvers\SchemaResolver` class, you must set the new resolver

```php
use Opis\JsonSchema\{
    Validator,
    SchemaLoader,
};
use Opis\JsonSchema\Resolvers\SchemaResolver;

class MySchemaResolver extends SchemaResolver {
    // ...
}

$validator = new Validator(
    new SchemaLoader(null, new MySchemaResolver())
);

// Or
$validator = new Validator();
$validator->loader()->setResolver(new MySchemaResolver());
```
 
## Filters

In 2.0 filters can also be callables.
For more info, please check [custom filters](php-filter.html) page.

{% capture v2 %}
```php
$modulo_filter = function (int $value, array $args): bool {
    $divisor = $args['divisor'] ?? 1;
    $reminder = $args['reminder'] ?? 0;
    return $value % $divisor == $reminder;
};
```
{% endcapture %}
{% capture v1 %}
```php
use Opis\JsonSchema\IFilter;

class ModuloFilter implements IFilter
{
    public function validate($value, array $args): bool {
        $divisor = $args['divisor'] ?? 1;
        $reminder = $args['reminder'] ?? 0;
        return $value % $divisor == $reminder;
    }
}
```
{% endcapture %}
{% include tabs.html 1="2.0" 2="1.x" _1=v2 _2=v1 %}

Registering a filter

{% capture v2 %}
```php
use Opis\JsonSchema\Validator;
use Opis\JsonSchema\Resolvers\FilterResolver;

$validator = new Validator();

/** @var FilterResolver $filters */
$filters = $validator->parser()->getFilterResolver();

$modulo = function (int $value, array $args): bool {
    $divisor = $args['divisor'] ?? 1;
    $reminder = $args['reminder'] ?? 0;
    return $value % $divisor == $reminder;
};

// Register our modulo filter
$filters->registerCallable("integer", "modulo", $modulo);
```
{% endcapture %}
{% capture v1 %}
```php
use Opis\JsonSchema\{
    Validator,
    FilterContainer
};

// Create a new FilterContainer
$filters = new FilterContainer();

// Register our modulo filter
// Class ModuleFilter is defined in the above example
$filters->add("integer", "modulo", new ModuloFilter());

// Create a validator
$validator = new Validator();

// Set filters to be used by validator
$validator->setFilters($filters);
```
{% endcapture %}
{% include tabs.html 1="2.0" 2="1.x" _1=v2 _2=v1 %}

## Formats

In 2.0 formats can also be callables.
For more info, please check [custom formats](php-format.html) page.

{% capture v2 %}
```php
$prime_number = function (int $data): bool {
    if ($data < 2) {
        return false;
    }
    if ($data == 2) {
        return true;
    }
    $max = floor(sqrt($data)) + 1;
    for ($i = 3; $i < $max; $i += 2) {
        if ($data % $i == 0) {
            return false;
        }
    }
    
    return true;
};
```
{% endcapture %}
{% capture v1 %}
```php
use Opis\JsonSchema\IFormat;

class PrimeNumberFormat implements IFormat
{
    public function validate($data): bool {
        if ($data < 2) {
            return false;
        }
        if ($data == 2) {
            return true;
        }
        $max = floor(sqrt($data)) + 1;
        for ($i = 3; $i < $max; $i += 2) {
            if ($data % $i == 0) {
                return false;
            }
        }
        
        return true;
    }
}
```
{% endcapture %}
{% include tabs.html 1="2.0" 2="1.x" _1=v2 _2=v1 %}

Registering a format

{% capture v2 %}
```php
use Opis\JsonSchema\Validator;
use Opis\JsonSchema\Resolvers\FormatResolver;

$validator = new Validator();

/** @var FormatResolver $formats */
$formats = $validator->parser()->getFormatResolver();

// Register our prime number format
$formats->registerCallable("integer", "prime", function (int $value): bool {
    // See the example above for the function contents
});
```
{% endcapture %}
{% capture v1 %}
```php
use Opis\JsonSchema\{
    Validator,
    FormatContainer
};

// Create a new FormatContainer
$formats = new FormatContainer();

// Register our prime format
// Class PrimeNumberFormat is defined in the above example
$formats->add("integer", "prime", new PrimeNumberFormat());

// Create a IValidator
$validator = new Validator();

// Set formats to be used by validator
$validator->setFormats($formats);
```
{% endcapture %}
{% include tabs.html 1="2.0" 2="1.x" _1=v2 _2=v1 %}

## Media types

Starting with 2.0 we added a fallback mechanism that uses `finfo` to auto-detect
the media type of the content.
For more info, please check [custom media types](php-media-type.html) page.

In draft-2020-12 and draft-2019-09 the [`contentMediaType` keyword](string.html#contentmediatype)
is disabled by default. 
You can enable it by setting the [`decodeContent` option](php-loader.html#parser-options).
{:.alert.alert-warning data-title="Important"}

## Content encodings

In 2.0 you can add your custom content encodings.
For more info, please check [custom content encodings](php-content-encoding.html) page.

In draft-2020-12 and draft-2019-09 the [`contentEncoding` keyword](string.html#contentencoding)
is disabled by default.
You can enable it by setting the [`decodeContent` option](php-loader.html#parser-options).
{:.alert.alert-warning data-title="Important"}
