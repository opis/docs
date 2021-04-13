---
layout: project
library: json-schema
version: 2.x
title: Creating Opis JSON Schema content encodings
description: the opis json schema validation using custom content encodings
keywords: opis, json, schema, validation, content, encoding, base64
---

A content encoding can be defined in two ways:

- as a callable, with the following signature:
  ```php
  function (string $value, string $type): ?string
  ```
- as class implementing `Opis\JsonSchema\ContentEncoding` interface. The `decode` method has the following signature:
  ```php
  public function decode(string $value, string $type): ?string
  ```

So, if you can decode the value you should return a string representing the decoded value,
otherwise you should return `null`.

## Using content encodings

You should register your content encodings before you validate any schema.
If you want, you can also create a default content encoding handler, so you
can use a generic lib to decode.

```php
<?php

use Opis\JsonSchema\Validator;
use Opis\JsonSchema\Resolvers\ContentEncodingResolver;

$validator = new Validator();

/** @var ContentEncodingResolver $contentEncodings */
$contentEncodings = $validator->parser()->getContentEncodingResolver();

// Register a content encoding
$contentEncodings->registerCallable('my-encoding', function (string $content): ?string {
    // return a string representing the decoded $content
    // or null if you cannot decode 
    return null;
});

// Register the default handler

$contentEncodings->setDefaultHandler(function (string $content, string $encoding): ?string {
    // return a string representing the decoded $content which is encoded as $encoding
    // or null if you cannot decode
    return null;
});

```
