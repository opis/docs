---
layout: project
library: json-schema
version: 2.x
title: JSON Schema references ($ref)
description: using opis json schema $ref keyword to reuse schema by references 
keywords: opis, json, schema, validation, reference, $ref
---

Remember when we mentioned about the `$id` keyword in the [JSON Schema Structure](structure.html#id-keyword)?
Now is time to use that `$id` for something. As we said, a JSON Schema document
can be identified by an unique id. 

Consider that we have two JSON Schema documents:
one validates a custom email address and the other one validates an user which must
have that custom email address. In order to reuse the custom email validator
we make a reference to it by using the `$ref` keyword. Let's see how it will look.

{% capture schema %}
```json
{
  "$id": "http://example.com/custom-email-validator.json#",
  
  "type": "string",
  "format": "email",
  "pattern": "@example\\.test$"
}
```
The custom email validator.
{:.blockquote-footer}

```json
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "minLength": 2
    },
    "email": {
      "$ref": "http://example.com/custom-email-validator.json#"
    }
  },
  "required": ["name", "email"],
  "additionalProperties": false
}
```
The user validator.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"name": "Opis", "email": "opis@example.test"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"name": "Opis", "email": "opis@example.com"}`{:.language-json} | *invalid*{:.text-danger.text-normal} - `pattern` not matched|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


And what happens here is something which produces a result similar to
the following schema

```json
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "minLength": 2
    },
    "email": {
        "type": "string",
        "format": "email",
        "pattern": "@example\\.test$"
    }
  },
  "required": ["name", "email"],
  "additionalProperties": false
}
```

This is pretty cool because now you can write and link different schemas.
You can use `$ref` wherever you need, as many times as you need.

This is the first step in schema reusing.

### $ref

{% include drafts.html v="all" %}

An instance is valid against this keyword if is valid against the
schema that points to the location indicated in the value of this keyword.
The value of this keyword must be a string representing an URI, URI reference, 
[URI template](uri-template.html) or a [JSON pointer](pointers.html).

In draft-06 and draft-07 sibling keywords are ignored. Starting with draft-2019-09
sibling keywords are evaluated.

This keyword can be applied to any instance type.

To make `$ref` a powerful tool, we added a few sibling keywords
that will make your life easier:

- [`$vars` keyword](variables.html#variables-and-uri-templates) - handle URI templates
- [`$globals` keyword](variables.html#global-variables) - easy override global variables before validating the referenced schema
- [`$map` keyword](mappers.html) - map data to a new structure
- [`$inject` keyword](slots.html) - inject slots for schema composition

### $recursiveRef

{% include drafts.html v="2019-09" %}

This keyword is an attempt to create extensible schemas.
We do not recommend using it!
{:.alert.alert-danger data-title="Important"}

The value of this keyword must be a string const with value `#`.
It behaves like `$ref` but the base URI is resolved dynamically using
[`$recursiveAnchor` keyword](#recursiveanchor).

{% capture extensible_schema %}
```json
{
  "$schema": "https://json-schema.org/draft/2019-09/schema",
  "$id": "https://example.com/tree",
  "$recursiveAnchor": true,

  "type": "object",
  "properties": {
    "data": true,
    "children": {
      "type": "array",
      "items": {
        "$recursiveRef": "#"
      }
    }
  }
}
```

Tree schema, extensible
{:.blockquote-footer}
{% endcapture %}

{% capture strict_schema %}
```json
{
  "$schema": "https://json-schema.org/draft/2019-09/schema",
  "$id": "https://example.com/strict-tree",
  "$recursiveAnchor": true,

  "$ref": "tree",
  "unevaluatedProperties": false
}
```

Strict tree schema, guards against misspelled properties
{:.blockquote-footer}
{% endcapture %}

{% capture examples %}
|Input|Status|
|-----|------|
| `{"children": [ { "data": 1 } ]}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"children": [ { "daat": 1 } ]}`{:.language-json} | *invalid*{:.text-danger.text-normal} - instance with misspelled field |
{:.table}
{% endcapture %}
{% include tabs.html 1="Extensible Schema" 2="Strict Schema" 3="Examples" _1=extensible_schema _2=strict_schema _3=examples %}

### $recursiveAnchor

{% include drafts.html v="2019-09" %}

This keyword is an attempt to create extensible schemas.
We do not recommend using it!
{:.alert.alert-danger data-title="Important"}

This keyword is used to dynamically identify a base URI at runtime 
for `$recursiveRef` by marking where such a calculation can start, and where it stops.
If set to true, then when the containing schema object is used as a target of `$recursiveRef`, 
a new base URI is determined by examining the dynamic scope for 
the outermost schema that also contains `$recursiveAnchor` with a value of true. 
The base URI of that schema is then used as the dynamic base URI.
If no such schema exists, then the base URI is unchanged.
If this keyword is set to false, the base URI is unchanged.
The value of this keyword must be a boolean.


### $dynamicRef

{% include drafts.html v="2020-12" %}

This keyword is an attempt to create extensible schemas.
We do not recommend using it!
{:.alert.alert-danger data-title="Important"}

This keyword is the successor of `$recursiveRef`.
The value of this keyword must be a string representing a fragment.
It behaves like `$ref` but the base URI is resolved dynamically using
[`$dynamicAnchor` keyword](#dynamicanchor).

{% capture extensible_schema %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/tree",
  "$dynamicAnchor": "node",

  "type": "object",
  "properties": {
    "data": true,
    "children": {
      "type": "array",
      "items": {
        "$dynamicRef": "#node"
      }
    }
  }
}
```

Tree schema, extensible
{:.blockquote-footer}
{% endcapture %}

{% capture strict_schema %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/strict-tree",
  "$dynamicAnchor": "node",

  "$ref": "tree",
  "unevaluatedProperties": false
}
```

Strict tree schema, guards against misspelled properties
{:.blockquote-footer}
{% endcapture %}

{% capture examples %}
|Input|Status|
|-----|------|
| `{"children": [ { "data": 1 } ]}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"children": [ { "daat": 1 } ]}`{:.language-json} | *invalid*{:.text-danger.text-normal} - instance with misspelled field |
{:.table}
{% endcapture %}
{% include tabs.html 1="Extensible Schema" 2="Strict Schema" 3="Examples" _1=extensible_schema _2=strict_schema _3=examples %}


### $dynamicAnchor

{% include drafts.html v="2020-12" %}

This keyword is an attempt to create extensible schemas.
We do not recommend using it!
{:.alert.alert-danger data-title="Important"}

This keyword is the successor of `$recursiveAnchor`.
The value of this keyword must be a string. If the value of this keyword equals
the fragment value from the `$dynamicRef` keyword then it behaves like `$recursiveAnchor`
being set to `true`.


