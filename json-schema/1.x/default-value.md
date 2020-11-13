---
layout: project
library: json-schema
version: 1.x
title: Default value
description: php opis json schema default value
keywords: opis, php, json, schema, validation, default
---

You can specify a default value for an item using the `default` keyword.
When a data doesn't have a corresponding value, the value of this keyword
will be used instead to do the validation checks.
This keyword is not mandatory and the value of this keyword can be anything.

The `default` keyword support is enabled by default. 
To disable it, just pass `false` to the `Opis\JsonSchema\Validator::defaultSupport` static method.
{:.alert.alert-info data-title="Remember"}

{% capture schema %}
```json
{
    "type": "object",
    "properties": {
        "prop1": {
            "type": "string",
            "default": "test"
        }
    }
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"prop1": "string"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{}`{:.language-json} | *valid*{:.text-success.text-normal} - the default value for `prop1` is used to validate the property|
| `{"prop1": 5}`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a string|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


Please pay attention when using `default`! The value of the keyword must pass
the validations!
{:.alert.alert-warning data-title="Important"}

{% capture schema %}
```json
{
  "type": "object",
  "properties": {
    "prop1": {
      "type": "string",
      "default": 5
    }
  }
}
```

This will not be valid if `prop1` is missing because 
the default value is not a string.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"prop1": "string"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{}`{:.language-json} | *invalid*{:.text-danger.text-normal} - the default value is not a string|
| `{"prop1": null}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `null` is not a string|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}
