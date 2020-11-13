---
layout: project
library: json-schema
version: 1.x
title: Internal structure of a json schema document
description: the opis json schema validation internal structure of document
keywords: opis, json, schema, validation, document, internal, structure
---

Opis JSON Schema uses an internal representation of a schema document,
which must implement `\Opis\JsonSchema\ISchema` interface.

Opis JSON Schema provides a class that implements the `ISchema` interface in
class `\Opis\JsonSchema\Schema`.

The following methods are implemented.

## id()

Used to get the document id.

**Returns** `string`

## draft()

Used to get the document draft.

**Returns** `string`

## resolve()

Used to get a subschema by id. If `null` is used, returns the root schema.

**Arguments**

- [`string` $id = `null`] - subschema id

**Returns** `mixed`