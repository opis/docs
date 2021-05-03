---
layout: project
library: json-schema
version: 2.x
title: JSON Schema loader by id
description: the opis json schema loader, resolver and parser system
keywords: opis, json, schema, validation, loader, resolver, parser
---

The loader is used to produce a validatable schema by [parsing](#parser) the
contents of a [resolved](#resolver) schema.

A schema is parsed only once, and the result is cached.

So, why do we have to parse the schema contents? 
This is because different drafts contain different keywords,
or the same keyword behaves differently in two distinct drafts.
We also need to find all id's inside a schema, 
to quickly resolve a reference the next time is needed.

```php
use Opis\JsonSchema\{
    Validator,
    SchemaLoader,
};

// Option 1. - Get the loader from the validator

$validator = new Validator();
$loader = $validator->loader();

// Option 2. - Create the loader
$loader= new SchemaLoader();

// Validator constructor accepts a loader as argument
$validator = new Validator($loader);

// Or you can set the loader
$validator->setLoader($loader);
```

## Resolver

Is used to resolve the contents of a schema by uri.

```php
use Opis\JsonSchema\Validator;

$validator = new Validator();

// Get the resolver from schema loader
$resolver = $validator->loader()->resolver();

// Or, get it using the shortcut
$resolver = $validator->resolver();
```

### registerRaw method

```php
public function registerRaw(
    string|object|bool $schema,
    ?string $id = null
): bool
```

Use this method to register an inline raw schema.
You can optionally set the id.

```php
$resolver->registerRaw((object)['type' => 'object']);

$resolver->registerRaw('{"type": "object"}', 'http://example.com/object.json');
```

### registerFile method

```php
public function registerFile(string $id, string $file): self
```

Use this method to register a file as a schema. You must specify the id.

```php
$resolver->regiserFile('http://example.com/schema.json', '/path/to/file.json');
```

### registerPrefix method

```php
public function registerPrefix(string $prefix, string $dir): self
```

Use this method to register a filesystem directory from where to load schema files.
You must specify the id prefix.

```php
$resolver->registerPreifx('/path/to/dir', 'http://example.com/');
```

In the above example we create a loader that maps 
the `/path/to/dir` directory to `http://example.com/` prefix. 

Given that our dir contains

```text
[user]
   create.json
   update.json 
[resume]
   [hobby]
        item.json
   create.json
   update.json
```

the following documents should be available, and should contain the contents of corresponding files

- http://example.com/**user/create.json**
- http://example.com/**user/update.json**
- http://example.com/**resume/create.json**
- http://example.com/**resume/update.json**
- http://example.com/**resume/hobby/item.json**

### registerProtocol method

```php
public function registerProtocol(string $scheme, callable $handler): self
```

Use this method to register a custom protocol handler.

```php
use Opis\JsonSchema\Uri;

$resolver->registerProtocol('my-proto', function (Uri $uri) {
    // ...
});
```

The above example will handle all uris that have the `my-proto` scheme, 
such as `my-proto://example.com`, `my-proto://other.com/path/to/file.json` ...

### registerProtocolDir method

```php
public function registerProtocolDir(
    string $scheme, 
    string $host, 
    ?string $dir
): self
```

This method is similar to [registerPrefix](#registerprefix-method).

```php
$resolver->registerProtocolDir('http', 'example.com', '/path/to/dir');
```

The files resolved from `/path/to/dir` will be prefixed with `http://example.com/`.

## Parser

Holds all registered drafts and keyword parsers. 
Used to create a validatable schema object from a source.

Also, contains different resolvers: [filters](php-filter.html), [formats](php-format.html),
[media types](php-media-type.html), [content encodings](php-content-encoding.html).

```php
use Opis\JsonSchema\Validator;
use Opis\JsonSchema\Resolvers\{
    FilterResolver,
    FormatResolver,
    ContentEncodingResolver,
    ContentMediaTypeResolver,
};

$validator = new Validator();

// Get the parser from schema loader
$parser = $validator->loader()->parser();

// Or, by using the shortcut
$parser = $validator->parser();

// Accessing the resolvers...

/** @var FilterResolver $filter_resolver */
$filter_resolver = $parser->getFilterResolver();

/** @var FormatResolver $format_resolver */
$format_resolver = $parser->getFormatResolver();

/** @var ContentEncodingResolver $encoding_resolver */
$encoding_resolver = $parser->getContentEncodingResolver();

/** @var ContentMediaTypeResolver $media_resolver */
$media_resolver = $parser->getMediaTypeResolver();
```

### Parser options

```php
use Opis\JsonSchema\Validator;

$validator = new Validator();

// set an option
$validator->parser()->setOption($name, $value);
```

If you ever want to use only the features of json-schema (without the useful additions like filters or uri-templates),
just use the settings from the "Vanilla" column.


| Option   |      Default     |   Vanilla   |  Description |
|----------|:-------------:|:------:|------:|
| defaultDraft |  `"2020-12"` |  `"2020-12"` | Sets the default draft to use when [`$schema` keyword](structure.html#schema-keyword) is missing |
| decodeContent |  `["06", "07"]` |  `["06", "07"]` | The draft versions where content related keywords are allowed: [contentEncoding](string.html#contentencoding), [contentMediaType](string.html#contentmediatype), [contentSchema](string.html#contentschema). Use `true` to allow regardless of draft version.  |
| allowFilters |  `true` |  `false` | Enables/disables [`$filters` keyword](filters.html) |
| allowFormats |  `true` |  `true` | Enables/disables [`format` keyword](formats.html) |
| allowMappers |  `true` |  `false` | Enables/disables [`$map` keyword](mappers.html) |
| allowTemplates |  `true` |  `false` | Enables/disables [URI templates](uri-template.html) |
| allowGlobals |  `true` |  `false` | Enables/disables [`$globals` keyword](variables.html#global-variables) |
| allowDefaults |  `true` |  `false` | Enables/disables [`default` keyword](default-value.html) |
| allowSlots |  `true` |  `false` | Enables/disables [`$slots` keyword](slots.html) |
| allowPragmas |  `true` |  `false` | Enables/disables [`$pragma` keyword](pragma.html) |
| allowDataKeyword |  `true` |  `false` | Enables/disables [`$data` keyword](data-keyword.html) |
| allowUnevaluated |  `true` |  `true` | Enables/disables [`unevaluatedProperties` keyword](object.html#unevaluatedproperties) and [`unevaluatedItems` keyword](array.html#unevaluateditems) |
| allowKeywordsAlongsideRef |  `false` |  `false` | If set to `true` then siblings of [`$ref` keyword](references.html#ref) are evaluated in draft-06 and draft-07. In other drafts this is enabled by default. |
| allowRelativeJsonPointerInRef |  `true` |  `false` | If set to `true` then [`$ref` keyword](references.html#ref) will use relative pointers if the value represents a relative pointer. In rare cases, this can collide with paths. (Since v2.1.0). |
| allowExclusiveMinMaxAsBool |  `true` |  `false` | If set to `true` then [`exclusiveMinimum` keyword](number.html#exclusiveminimum) and [`exclusiveMaximum` keyword](number.html#exclusivemaximum) will allow booleans as value. (Since v2.1.0). |
| keepDependenciesKeyword |  `true` |  `false` | If set to `true` then [`dependencies` keyword](object.html#dependencies) will be available for draft-2019-09 and draft-2020-12. (Since v2.1.0). |
| keepAdditionalItemsKeyword |  `true` |  `false` | If set to `true` then [`additionalItems` keyword](array.html#additionalitems) will be available for draft-2020-12. (Since v2.1.0). |
{:.table.table-striped}

