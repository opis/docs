---
layout: project
library: json-schema
version: 2.x
title: Default value
description: php opis json schema default value
keywords: opis, php, json, schema, validation, default
---

You can specify a default value for an item using the `default` keyword.
When a data doesn't have a corresponding value, the value of this keyword
will be used instead to do the validation checks.
This keyword is not mandatory, and the value of this keyword can be anything.

This keyword can alter the input data, defining the missing properties
on objects, as shown in [this PHP example](#php-example).
You can disable it by setting the [`allowDefaults` option](php-loader.html#parser-options) to `false`.
{:.alert.alert-danger data-title="Important"}

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
| `{}`{:.language-json} | *valid*{:.text-success.text-normal} - the default value for `prop1` is not required |
| `{"prop1": 5}`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a string|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

## PHP Example

Consider the following schema

```json
{
  "$id": "http://example.com/schema.json",
  
  "type": "object",
  "properties": {
    "name": {
      "type": "string"
    },
    "age": {
      "$comment": "value is null <=> age not disclosed",
      "type": ["integer", "null"],
      "default": null
    }
  },
  "required": ["name"]
}
```

As you can see, the name is required, but the age isn't. If not present, the age
should be considered not disclosed (value is `null`).

Our PHP code is something like this:

```php
$data = (object)['name' => 'Opis', 'age' => 18];

$result = $validator->validate($data, "http://example.com/schema.json");
if ($result->isValid()) {
    inset_into_db($data->name, $data->age);
}
```

What happens when age property is missing?

```php
$data = (object)['name' => 'Opis'];
```

The age property is automatically added to the object with the default value.
In this way you avoid the double check, so you don't have to do:

```php
$data->age ?? null
```

You might think that this isn't something that saved precious time.
Well... what if we had another property, let's say address, that
contains an object?

Instead of editing the same object in two or more files, you edit just the value
of the `default` keyword.

```json
{
  "type": "object",
  "properties": {
    "address": {
      "type": "object",
      "properties": {
        "country": {},
        "county": {},
        "city": {},
        "street": {},
        "street2": {}
      },
      "default": {
        "country": null,
        "county": null,
        "city": null,
        "street": "",
        "street2": ""
      }
    }
  }
}
```

Now in php you have to do something like this

```php
$data->address ?? (object) [
    'country' => null,
    'county' => null,
    'city' => null,
    'street' => '',
    'street2' => '',
];
```

The fun begins when you have to add the zip property to the address: add it into the schema document,
then find where you have to change in PHP files just to add the same object. 
We hate that, that's way we set the object's missing properties to default values (if `default` keyword is present).

If you don't want this behaviour just disable it by setting the [`allowDefaults` option](php-loader.html#parser-options) to `false`.
