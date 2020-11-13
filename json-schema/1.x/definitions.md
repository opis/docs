---
layout: project
library: json-schema
version: 1.x
title: JSON Schema definitions
description: using opis json schema $ref keyword to reuse internal definitions 
keywords: opis, json, schema, validation, reference, definitions, $ref
---

There are cases when you want to reuse validations that are specific only to
that schema document. For example, we have a custom email validator, and
a custom username validator, and we want to apply those validators multiple
times. Outside the schema document these validators aren't useful. 
This can be easily achieved using [`$ref` keyword](ref-keyword.html)
and the [`definitions` keyword](#definitions).

{% capture schema %}
```json
{
  "type": "object",
  "properties": {
    "username": {"$ref": "#/definitions/custom-username"},
    "aliases": {
      "type": "array",
      "items": {"$ref": "#/definitions/custom-username"}
    },
    "primary_email": {"$ref": "#/definitions/custom-email"},
    "other_emails": {
      "type": "array",
      "items": {"$ref": "#/definitions/custom-email"}
    }
  },
  
  "definitions": {
    "custom-username": {
      "type": "string",
      "minLength":3
    },
    "custom-email": {
      "type": "string",
      "format": "email",
      "pattern": "\\.com$"
    }
  }
}
```  
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"username": "opis", "primary_email": "opis@example.com"}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"aliases": ["opis json schema", "opis the lib"]}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"other_emails": ["opis@example.com", "opis.lib@example.com"]}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"username": "ab", "primary_email": "opis@example.test"}`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `{"aliases": ["opis", "ab"]}`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `{"other_email": ["opis@example.test"]}`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


Ok, let's see what happens there. The confusing thing is the value of the
`$ref` keyword, which is something like this `#/definitions/something`.
That's an URI fragment (starts with `#`), and the rest of the string after
the `#` represents a [JSON pointer](pointers.html). JSON pointers are
covered in the [next](pointers.html) chapter, but we still explain
 the behaviour in a few words, using our example.

Consider this json pointer `/definitions/custom-email`. Because the
pointer starts with `/` (slash) we now that we begin at the root of
the schema document. Every substring delimited by a `/` slash, will
be used as property name (key) to descend. In our case we have two
substrings: `definitions` and `custom-email`. 

Descending into `definitions` gives us

```json
{
  "custom-username": {
    "type": "string",
    "minLength":3
  },
  "custom-email": {
    "type": "string",
    "format": "email",
    "pattern": "\\.com$"
  }
}
```

And from here, descending into `custom-email` gives us

```json
{
  "type": "string",
  "format": "email",
  "pattern": "\\.com$"
}
```

Now, this is the value given by our json pointer.

## Examples

### Definition referencing other definition

{% capture schema %}
```json
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string"
    },
    "personal_data": {
      "$ref": "#/definitions/personal"
    }
  },
   
  "definitions": {
    "email": {
      "type": "string",
      "format": "email"    
    },
    "personal": {
      "type": "object",
      "properties": {
        "mail": {
          "$ref": "#/definitions/email"
        }
      }
    }
  }
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `{"name": "John", "personal_data": {"mail": "john@example.com"}}`{:.language-json} | *valid*{:.text-success.text-normal} |
| `{"name": "John", "personal_data": {"mail": "invalid-email"}}`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `{"name": "John", "personal_data": "john@example.com"}`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### Recursive validation

{% capture schema %}
```json
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string"
    },
    "best_friend": {
      "$ref": "#/definitions/friend"
    }
  },
  
  "definitions": {
    "friend": {
      "type": "object",
      "properties": {
        "name": {
          "type": "string"
        },
        "friends": {
          "type": "array",
          "items": {
            "$ref": "#/definitions/friend"
          }
        }
      }
    }
  }
}
```
{% endcapture %}
{% capture valid_input %}
```json
{
  "name": "John",
  "best_friend": {
    "name": "The dog"
  }
}
```
{: style="margin-bottom: 16px"}
```json
{
  "name": "John",
  "best_friend": {
    "name": "The dog",
    "friends": [
      {
        "name": "The neighbor's dog",
        "friends": [
          {
            "name": "Underdog"
          },
          {
            "name": "Scooby-Doo"
          }
        ]
      }
    ]
  }
}
```
{% endcapture %}
{% capture invalid_input %}
```json
{
  "name": "John",
  "best_friend": "The dog"
}
```
{: style="margin-bottom: 16px"}

```json
{
  "name": "John",
  "best_friend": {
    "name": "The dog",
    "friends": ["Underdog", "Scooby-Doo"]
  }
}
```
{% endcapture %}
{% include tabs.html 1="Schema" 2="Valid input" 3="Invalid input" _1=schema _2=valid_input _3=invalid_input %}


