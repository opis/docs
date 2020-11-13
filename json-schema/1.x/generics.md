---
layout: project
library: json-schema
version: 1.x
title: Generic validation keywords
description: php opis json schema generic validation keywords
keywords: opis, php, json, schema, generic, validation, type, enum, const
---

The `type`, `const`, and `enum` generic keywords, allow you to validate JSON data, 
by checking if its value, or its type, matches a given value or type. 
The keywords can be used with all JSON types and are evaluated in the order on which they are presented below. 
All these keywords are optional.


## type

The `type` keyword specifies the type of data that the schema is expecting to validate.
This keyword is not mandatory and the value of the keyword must be a string,
representing a valid [data type][data_types], or an array of strings, representing a
valid list of [data types][data_types].

{% capture schema %}
```json
{
  "type": "string"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"some text"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `""`{:.language-json} | *valid*{:.text-success.text-normal} - empty string |
| `12`{:.language-json} | *invalid*{:.text-danger.text-normal} - is integer/number |
| `null`{:.language-json} | *invalid*{:.text-danger.text-normal} - is null |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

When specifying multiple types, their order is irrelevant to the validation process, but
you should make sure that a data type is specified only once. 

{% capture schema %}
```json
{
  "type": ["object", "null"]
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"a": 1}`{:.language-json} | *valid*{:.text-success.text-normal} - is object|
| `null`{:.language-json} | *valid*{:.text-success.text-normal} - is null|
| `"1, 2, 3"`{:.language-json} | *invalid*{:.text-danger.text-normal} - is string|
| `[{"a": 1}, {"b": 2}]`{:.language-json} | *invalid*{:.text-danger.text-normal} - is array |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

{% capture schema %}
```json
{
  "type": ["number", "string", "null"]
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `-10.5`{:.language-json} | *valid*{:.text-success.text-normal} - is number|
| `"some string"`{:.language-json} | *valid*{:.text-success.text-normal} - is string|
| `null`{:.language-json} | *valid*{:.text-success.text-normal} - is null|
| `false`{:.language-json} | *invalid*{:.text-danger.text-normal} - is boolean|
| `{"a": 1}`{:.language-json} | *invalid*{:.text-danger.text-normal} - is object |
| `[1, 2, 3]`{:.language-json} | *invalid*{:.text-danger.text-normal} - is array |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

## const

An instance validates against this keyword if its value equals to the
value of this keyword. The value of this keyword can be anything.

{% capture schema %}
```json
{
  "const": "test"
}
```
Validates if equals to `"test"`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"test"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"Test"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `"tesT"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `3.4`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

{% capture schema %}
```json
{
  "const": {
    "a": 1,
    "b": "2"
  }
}
```
Validates if the object have the same properties and values (order of properties does not matter).
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"a": 1, "b": "2"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"b": "2", "a": 1}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"a": 1, "b": "2", "c": null}`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `5.10`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


## enum

An instance validates against this keyword if its value equals can be
found in the items defined by the value of this keyword. 
The value of this keyword must be an array containing anything.
An empty array is not allowed.

{% capture schema %}
```json
{
  "enum": ["a", "b", 1, null]
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"a"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"b"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `1`{:.language-json} | *valid*{:.text-success.text-normal} |
| `null`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"A"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `-1`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `false`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `["a", "b", 1, null]`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


[data_types]: ./structure.html#data-types "Data types"
