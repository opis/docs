---
layout: project
library: json-schema
version: 2.x
title: JSON Schema definitions
description: using opis json schema $ref keyword to reuse internal definitions 
keywords: opis, json, schema, validation, reference, definitions, $ref, $defs
---

There are cases when you want to reuse validations that are specific only to
that schema document, or you simply want to group multiple sub-schemas together. 
For example, we have a custom email validator, and
a custom username validator, and we want to apply those validators multiple
times.
This can be easily achieved using [`$ref` keyword](references.html#ref)
and the `$defs` keyword. 

In drafts 06 and 07 `$defs` keyword was named `definitions`, this has changed starting with draft 2019-09.
Don't worry, `definitions` can still be used (you can use any name, it doesn't really matter).

{% capture schema %}
```json
{
  "type": "object",
  "properties": {
    "username": {"$ref": "#/$defs/custom-username"},
    "aliases": {
      "type": "array",
      "items": {"$ref": "#/$defs/custom-username"}
    },
    "primary_email": {"$ref": "#/$defs/custom-email"},
    "other_emails": {
      "type": "array",
      "items": {"$ref": "#/$defs/custom-email"}
    }
  },
  
  "$defs": {
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
`$ref` keyword, which is something like this `#/$defs/something`.
That's an URI fragment (starts with `#`), and the rest of the string after
the `#` represents a [JSON pointer](pointers.html). JSON pointers are
covered in the [next](pointers.html) chapter, but we still explain
 the behaviour in a few words, using our example.

Consider this json pointer `/$defs/custom-email`. Because the
pointer starts with `/` (slash) we know that we begin at the root of
the schema document. Every substring delimited by a `/` slash, will
be used as property name (key) to descend. In our case we have two
substrings: `$defs` and `custom-email`. 

Descending into `$defs` gives us

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

Now, this is the value given by our JSON pointer.

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
      "$ref": "#/$defs/personal"
    }
  },
   
  "$defs": {
    "email": {
      "type": "string",
      "format": "email"    
    },
    "personal": {
      "type": "object",
      "properties": {
        "mail": {
          "$ref": "#/$defs/email"
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
      "$ref": "#/$defs/friend"
    }
  },
  
  "$defs": {
    "friend": {
      "type": "object",
      "properties": {
        "name": {
          "type": "string"
        },
        "friends": {
          "type": "array",
          "items": {
            "$ref": "#/$defs/friend"
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


