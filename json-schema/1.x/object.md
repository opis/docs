---
layout: project
library: json-schema
version: 1.x
title: Object type
description: php opis json schema validation of objects
keywords: opis, php, json, schema, object, validation
---

The `object` type is used for validating key-value maps (objects).

{% capture schema %}
```json
{
  "type": "object"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{}`{:.language-json} | *valid*{:.text-success.text-normal} - object with no properties |
| `{"prop1": "val1", "prop2": 2.5}`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `12`{:.language-json} | *invalid*{:.text-danger.text-normal} - is integer/number|
| `"some text"`{:.language-json} | *invalid*{:.text-danger.text-normal} - is string|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

## Validation keywords

The following keywords are supported by the `object` type, and evaluated
in the following order: `required`, `dependencies`, `minProperties`, `maxProperties`,
`propertyNames`, `properties`, `patternProperties`, `additionalProperties`. 
All keywords are optional.

### properties

An object is valid against this keyword if every property
that is present in both the object and the value of this keyword,
 validates against the corresponding schema.
The value of this keyword must be an object, where properties must
contain valid json schemas (objects or booleans). Only the property names
that are present in both the object and the keyword value are checked.

{% capture schema %}
```json
{
  "type": "object",
  "properties": {
    "a": {
      "type": "string"
    },
    "b": {
      "type": "integer"
    }
  }
}
```

Property `a` must be a string and property `b` must be an
integer, if present.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"a": "str", "b": 5}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"a": "str"}`{:.language-json} | *valid*{:.text-success.text-normal} - `a` is a string|
| `{"b": 5, "c": null}`{:.language-json} | *valid*{:.text-success.text-normal} - `c` is an integer|
| `{"prop1": 0, "prop2": "str"}`{:.language-json} | *valid*{:.text-success.text-normal} - both `a` and `b` properties are missing|
| `{"a": 1, "b": 5}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `a` is not a string|
| `{"a": 1, "b": "text"}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `a` is not string and `b` is not an integer |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### required

An object is valid against this keyword if it contains all property names (keys)
specified by the value of this keyword. The value of this keyword must be a
non-empty array of strings representing property names.

{% capture schema %}
```json
{
  "type": "object",
  "required": ["a", "b"]
}
```

Object must have both `a` and `b` properties.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"a": 1, "b": 2, "c": 3}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"a": 1, "b": null}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"a": 1, "c": 3}`{:.language-json} | *invalid*{:.text-danger.text-normal} - property `b` is missing|
| `{"c": 1, "d": 3}`{:.language-json} | *invalid*{:.text-danger.text-normal} - both `a` and `b` properties are missing |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### dependencies

An object is valid against this keyword if it mets all dependencies
specified by this keyword value. The value of this keyword must be an object,
where property values can be:

- objects representing valid json schemas, and the whole object must match
the entire schema
- arrays of strings representing property names, then the object must
contain all property names

Only property names (from this keyword value) that are also present
in the object are checked.

{% capture schema %}
```json
{
  "type": "object",
  "dependencies": {
    "a": ["b", "c"],
    "c": {
      "type": "object",
      "properties": {
        "b": {
          "type": "integer"
        }
      }     
    }
  }
}
```

If the object has property `a`, then it must also have `b` and `c`.
If it has `c` then `b` can only be an integer.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"c": 1}`{:.language-json} | *valid*{:.text-success.text-normal} - `b` is not required |
| `{"c": 1, "b": 4}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"a": 1, "b": 4, "c": 3, "d": true}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"b": "str"}`{:.language-json} | *valid*{:.text-success.text-normal} - no dependencies |
| `{"c": 1, "b": "str"}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `b` must be an integer|
| `{"a": 1, "b": "str"}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `c` is not present|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


### minProperties

An object is valid against this keyword if the number of properties it contains
is greater then, or equal to, the value of this keyword. The value of this
keyword must be a non-negative integer. Using `0` as a value has no effect.

{% capture schema %}
```json
{
  "type": "object",
  "minProperties": 2
}
```

Object must have at least `2` properties.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"a": "a", "b": "b", "c": "c"}`{:.language-json} | *valid*{:.text-success.text-normal} - has 3 properties |
| `{"a": "a", "b": "b"}`{:.language-json} | *valid*{:.text-success.text-normal} - has 2 properties|
| `{"a": "a"}`{:.language-json} | *invalid*{:.text-danger.text-normal} - has a single property|
| `{}`{:.language-json} | *invalid*{:.text-danger.text-normal} - has no properties|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### maxProperties

An object is valid against this keyword if the number of properties it contains
is lower then, or equal to, the value of this keyword. The value of this
keyword must be a non-negative integer. Using `0` as a value means that
the object must be empty (no properties).

{% capture schema %}
```json
{
  "type": "object",
  "maxProperties": 2
}
```

Object can have at most `2` properties.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"a": "a", "b": "b"}`{:.language-json} | *valid*{:.text-success.text-normal} - has 2 properties|
| `{"a": "a"}`{:.language-json} | *valid*{:.text-success.text-normal} - has a single property|
| `{}`{:.language-json} | *valid*{:.text-success.text-normal} - has no properties|
| `{"a": "a", "b": "b", "c": "c"}`{:.language-json} | *invalid*{:.text-danger.text-normal} - has more than 2 properties|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### propertyNames

An object is valid against this keyword if every property name (key) is valid
against the value of this keyword. The value of this keyword must be a valid
json schema (an object or a boolean).

Please note that the value of `propertyNames` (the schema) will always 
test strings.
{:.alert.alert-info data-title="Important"}

{% capture schema %}
```json
{
  "type": "object",
  "propertyNames": {
    "type": "string",
    "minLength": 2
  }
}
```

Every property name must have a minimum length of `2`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"prop1": 0, "prop2": "str"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"prop": 1, "a": 2}`{:.language-json} | *invalid*{:.text-danger.text-normal} - length of `a` is smaller than `2`|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### patternProperties

An object is valid against this keyword if every property where
a property name (key) matches a regular expression from the value of this keyword,
is also valid against the corresponding schema.
The value of this keyword must an object, where 
the keys must be valid regular expressions and
the corresponding values must be valid json schemas (object or boolean).

{% capture schema %}
```json
{
  "type": "object",
  "patternProperties": {
    "^str-": {
      "type": "string"
    },
    "^int-": {
      "type": "integer"
    }
  }
}
```

Every property name that starts with `str-` must be a string and
every property name that starts with `int-` must be an integer.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"str-a": "a"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"int-i": 2}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"int-i": 2, "str-a": "a", "other": [1, 2]}`{:.language-json} | *valid*{:.text-success.text-normal} - `other` property is not matched |
| `{"other": "a"}`{:.language-json} | *valid*{:.text-success.text-normal} - no property was match |
| `{"str-a": "a", "str-b": 2}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `str-b` property is integer, not string|
| `{"str-a": "a", "int-b": 2.5}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `int-b` property is float, not integer|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### additionalProperties

An object is valid against this keyword if all _unchecked_ properties are
valid against the schema defined by the value of this keyword.
_Unchecked_ properties are the properties not checked by the `properties` and
`patternProperties` keywords (if a property name is not present in `properties` keyword 
and doesn't match any regular expression defined by `patternProperties` keyword, then
it is considered _unchecked_).
The value of this keyword must be a valid json schema (object or boolean).

To be more concise, if we have _unchecked_ properties:
- if the value of this keyword is `true`, is always valid
- if the value is `false`, is never valid
- if the value contains an object (schema), every property must be valid against that schema.

{% capture schema %}
```json
{
  "type": "object",
  "additionalProperties": {
    "type": "string"
  }
}
```

Every property value of the object must be a string.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"a": "a", "b": "str"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{}`{:.language-json} | *valid*{:.text-success.text-normal} - no properties to check|
| `{"str-a": "a", "int-b": 2}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `int-b` is integer, not string|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


{% capture schema %}
```json
{
  "type": "object",
  "properties": {
    "a": true,
    "b": true  
  },
  "additionalProperties": false
}
```

Object is invalid if contains other properties than `a` and `b`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"a": "a", "b": "str"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"a": 1}`{:.language-json} | *valid*{:.text-success.text-normal}|
| `{}`{:.language-json} | *valid*{:.text-success.text-normal} - no properties to check|
| `{"a": "a", "c": 2}`{:.language-json} | *invalid*{:.text-danger.text-normal} - property `c` is not allowed|
| `{"a": "a", "c": 2, "d": null}`{:.language-json} | *invalid*{:.text-danger.text-normal} - properties `c` and `d` are not allowed|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


{% capture schema %}
```json
{
  "type": "object",
  "patternProperties": {
    "^a": true,
    "^b": true  
  },
  "additionalProperties": false
}
```

Object is invalid if property names doesn't start with `a` or `b`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"a": "a", "b": "str"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"aAA": "a", "bBB": "str"}`{:.language-json} | *valid*{:.text-success.text-normal}|
| `{"abc": "a"}`{:.language-json} | *valid*{:.text-success.text-normal}|
| `{}`{:.language-json} | *valid*{:.text-success.text-normal} - no properties to check|
| `{"abc": "a", "extra": 2}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `extra` starts with an `e`|
| `{"abc": "a", "Bcd": 2}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `Bcd` starts with a `B` instead of `b`|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

{% capture schema %}
```json
{
  "type": "object",
  "properties": {
    "a": true,
    "b": true
  },
  "patternProperties": {
    "^extra-": {
      "type": "string"
    }
  },
  "additionalProperties": {
    "type": "integer"
  }
}
```

Object is invalid if property names, exluding `a` and `b` and
those starting with `extra-`, are not integers. Also, properties starting with
`extra-` must be strings.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"a": "a", "b": "str"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"a": 1, "extra-a": "yes"}`{:.language-json} | *valid*{:.text-success.text-normal}|
| `{"a": 1, "extra-a": "yes", "other": 1}`{:.language-json} | *valid*{:.text-success.text-normal}|
| `{}`{:.language-json} | *valid*{:.text-success.text-normal} - no properties to check|
| `{"a": "a", "extra": 3.5, "other": null}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `extra` and `other` must be integers|
| `{"Extra-x": "x"}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `Extra-x` does not start with `extra-`, so it must be an integer|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


