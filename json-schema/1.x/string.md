---
layout: project
library: json-schema
version: 1.x
title: String type
description: php opis json schema validation of string and text containing unicode characters
keywords: opis, php, json, schema, string, text, validation, pattern, regex, mime, base64
---

The `string` type is used for validating strings/texts containing
Unicode characters.

Please note that in order to calculate the length of a string,
Opis JSON Schema uses the following libraries/functions, 
depending which one is available on your system: 
[Opis String](https://github.com/opis/string){:target="_blank"} 
(recommended, add it with `composer require opis/string`),
[mb_strlen](http://php.net/manual/en/function.mb-strlen.php){:target="_blank"}
(you must enable [mb_string](http://php.net/manual/en/book.mbstring.php){:target="_blank"} extension for PHP),
[strlen](http://php.net/manual/en/function.strlen.php){:target="_blank"} 
(will not always return the correct length of a string with Unicode characters).
{:.alert.alert-info data-title="Important"}

{% capture schema %}
```json
{
  "type": "string"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"some text"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `""`{:.language-json} | *valid*{:.text-success.text-normal} - empty string |
| `12`{:.language-json} | *invalid*{:.text-danger.text-normal} - is integer/number |
| `null`{:.language-json} | *invalid*{:.text-danger.text-normal} - is null|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

## Validation keywords

The following keywords are supported by the `string` type, and evaluated
in the presented order. All keywords are optional.

### minLength

A string is valid against this keyword if its length is greater then, 
or equal to, the value of this keyword. 
Value of this keyword must be a non-negative integer.

{% capture schema %}
```json
{
  "type": "string",
  "minLength": 3
}
```

Valid if contains at least `3` characters.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"abc"`{:.language-json} | *valid*{:.text-success.text-normal} - length = 3|
| `"abcd"`{:.language-json} | *valid*{:.text-success.text-normal} - length > 3 |
| `"ab"`{:.language-json} | *invalid*{:.text-danger.text-normal} - length < 3 |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### maxLength

A string is valid against this keyword if its length is lower then, 
or equal to, the value of this keyword. 
Value of this keyword must be a non-negative integer.

{% capture schema %}
```json
{
  "type": "string",
  "maxLength": 3
}
```

Valid if contains at most `3` characters.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"ab"`{:.language-json} | *valid*{:.text-success.text-normal} - length < 3 |
| `""`{:.language-json} | *valid*{:.text-success.text-normal} - length < 3 |
| `"abc"`{:.language-json} | *valid*{:.text-success.text-normal} - length = 3 |
| `"abcd"`{:.language-json} | *invalid*{:.text-danger.text-normal} - length > 3|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### pattern

A string is valid against this keyword if it matches the regular expression
specified by the value of this keyword.
Value of this keyword must be a string representing a valid regular
expression.

Please note that the delimiter used by Opis JSON Schema is `\x07` (bell)
and the modifier is `u` ([PCRE_UTF8](http://php.net/manual/en/reference.pcre.pattern.modifiers.php){:target="_blank"}).
{:.alert.alert-info data-title="Important"}


{% capture schema %}
```json
{
  "type": "string",
  "pattern": "^opis\\/[a-z-]+$"
}
```

Valid if starts with `opis/` and is followed by either `-` (minus sign) or a lower case letter
between `a` and `z`. The rest of the string can be any character.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"opis/json-schema"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"opis/--"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"opis"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `"opis/Json-Schema"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

For more information about PHP regular expressions, you can read about
- [Pattern Syntax](http://php.net/manual/en/reference.pcre.pattern.syntax.php){:target="_blank"}
- [preg_match function](http://php.net/manual/en/function.preg-match.php){:target="_blank"}

### contentEncoding

A string is valid against this keyword if it is encoded using the
method indicated by the value of this keyword. 
Value of this keyword must be a string.

Currently, there can only be two values for this keyword
- `binary` - any string is valid
- `base64` - the string must be a valid base64 encoded string

{% capture schema %}
```json
{
  "type": "string",
  "contentEncoding": "base64"
}
```
Valid if contains only characters inside the base64 alphabet.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"b3Bpcy9qc29uLXNjaGVtYQ=="`{:.language-json} | *valid*{:.text-success.text-normal} - decodes to `"opis/json-schema"` |
| `"opis/json-schema"`{:.language-json} | *invalid*{:.text-danger.text-normal} - the `-` character is not in the `base64` alphabet |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


### contentMediaType

A string is valid against this keyword if its content has the media type
(MIME type) indicated by the value of this keyword.
If the `contentEncoding` keyword is also specified, then the decoded content
must have the indicated media type.
Value of this keyword must be a string.

Out of the box, Opis JSON Schema comes with the following media types
- `text/plain` - any text
- `application/json` - json encoded string

If you want to add new media types (MIME types), please read about [Media Types](php-media-type.html).

{% capture schema %}
```json
{
  "type": "string",
  "contentMediaType": "application/json"
}
```
Valid if the string contains valid JSON syntax.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"{\"a\": 1}"`{:.language-json} | *valid*{:.text-success.text-normal} - JSON object|
| `"[\"a\", \"b\", 2]"`{:.language-json} | *valid*{:.text-success.text-normal} - JSON array|
| `"\"text\""`{:.language-json} | *valid*{:.text-success.text-normal} - JSON string|
| `"null"`{:.language-json} | *valid*{:.text-success.text-normal} - JSON null|
| `"1-2-3"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `"{a: 1}"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `"a = 23"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


{% capture schema %}
```json
{
  "type": "string",
  "contentEncoding": "base64",
  "contentMediaType": "application/json"
}
```
Valid if contains only characters inside base64 alphabet, and the base64 decoded
content contains valid JSON syntax.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"eyJhIjogMX0="`{:.language-json} | *valid*{:.text-success.text-normal} - decodes to `"{\"a\": 1}"` which is a JSON object |
| `"bnVsbA=="`{:.language-json} | *valid*{:.text-success.text-normal} - decodes to `"null"` which is JSON null|
| `"1-2-3"`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a base64 encoded string|
| `"e2E6IDF9"`{:.language-json} | *invalid*{:.text-danger.text-normal} - decodes to `"{a: 1}"` which is not a JSON object|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

