---
layout: project
library: json-schema
version: 1.x
title: Number type
description: php opis json schema validation of numbers, integer and float
keywords: opis, php, json, schema, number, validation, integer, float
---

The `number` type is used for validating integer and float values.

{% capture schema %}
```json
{
  "type": "number"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `5`{:.language-json} | *valid*{:.text-success.text-normal} - integer |
| `-10.8`{:.language-json} | *valid*{:.text-success.text-normal} - float |
| `"123"`{:.language-json} | *invalid*{:.text-danger.text-normal} - is string |
| `null`{:.language-json} | *invalid*{:.text-danger.text-normal} - is null|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

## Validation keywords

The following keywords are supported by the `number` type, and evaluated
in the presented order. All keywords are optional.

### minimum

A number is valid against this keyword if is greater than, or equal to, the
value of this keyword. 
Value of this keyword must be a number (integer or float).

{% capture schema %}
```json
{
  "type": "number",
  "minimum": 10.5
}
```
Valid if the number is at least `10.5`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `11`{:.language-json} | *valid*{:.text-success.text-normal} - is greater |
| `10.5`{:.language-json} | *valid*{:.text-success.text-normal} - is equal |
| `10`{:.language-json} | *invalid*{:.text-danger.text-normal} - is lower |
| `10.49`{:.language-json} | *invalid*{:.text-danger.text-normal} - is lower|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### exclusiveMinimum

A number is valid against this keyword if is strictly greater than the
value of this keyword. Value of this keyword must be a number (integer or float)
or a boolean. If this keyword holds a boolean, 
then the `minimum` keyword is required and is used as reference for comparison.

{% capture schema %}
```json
{
  "type": "number",
  "exclusiveMinimum": 10.5
}
```
Valid if the number is greater than `10.5`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `11`{:.language-json} | *valid*{:.text-success.text-normal} - is greater |
| `10.6`{:.language-json} | *valid*{:.text-success.text-normal} - is greater |
| `10.5`{:.language-json} | *invalid*{:.text-danger.text-normal} - is equal |
| `10`{:.language-json} | *invalid*{:.text-danger.text-normal} - is lower|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


{% capture schema %}
```json
{
  "type": "number",
  "minimum": 10.5,
  "exclusiveMinimum": true
}
```
Valid if the number is greater than `10.5`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `11`{:.language-json} | *valid*{:.text-success.text-normal} - is greater |
| `10.6`{:.language-json} | *valid*{:.text-success.text-normal} - is greater |
| `10.5`{:.language-json} | *invalid*{:.text-danger.text-normal} - is equal |
| `10`{:.language-json} | *invalid*{:.text-danger.text-normal} - is lower|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### maximum

A number is valid against this keyword if is lower than, or equal to, the
value of this keyword. 
Value of this keyword must be a number (integer or float).

{% capture schema %}
```json
{
  "type": "number",
  "maximum": 10.5
}
```
Valid if the number is at most `10.5`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `10`{:.language-json} | *valid*{:.text-success.text-normal} - is lower |
| `10.5`{:.language-json} | *valid*{:.text-success.text-normal} - is equal |
| `10.6`{:.language-json} | *invalid*{:.text-danger.text-normal} - is greater |
| `11`{:.language-json} | *invalid*{:.text-danger.text-normal} - is greater|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### exclusiveMaximum

A number is valid against this keyword if is strictly lower than the
value of this keyword. Value of this keyword must be a number (integer or float)
or a boolean. If this keyword holds a boolean, 
then the `maximum` keyword is required and is used as reference for comparison.

{% capture schema %}
```json
{
  "type": "number",
  "exclusiveMaximum": 10.5
}
```
Valid if the number is lower than `10.5`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `10`{:.language-json} | *valid*{:.text-success.text-normal} - is lower |
| `10.49`{:.language-json} | *valid*{:.text-success.text-normal} - is lower |
| `10.5`{:.language-json} | *invalid*{:.text-danger.text-normal} - is equal |
| `11`{:.language-json} | *invalid*{:.text-danger.text-normal} - is greater|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


{% capture schema %}
```json
{
  "type": "number",
  "maximum": 10.5,
  "exclusiveMaximum": true
}
```
Valid if the number is lower than `10.5`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `10`{:.language-json} | *valid*{:.text-success.text-normal} - is lower |
| `10.49`{:.language-json} | *valid*{:.text-success.text-normal} - is lower |
| `10.5`{:.language-json} | *invalid*{:.text-danger.text-normal} - is equal |
| `11`{:.language-json} | *invalid*{:.text-danger.text-normal} - is greater|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### multipleOf

A number is valid against this keyword if the division between the
number and the the value of this keyword results in an integer.
Value of this keyword must be a strictly positive number (zero is not allowed).

{% capture schema %}
```json
{
  "type": "number",
  "multipleOf": 0.5
}
```
Valid only if the result of the division between the given number and `0.5` is an integer
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `10`{:.language-json} | *valid*{:.text-success.text-normal} - `10 / 0.5 = 20` |
| `1.5`{:.language-json} | *valid*{:.text-success.text-normal} - `1.5 / 0.5 = 3` |
| `-2`{:.language-json} | *valid*{:.text-success.text-normal} - `-2 / 0.5 = -4` |
| `10.2`{:.language-json} | *invalid*{:.text-danger.text-normal} - `10.2 / 0.5 = 20.40` |
| `-3.6`{:.language-json} | *invalid*{:.text-danger.text-normal} - `-3.6 / 0.5 = -7.2` |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

