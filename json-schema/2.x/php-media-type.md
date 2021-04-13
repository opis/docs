---
layout: project
library: json-schema
version: 2.x
title: Creating Opis JSON Schema media types (MIME)
description: the opis json schema validation using custom media/mime types
keywords: opis, json, schema, validation, media, mime
---

A media type can be defined in two ways:

- as a callable, with the following signature:
  ```php
  function (string $content, string $media_type): bool
  ```
- as class implementing `Opis\JsonSchema\ContentMediaType` interface. The `validate` method has the following signature:
  ```php
  public function validate(string $content, string $media_type): bool
  ```


## Using media types

You should register your media types before you validate any schema.
If you want, you can also create a default media type handler so you
can use a generic lib to validate media types.

```php
<?php

use Opis\JsonSchema\Validator;
use Opis\JsonSchema\Resolvers\ContentMediaTypeResolver;

$validator = new Validator();

/** @var ContentMediaTypeResolver $mediaTypes */
$mediaTypes = $validator->parser()->getMediaTypeResolver();

// Register a media type
$mediaTypes->registerCallable('application/example', function (string $content): bool {
    //return true if $content is an application/example
    return false;
});

// Register the default handler

$mediaTypes->setDefaultHandler(function (string $content, string $media_type): bool {
    // return true if $content is an $media_type
    return false;
});

```
