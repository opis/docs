---
layout: project
library: json-schema
version: 2.x
title: String type
description: php opis json schema validation of string and text containing unicode characters
keywords: opis, php, json, schema, string, text, validation, pattern, regex, mime, base64
---

The `string` type is used for validating strings/texts containing
Unicode characters.

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

{% include drafts.html v="all" %}

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

{% include drafts.html v="all" %}

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

{% include drafts.html v="all" %}

A string is valid against this keyword if it matches the regular expression
specified by the value of this keyword.
Value of this keyword must be a string representing a valid regular
expression.

Please note that the delimiter used by Opis JSON Schema is `\x07` (bell)
and the modifier is `uD` ([PCRE_UTF8 & PCRE_DOLLAR_ENDONLY](http://php.net/manual/en/reference.pcre.pattern.modifiers.php){:target="_blank"}).
{:.alert.alert-info data-title="Important"}


{% capture schema %}
```json
{
  "type": "string",
  "pattern": "^opis/[a-z-]+$"
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

{% include drafts.html v="*all" %}

A string is valid against this keyword if it is encoded using the
method indicated by the value of this keyword. 
Value of this keyword must be a string.

<sup>*</sup> You should enable this keyword for your draft 
by setting the [`decodeContent` option](php-loader.html#parser-options).

Currently, there can only be three values for this keyword
- `binary` - any string is valid
- `base64` - the string must be a valid base64 encoded string
- `quoted-printable` - string must be a valid quoted printable text

If you want to add new content encodings, please read about [Content Encodings](php-content-encoding.html).

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

{% include drafts.html v="*all" %}

A string is valid against this keyword if its content has the media type
(MIME type) indicated by the value of this keyword.
If the `contentEncoding` keyword is also specified, then the decoded content
must have the indicated media type.
Value of this keyword must be a string.

<sup>*</sup> You should enable this keyword for your draft
by setting the [`decodeContent` option](php-loader.html#parser-options).

Out of the box, Opis JSON Schema knows how to detect `application/json` and for other media
types it uses [finfo class](https://www.php.net/manual/en/class.finfo.php){:target="blank"}
as a detector.

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

### contentSchema

{% include drafts.html v="*2019-09, *2020-12" %}

A string is valid against this keyword if its content has the `application/json` media type
and the decoded content validates against the schema indicated by this keyword.
If the `contentEncoding` keyword is also specified, then the decoded content
is validated against this keyword.
Value of this keyword must be a valid json schema (object or boolean).

<sup>*</sup> You should enable this keyword for your draft
by setting the [`decodeContent` option](php-loader.html#parser-options).

{% capture schema %}
```json
{
  "type": "string",
  "contentSchema": {
    "type": "object",
    "required": ["name", "age"]
  }
}
```
Valid if the string contains a valid JSON encoded string that validates against the schema when decoded.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"{\"name\": \"Opis\", \"age\": 50}"`{:.language-json} | *valid*{:.text-success.text-normal}|
| `"1-2-3"`{:.language-json} | *invalid*{:.text-danger.text-normal} - invalid json |
| `"[]"`{:.language-json} | *invalid*{:.text-danger.text-normal} - not an object |
| `"{\"name\": \"Opis\"}"`{:.language-json} | *invalid*{:.text-danger.text-normal} - `age` property is missing |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}
