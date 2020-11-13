---
layout: project
library: json-schema
version: 1.x
title: Opis JSON Schema URI Templates
description: about uri templates using opis json schema validation
keywords: opis, json, schema, validation, uri, template, variable
---

URI templates help us dynamically reuse schemas based on the values of current data being validated.

For simplicity, we can say that an URI template contains placeholders that will be replaced by
values to get an URI reference that will be used further. Here is an example:

```json
{
    "$ref": "http://example.com/{folder}/{file}.json",
    "$vars": {
        "folder": "user",
        "file": "schema"
    }
}
```

Above, we have two placeholders `{folder}` and `{file}`, which will be replaced with
the corresponding values from the `$vars` keyword (`{folder}` => `"user"`, `{file}` => `"schema"`),
resulting the final URI `http://example.com/user/schema.json`.

More details about the structure of uri templates can be found [here](https://tools.ietf.org/html/rfc6570){:target="_blank"}.

## Expansion examples

{% capture data_1 %}
```json
{
  "$ref": "{/var1,var2}",
  "$vars": {
    "var1": "a",
    "var2": "b"
  }
}
```
Resulted `$ref` is `/a/b`
{:.blockquote-footer}
{% endcapture %}

{% capture data_2 %}
```json
{
  "$ref": "{?var1,var2}",
  "$vars": {
    "var1": "a",
    "var2": "b"
  }
}
```

Resulted `$ref` is `?var1=a&var2=b`
{:.blockquote-footer}

```json
{
  "$ref": "?x=1&y=2{&var1,var2}",
  "$vars": {
    "var1": "a",
    "var2": "b"
  }
}
```

Resulted `$ref` is `?x=1&y=2&var1=a&var2=b`
{:.blockquote-footer}
{% endcapture %}
{% capture data_3 %}
```json
{
  "$ref": "{#var1}",
  "$vars": {
    "var1": "test"
  }
}
```

Resulted `$ref` is `#test`
{:.blockquote-footer}
{% endcapture %}
{% include tabs.html 1="Path" 2="Query string" 3="Fragment" _1=data_1 _2=data_2 _3=data_3 %}


## Modifier examples

{% capture data_1 %}
```json
{
  "$ref": "{var:3}",
  "$vars": {
    "var": "test"
  }
}
```
Resulted `$ref` is `tes`
{:.blockquote-footer}
{% endcapture %}
{% capture data_2 %}
```json
{
  "$ref": "{?address*}",
  "$vars": {
    "address": {
      "city": "Some city",
      "street": "Some street",
      "number": 5
    }
  }
}
```

Resulted `$ref` is `?city=Some%20city&street=Some%20street&number=5`
{:.blockquote-footer}
{% endcapture %}
{% include tabs.html 1="Substrings" 2="Compositions" _1=data_1 _2=data_2 %}
