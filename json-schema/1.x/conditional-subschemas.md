---
layout: project
library: json-schema
version: 1.x
title: Applying subschemas conditionally
description: php opis json schema applying subschemas conditionally
keywords: opis, php, json, schema, conditional, subschema, if then else
---

Sometimes you need to conditionally apply a subschema or to negate the validation
result. The following keywords help you do that.

## Validation keywords

The following keywords are supported by any instance type, and are evaluated in
the presented order. All keywords are optional.

### not

An instance is valid against this keyword if is **not** valid against 
 the schema defined by the value of this keyword. 
The value of this keyword must be a valid json schema (object or boolean).

{% capture schema %}
```json
{
  "not": {
    "type": "string"
  }
}
```

Accept anything but strings
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `-2.3`{:.language-json} | *valid*{:.text-success.text-normal} |
| `true`{:.language-json} | *valid*{:.text-success.text-normal} |
| `null`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"a": "test"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `[1, 2, 3]`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"some string"`{:.language-json} | *invalid*{:.text-danger.text-normal}|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

Please pay attention when using `not`!
You could unintentionally write schemas that never validate!
{:.alert.alert-warning data-title="Important"}

```json
{
  "type": "string",
  "not": {
    "type": "string"
  }
}
```
This schema will never validate
{:.blockquote-footer}

### if-then-else

This is a conditional structure containing three keywords: `if`, `then` and `else`.
Every keyword value must be a valid json schema (object or boolean).
If the `if` keyword is not present the `then` and `else` keywords are
ignored, but when the `if` keyword is present at least `then` or `else`
should also be present (both can be at the same time).
The instance is valid against this keyword in one of the following cases:
- the `if` keyword validates the instance and the `then` keyword also validates it
- the `if` keyword doesn't validate the instance but the `else` keyword validates it.

As a best practice, please place these keywords in the same order as defined here and do not
add other keywords between them.
{:.alert.alert-info data-title="Remember"}

{% capture schema %}
```json
{
  "if": {
    "type": "string"
  },
  "then": {
    "minLength": 3
  },
  "else": {
    "const": 0
  }
}
```

If the instance is a `string` then must have a minimum length of `3`, else
it must be `0`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"abc"`{:.language-json} | *valid*{:.text-success.text-normal} - string of length 3 |
| `"abcd"`{:.language-json} | *valid*{:.text-success.text-normal} - string of length 4 |
| `0`{:.language-json} | *valid*{:.text-success.text-normal} |
| `0.0`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"ab"`{:.language-json} | *valid*{:.text-success.text-normal} - string of length 2 |
| `1`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a string and not `0`|
| `["abc"]`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a string and not `0`|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

{% capture schema %}
```json
{
  "if": {
    "type": "string"
  },
  "then": {
    "minLength": 3
  }
}
```

If the instance is a `string` then must have a minimum length of `3`, else
it is invalid.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"abc"`{:.language-json} | *valid*{:.text-success.text-normal} - string of length 3 |
| `"abcd"`{:.language-json} | *valid*{:.text-success.text-normal} - string of length 4 |
| `"ab"`{:.language-json} | *invalid*{:.text-danger.text-normal} - string of length 2 |
| `0`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a string |
| `["abc"]`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a string |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


{% capture schema %}
```json
{
  "if": {
    "type": "string"
  },
  "else": {
    "const": 0
  }
}
```

If the instance is a `string` consider it valid, else
it is valid only when `0`.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"abc"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `""`{:.language-json} | *valid*{:.text-success.text-normal} |
| `0`{:.language-json} | *invalid*{:.text-valid.text-normal} |
| `-0.0`{:.language-json} | *invalid*{:.text-valid.text-normal} |
| `1`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a string and not `0`|
| `["abc"]`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a string and not `0`|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

