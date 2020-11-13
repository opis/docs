---
layout: project
library: json-schema
version: 1.x
title: Integer type
description: php opis json schema validation of integer numbers
keywords: opis, php, json, schema, integer, validation
---

The `integer` type is a subtype of [`number` type](number.html) used
for validating only integer numbers. 

{% capture schema %}
```json
{
  "type": "integer"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `5`{:.language-json} | *valid*{:.text-success.text-normal} - integer |
| `-10`{:.language-json} | *valid*{:.text-success.text-normal} - integer |
| `5.0`{:.language-json} | *invalid*{:.text-danger.text-normal} - is float |
| `"123"`{:.language-json} | *invalid*{:.text-danger.text-normal} - is string |
| `null`{:.language-json} | *invalid*{:.text-danger.text-normal} - is null|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

This type was added in order to simplify the writing of JSON schemas, because
in reality it is just syntactic sugar for the `number` type having the keyword
`multipleOf` set to `1`. 

```json
{
  "type": "number",
  "multipleOf": 1
}
```

## Validation keywords

The `integer` type supports [all keywords that `number` type supports](number.html#validation-keywords).
