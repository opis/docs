---
layout: project
library: json-schema
version: 1.x
title: JSON Schema structure
description: JSON Schema structure and metadata
keywords: opis, php, json, schema, validation, structure, metadata
---

JSON Schema is a declarative way of writing validation rules. It contains
all the steps that are necessary to be performed in order to validate something.
The format used to write these steps is, of course, 
[JSON](https://www.json.org/){:target="_blank"} itself.

**Opis JSON Schema** is the software/library (for PHP) that performs those steps
and tells you the validation status.

## Data types

Because json schema is written in JSON format, it supports all JSON
types plus an addition: the integer type, which is a subtype of the number type.

- `string` - represents a string/text, e.g. `"a string"`{:.language-json}, `"other string"`{:.language-json}
- `number` - represents an integer or a float, e.g. `-5`{:.language-json}, `10`{:.language-json}, 
`-5.8`{:.language-json}, `10.2`{:.language-json}
- `integer` - represents an integer, e.g. `-100`{:.language-json}, `125`{:.language-json},
 `0`{:.language-json}
- `boolean` - represents a boolean value, e.g. `true`{:.language-json} or `false`{:.language-json}
- `null` - indicates that a value is missing, e.g. `null`{:.language-json}
- `object` - a key-value map, where the key must be a `string` and the
value can be any type, e.g. `{"key": "value", "other-key": 5}`{:.language-json}
- `array` - an ordered list of any data types, e.g. `[1, -2.5, "some string", null]`{:.language-json}

## Document structure

A valid json schema document must be a JSON object or a boolean value.
If it is a boolean, then the validation status is indicated by the value
of the boolean: `true` - valid, `false` - invalid.
If it is an object, then it must contain the steps needed for validation. 
These steps come in the form of **keywords** and every keyword has a specific meaning.
Keywords are applied to data starting from the root of the document schema
and descend to their children.

Here are some examples

```json
true
```
Always valid.
{:.blockquote-footer}

```json
false
```
Always invalid.
{:.blockquote-footer}

```json
{}
```
Always valid because there are no steps defined.
{:.blockquote-footer}

{% capture schema %}
```json
{
  "type": "string"
}
```
Validation status is keyword dependent (in this case) the data is valid
only if it holds a string.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"test"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `123`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

Some keywords are purely decorative, like metadata keywords, which just describe the author intent.
Others are for identifying a document or a subschema, and the rest of them are for
validity checks. Usually keywords work independently and there are only a few
exceptions.

## *$schema* keyword

This keyword is used to specify the desired schema version. 
The value of this keyword must be a string representing an URI.

Currently the supported URIs are:

- `http://json-schema.org/draft-07/schema#` - latest version 
- `http://json-schema.org/draft-06/schema#` - previous version

This keyword is not required and if it is missing, the URI of the 
latest schema version will be used instead.

The only difference between draft 06 and draft 07 is that draft 06 does not
support [if-then-else keywords](conditional-subschemas.html#if-then-else). 

## *$id* keyword

This keyword is used to specify an unique ID for a document or a document subschemas.
The value of this keyword must be a string representing an URI. All subschema
IDs are resolved relative to the document's ID.
It is not a required keyword, but we recommend you using it, as a  best practice.

The usage of this keyword will be covered in the next chapters.

## *definitions* keyword

This keyword does not directly validate data, but it contains a map
of validation schemas. The value of this keyword can be anything.
This keyword is not required.

## Metadata keywords

These keywords are not used for validation, but to describe the
validation schema and how it works. All keywords are optional.

### title

Contains a short description about the validation. The value of this
keyword must be a string.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/number.json#",
  "title": "Test if it is a number",
  
  "type": "number"
}
``` 

### description

Contains a long description about the validation. The value of this
keyword must be a string.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/number.json#",
  "title": "Test if it is a number",
  "description": "A data is considered number if it is an integer or a float.",
  
  "type": "number"
}
``` 

### examples

Contains a list of valid examples. The value of this keyword must be 
an array.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/number.json#",
  "title": "Test if it is a number",
  "description": "A data is considered a number if is an integer or a float.",
  "examples": [-5.10, -2, 0, 5, 8.10],
  
  "type": "number"
}
``` 

### $comment

Contains an observation about the schema. The value of this keyword must be a string.

```json
{
  "$comment": "We should replace this broken regex with format: email.",
  
  "type": "string",
  "pattern": "[a-zA-Z0-9\\.]+@[a-zA-Z0-9]+\\.[a-zA-Z]{2,3}"
}
```

## JSON Schema examples

For most of the basic examples we will not use `$schema`, `$id` and metadata keywords,
but when writing schemas it is recommended to use at least the `$id` keyword.
{:.alert.alert-info data-title="Remember"}

Don't worry if you don't exactly understand every keyword, they are presented
in depth in the next chapters.

### Validating a simple user

{% capture schema %}
```json
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string"
    },
    "email": {
      "type": "string",
      "format": "email"
    },
    "age": {
      "type": "integer",
      "minimum": 18,
      "maximum": 150
    }
  },
  "required": ["email"]
}
```
{% endcapture %}
{% capture examples %}
| Input | Status |
|-------|--------|
| `{"name": "John", "email": "john@example.com", "age": 25}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"email": "john@example.com"}`{:.language-json} | *valid*{:.text-success.text-normal} - only the `email` is required | 
| `{"name": "John", "age": 25}`{:.language-json} | *invalid*{:.text-danger.text-normal} - required `email` is missing |
| `{"name": "John", "email": "john(at)example.com", "age": 25}`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a valid email address |
| `{"email": 123}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `email` must be a string |
| `{"email": "john@example.com", "age": 25.5}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `age` must be an integer |
| `"john@example.com"`{:.language-json} | *invalid*{:.text-danger.text-normal} - must be an object |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### Validating a list

{% capture schema %}
```json
{
  "type": "array",
  "minItems": 2,
  "items": {
    "type": ["string", "number"]
  }
}
```
{% endcapture %}
{% capture examples %}
|-------|--------|
| `[1, "a"]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[-5.1, 10.8, 2]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `["a", "b", "c", "d", 4, 5]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1]` {:.language-json} | *invalid*{:.text-danger.text-normal} - must have at least 2 items |
| `["a", {"x": 1}]`{:.language-json} | *invalid*{:.text-danger.text-normal} - contains an object |
| `{"0": 1, "1": 2}`{:.language-json} | *invalid*{:.text-danger.text-normal} - not an array |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}
