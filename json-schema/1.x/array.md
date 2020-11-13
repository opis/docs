---
layout: project
library: json-schema
version: 1.x
title: Array type
description: php opis json schema validation of arrays
keywords: opis, php, json, schema, array, validation
---

The `array` type is used for validating JSON indexed arrays.

{% capture schema %}
```json
{
  "type": "array"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `[]`{:.language-json} | *valid*{:.text-success.text-normal} - empty array |
| `[2, 1, "str", false, null, {}]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `12`{:.language-json} | *invalid*{:.text-danger.text-normal} - is integer/number|
| `null`{:.language-json} | *invalid*{:.text-danger.text-normal} - is null|
| `"[1, 2, 3]"`{:.language-json} | *invalid*{:.text-danger.text-normal} - is string|
| `{"0": 1, "1": 2, "2": 3}`{:.language-json} | *invalid*{:.text-danger.text-normal} - is object|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


## Validation keywords

The following keywords are supported by the `array` type, and evaluated
in the presented order. All keywords are optional.

### minItems

An array is valid against this keyword, if the number of items it contains 
is greater than, or equal to, the value of this keyword.
The value of this keyword must be a non-negative integer.


{% capture schema %}
```json
{
  "type": "array",
  "minItems": 2
}
```

Array must have at least `2` items.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `[1, 2, 3]`{:.language-json} | *valid*{:.text-success.text-normal} - contains more than 2 items |
| `["a", "b"]`{:.language-json} | *valid*{:.text-success.text-normal} - contains 2 items|
| `["text"]`{:.language-json} | *invalid*{:.text-danger.text-normal} - contains a single item|
| `[]`{:.language-json} | *invalid*{:.text-danger.text-normal} - contains no items|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### maxItems

An array is valid against this keyword, if the number of items it contains
is lower than, or equal to, the value of this keyword.
The value of this keyword must be a non-negative integer.

{% capture schema %}
```json
{
  "type": "array",
  "maxItems": 2
}
```

Array can have at most `2` items.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `[1, 2]`{:.language-json} | *valid*{:.text-success.text-normal} - contains 2 items |
| `["a"]`{:.language-json} | *valid*{:.text-success.text-normal} - contains a single item|
| `[]`{:.language-json} | *valid*{:.text-success.text-normal} - contains no items|
| `[1, 2, 3]`{:.language-json} | *invalid*{:.text-danger.text-normal} - contains more than 2 items|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### uniqueItems

An array is valid against this keyword if an item cannot be found
more than once in the array.
The value of this keyword must be a boolean. If set to `false` the keyword
validation will be ignored.

{% capture schema %}
```json
{
  "type": "array",
  "uniqueItems": true
}
```

Array must have unique items (for every data type).
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `[1, 2, 3]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `["a", "b", "c"]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1, "1"]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[[1, 2], [3, 4]]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1, 2, 1]`{:.language-json} | *invalid*{:.text-danger.text-normal} - duplicate `1`|
| `["a", "b", "B", "a"]`{:.language-json} | *invalid*{:.text-danger.text-normal} - duplicate `a`|
| `[[1, 2], [1, 3], [1, 2]]`{:.language-json} | *invalid*{:.text-danger.text-normal} - duplicate `[1, 2]`|
| `[{"a": 1, "b": 2}, {"a": 1, "c": 2}, {"a": 1, "b": 2}]`{:.language-json} | *invalid*{:.text-danger.text-normal} - duplicate `{"a": 1, "b": 2}`|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### contains

An array is valid against this keyword if at least one item is valid against
the schema defined by the keyword value.
The value of this keyword must be a valid json schema (object or boolean).

Please note that an empty array will never be valid against this keyword.
{:.alert.alert-info data-title="Important"}

{% capture schema %}
```json
{
  "type": "array",
  "contains": {
    "type": "integer"
  }
}
```

Array must contain at least one integer.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `[1]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1, 2]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `["a", "b" -4.0]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `["a", "b", "1"]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `[2.3, 4.5, -6.7]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### items

An array is valid against this keyword if items are valid against the
corresponding schemas provided by the keyword value. The value of
this keyword can be
- a valid json schema (object or boolean), then every item must be valid
against this schema
- an array of valid json schemas, then each item must be valid against
the schema defined at the same position (index). Items that don't have a corresponding
position (array contains 5 items and this keyword only has 3) 
will be considered valid, unless the [`additionalItems` keyword](#additionalitems)
is present - which will decide the validity.

{% capture schema %}
```json
{
  "type": "array",
  "items": {
    "type": "integer",
    "minimum": 0
  }
}
```

Array must contain only positive integers.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `[1, 2, 3]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[-0, 2.0]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[-2, 3, 4]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `["a", 2]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


{% capture schema %}
```json
{
  "type": "array",
  "items": [
    {"type": "integer"},
    {"type": "string"}
  ]
}
```

First item of the array must be an integer and the second a string.
Other items can be anything.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `[1, "a"]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1.0, "a", 5.6, null, true]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `["a", 1]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `[5.5, "a"]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `[5, 6]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### additionalItems

An array is valid against this keyword if all _unchecked_ items
are valid against the schema defined by the keyword value.
An item is considered _unchecked_ if [`items` keyword](#items) contains
an array of schemas and doesn't have a corresponding position (index).
If the `items` keyword is not an array, then this keyword is ignored.
The value of the keyword must be a valid json schema (object, boolean).


{% capture schema %}
```json
{
  "type": "array",
  "items": [
    {"type": "integer"},
    {"type": "string"}
  ],
  "additionalItems": {
    "type": "boolean"
  }
}
```

First item of the array must be an integer and the second a string.
Other items can only be booleans.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `[1, "a", true, false, true, true]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1, "a"]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1, "a", 2]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `[1, "a", true, 2, false]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `[1, true, false]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}
