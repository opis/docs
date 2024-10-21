---
layout: project
library: json-schema
version: 2.x
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

{% include drafts.html v="all" %}

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

{% include drafts.html v="all" %}

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

{% include drafts.html v="all" %}

An array is valid against this keyword if an item cannot be found
more than once in the array.
The value of this keyword must be a boolean. If set to `false` the keyword
validation will be ignored.

Be aware of additional processing time needed when enabling `uniqueItems` for larger JSON arrays, especially JSON arrays of objects.
{:.alert.alert-warning data-title="Performance"}

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

{% include drafts.html v="all" %}

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

### minContains

{% include drafts.html v="2019-09, 2020-12" %}

An array is valid against this keyword if the number of valid items verified by the [`contains` keyword](#contains)
is at least the value specified by this keyword.
Value of this keyword must be a positive integer.

{% capture schema %}
```json
{
  "type": "array",
  "contains": {
    "type": "integer"
  },
  "minContains": 2
}
```

Array must contain at least two integers.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `[5, 8, 10, 5.5]`{:.language-json} | *valid*{:.text-success.text-normal} - 3 integers |
| `[5, "a", 10]`{:.language-json} | *valid*{:.text-success.text-normal} - 2 integers |
| `[1.2, 2.5, 5]`{:.language-json} | *invalid*{:.text-danger.text-normal} - only 1 integer|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### maxContains

{% include drafts.html v="2019-09, 2020-12" %}

An array is valid against this keyword if the number of valid items verified by the [`contains` keyword](#contains)
is at most the value specified by this keyword.
Value of this keyword must be a positive integer.

{% capture schema %}
```json
{
  "type": "array",
  "contains": {
    "type": "integer"
  },
  "maxContains": 2
}
```

Array must contain at most two integers.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `["a", "b"]`{:.language-json} | *valid*{:.text-success.text-normal} - no integers |
| `[5, "a", 10]`{:.language-json} | *valid*{:.text-success.text-normal} - 2 integers |
| `[5, 8, 10, "a"]`{:.language-json} | *invalid*{:.text-danger.text-normal} - 3 integer |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


### items

{% include drafts.html v="06, 07, 2019-09, *2020-12" %}

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
<br><sup>*</sup>Starting with _draft 2020-12_, this option cannot be used anymore,
  use [`prefixItems` keyword](#prefixitems) instead.

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

### prefixItems

{% include drafts.html v="2020-12" %}

An array is valid against this keyword if items are valid against the
corresponding schemas provided by the keyword value. The value of
this keyword must be an array of valid json schemas, and each item must be valid against
the schema defined at the same position (index). Items that don't have a corresponding
position (array contains 5 items and this keyword only has 3)
will be considered valid, unless the [`items` keyword](#items)
is present - which will decide the validity.


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

{% include drafts.html v="06, 07, 2019-09, *2020-12" %}

An array is valid against this keyword if all _unchecked_ items
are valid against the schema defined by the keyword value.
An item is considered _unchecked_ if [`items` keyword](#items) or [`prefixItems` keyword](#prefixitems) 
(starting with draft 2020-12) contains an array of schemas and doesn't have a corresponding position (index).
The value of the keyword must be a valid json schema (object, boolean).

This keyword is not officially defined in draft-2020-12, but we kept it for
backward compatibility reasons.<br> 
In draft-2020-12 this keyword was replaced by the [`items` keyword](#items), while the array
version of the former `items` keyword was replaced by the [`prefixItems` keyword](#prefixitems).<br>
You can disable it for draft-2020-12 by setting the [`keepAdditionalItemsKeyword` option](php-loader.html#parser-options) to `false`.
{:.alert.alert-warning data-title="Important"}

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

### unevaluatedItems

{% include drafts.html v="2019-09, 2020-12" %}

An array is valid against this keyword if every _unevaluated_ item is valid against the schema
defined by the value of this keyword.

_Unevaluated items_ are the items that were not evaluated anywhere in the current schema.
This keyword can see through adjacent keywords, such as `allOf`.

This keyword is hard to follow when you are dealing with complex schemas.
Also, it slows down the validation process because short-circuits must be disabled
for this keyword to work correctly.
We do not recommend using it, unless you fully understand its behavior!
{:.alert.alert-danger data-title="Important"}

{% capture schema %}
```json
{
  "type": "array",
  "prefixItems": [
    { "type": "string" }
  ],
  "allOf": [
    {
      "prefixItems": [
        true,
        { "type": "number" }
      ]
    }
  ],
  "unevaluatedItems": false
}
```

Schema doesn't allow unevaluated items.
{:.blockquote-footer}
{% endcapture %}

{% capture examples %}
|Input|Status|
|-----|------|
| `["foo", 42]`{:.language-json} | *valid*{:.text-success.text-normal} - all evaluated |
| `["foo", 42, true]`{:.language-json} | *invalid*{:.text-danger.text-normal} - `true` is unevaluated|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

You can disable unevaluatedItems keyword by setting the [`allowUnevaluated` option](php-loader.html#parser-options) to `false`.
{:.alert.alert-info data-title="Remember"}

