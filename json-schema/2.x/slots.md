---
layout: project
library: json-schema
version: 2.x
title: Slots
description: php opis json schema composition with slots
keywords: opis, php, json, schema, slots, composition
---

The slots are a new way to create customizable JSON schemas.
Slots are _replaceable_ parts of a schema, and they are very similar to 
 named slots in HTML. A slot must contain a valid JSON schema.

## *$slots* keyword

This keyword is used to evaluate current slots, or their fallback if the slots are not provided
using the [`$inject` keyword](#inject-keyword). 
The value of this keyword must be an object, the property names represents slot names,
and the values contain fallback schema.

```json
{
 "$slots": {
  "optional": true,
  "required": false,
  "alias": "slot-name",
  "email": {
   "type": "string",
   "format": "email"
  },
  "some-ref": {
   "$ref": "other-schema.json"
  }
 }
}
```

In the above schema we have five slots, named: `optional`, `required`, `alias`, `email`, and `some-ref`.
Schemas provided by injected slots named like so are evaluated. 
If, for example, the slot `email` is not injected, the schema 
that will be evaluated is 
```json
{
 "type": "string",
 "format": "email"
}
```

If the slot `other-slot-name` is injected, his schema is not evaluated because that name is not
present in `$slots` object.

Other things to note:

- if the fallback value of a slot is `true` - the slot is optional (because boolean schema true validates any data)
- if the fallback value of a slot is `false` - the slot is required (because boolean schema false validates nothing)
- if the fallback value of a slot is a string - the slot is resolved with the name extracted from value (alias)

## *$inject* keyword

This keyword injects new slots into the current execution context,
and can only be used next to [`$ref` keyword](references.html#ref).
The value of this keyword must be an object, the property names represents slot names,
and the values must be valid JSON schemas.

```json
{
 "$ref": "other-schema.json",
 "$inject": {
  "mail": {"type": "string", "format": "email"},
  "name": true
 }
}
```

## Examples

In this example we are trying to create a customizable user schema.
Our user is very simple, contains only two properties: name and age.
The name should be a string, and the age should be an integer >= 18.
Something like this:

```json
{
  "$id": "http://example.com/user.json",

  "type": "object",
  "properties": {
    "name": {
      "type": "string"
    },
    "age": {
      "type": "integer",
      "minimum": 18
    }
  },
 
  "required": ["name", "age"]
}
```

Now we want to allow extra validations for properties. So wa add some slots


```json
{
  "$id": "http://example.com/user.json",

  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "$slots": {
        "name-prop": true
      }
    },
    "age": {
      "type": "integer",
      "$slots": {
        "age-prop": {
         "minimum": 18
        }
      }
    }
  },

  "required": ["name", "age"]
}
```

The `name` property contains one slot named `name-prop`, the fallback is `true`
which means that the slot is optional.
The `age` property contains one slot named `age-prop`, the fallback is a schema that
checks if the value is at least 18 - this way we can override the minimum value.

If there are no slots injected, the name can be any string, and the age must be an integer >= 18.

Now, let's inject some slots.

{% capture schema %}
```json
{
 "$ref": "http://example.com/user.json",

 "$inject": {
  "name-prop": {
   "minLength": 3,
   "maxLength": 20
  },
  "age-prop": {
   "minimum": 21,
   "maximum": 65
  }
 }
}
```

name must be a string with length between 3-20,
age must be between 21-65
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"name": "opis", "age": 50}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"name": "op", "age": 50}`{:.language-json} | *invalid*{:.text-danger.text-normal} - min length for name is 3|
| `{"name": "opis", "age": 18}`{:.language-json} | *invalid*{:.text-danger.text-normal} - min age is 21|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}




