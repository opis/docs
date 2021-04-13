---
layout: project
library: json-schema
version: 2.x
title: $data keyword
description: php opis json schema $data keyword support
keywords: opis, php, json, schema, $data
---

This keyword uses values from data as schema values. Since it can be dangerous to
allow arbitrary content inside a schema this keyword is restricted to the following keywords:
 
[minLength](string.html#minlength), [maxLength](string.html#maxlength), [pattern](string.html#pattern),  

[minimum](number.html#minimum), [maximum](number.html#maximum), [multipleOf](number.html#multipleof), 
[exclusiveMinimum](number.html#exclusiveminimum), [exclusiveMaximum](number.html#exclusivemaximum),

[minItems](array.html#minitems), [maxItems](array.html#maxitems), [uniqueItems](array.html#uniqueitems),

[required](object.html#required), [minProperties](object.html#minproperties), [maxProperties](object.html#maxproperties),

[format](formats.html), [const](generics.html#const), [enum](generics.html#enum).


To use this keyword you must replace the value of the original keyword 
with an object having the following structure `{"$data": "json-pointer"}`.
The value of the keyword will dynamically resolve to the value extracted by the [JSON pointer](pointers.html).

{% capture schema %}
```json
{
  "type": "object",
  "properties": {
    "password": {
      "type": "string"
    },
    "repeat": {
      "type": "string",
      "const": {"$data": "/password"}
    }
  }
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"password": "a", "repeat": "a"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"password": "a", "repeat": "b"}`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

You can disable $data keyword by setting the [`allowDataKeyword` option](php-loader.html#parser-options) to `false`.
{:.alert.alert-info data-title="Remember"}

