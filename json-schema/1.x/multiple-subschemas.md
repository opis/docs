---
layout: project
library: json-schema
version: 1.x
title: Applying multiple subschemas
description: php opis json schema applyingmultiple subschemas with allOf, anyOf, oneOf
keywords: opis, php, json, schema, multiple subschemas, allOf, anyOf, oneOf
---

It is a common practice to validate something against multiple subschemas,
and the following keywords will help you combine subschemas into one validator.

## Validation keywords

The following keywords are supported by any instance type, and are evaluated in
the presented order. All keywords are optional.

### anyOf

An instance is valid against this keyword if is valid against **at least one** schema
defined by the value of this keyword. 
The value of this keyword must be an array of valid json schemas (objects or booleans).

Please note that Opis JSON Schema will stop checking other subschemas once
a subschema validates the instance. This is done for performance reasons.
{:.alert.alert-info data-title="Important"}


{% capture schema %}
```json
{
  "type": "array",
  "anyOf": [
    {
      "contains": {"const": 0}
    },
    {
      "contains": {"const": "ok"}
    }
  ]
}
```

The array is valid if contains `0` or `"ok"`.  Am empty array is not valid.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `["a", 1, 0, 2]`{:.language-json} | *valid*{:.text-success.text-normal} - one subschema match |
| `["a", 0, "ok", 2]`{:.language-json} | *valid*{:.text-success.text-normal} - two subschemas match |
| `["a", "b"]`{:.language-json} | *invalid*{:.text-danger.text-normal} - no subschema match |
| `[]`{:.language-json} | *invalid*{:.text-danger.text-normal} - no subschema match |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


Please pay attention when using `anyOf`!
You could unintentionally write schemas that never validate!
{:.alert.alert-warning data-title="Important"}

```json
{
  "type": "string",
  "anyOf": [
    {"const": 0},
    {"const": 1}
  ]
}
```
This schema will never validate
{:.blockquote-footer}

### oneOf

An instance is valid against this keyword if is valid against **exactly one** schema
defined by the value of this keyword. 
The value of this keyword must be an array of valid json schemas (objects or booleans).

Please note that Opis JSON Schema will stop checking other subschemas once
two subschemas validate the instance. This is done for performance reasons.
{:.alert.alert-info data-title="Important"}

{% capture schema %}
```json
{
  "type": "array",
  "items": {
    "type": "number"  
  },
  "oneOf": [
    {
      "items": {
        "exclusiveMinimum": 0
      }
    },
    {
      "items": {
        "exclusiveMaximum": 0
      }
    },
    {
      "items": {
        "const": 0
      }
    }
  ]
}
```

The array is valid in one of these cases: contains only positive numbers,
contains only negative numbers, or contains only zeroes. 
Am empty array is not valid.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `[1, 2, 3]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[-1, -2, -3]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[0, -0, 0.0]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[-1, 1]`{:.language-json} | *invalid*{:.text-danger.text-normal} - two subschemas match |
| `[-1, 0]`{:.language-json} | *invalid*{:.text-danger.text-normal} - two subschemas match |
| `[1, 0]`{:.language-json} | *invalid*{:.text-danger.text-normal} - two subschemas match |
| `[-1, 0, 1]`{:.language-json} | *invalid*{:.text-danger.text-normal} - three subschemas match |
| `[]`{:.language-json} | *invalid*{:.text-danger.text-normal} - no subschema match |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


Please pay attention when using `oneOf`!
You could unintentionally write schemas that never validate!
{:.alert.alert-warning data-title="Important"}

```json
{
  "oneOf": [
    {"const": 0},
    {"enum": [0, 1, 2]}
  ]
}
```
This schema will never validate
{:.blockquote-footer}

### allOf

An instance is valid against this keyword if is valid against **all** schemas
defined by the value of this keyword. 
The value of this keyword must be an array of valid json schemas (objects or booleans).

{% capture schema %}
```json
{
  "allOf": [
    {"minLength": 2},
    {"pattern": "^a"}
  ]
}
```

If instance is a `string` then it must have a minimum length of `2` and start with `a`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"abc"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"ab"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `2`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1, 2, 3]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"a"`{:.language-json} | *invalid*{:.text-danger.text-normal} - length is `1` |
| `"Ab"`{:.language-json} | *invalid*{:.text-danger.text-normal} - must start with an `a` |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


Please pay attention when using `allOf`!
You could unintentionally write schemas that never validate!
{:.alert.alert-warning data-title="Important"}

```json
{
  "allOf": [
    {"type": "string"},
    {"type": "number"}
  ]
}
```
This schema will never validate
{:.blockquote-footer}