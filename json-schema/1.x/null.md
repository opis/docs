---
layout: project
library: json-schema
version: 1.x
title: Null type
description: php opis json schema validation of null type
keywords: opis, php, json, schema, null, validation
---

The `null` type is used to validate the `null` value.

{% capture schema %}
```json
{
  "type": "null"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `null`{:.language-json} | *valid*{:.text-success.text-normal} |
| `""`{:.language-json} | *invalid*{:.text-danger.text-normal} - is string |
| `false`{:.language-json} | *invalid*{:.text-danger.text-normal} - is boolean|
| `0`{:.language-json} | *invalid*{:.text-danger.text-normal} - is integer/number|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

## Validation keywords

The `null` type has no specific validation keywords.
