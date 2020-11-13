---
layout: project
library: json-schema
version: 1.x
title: Formats
description: php opis json schema formats
keywords: opis, php, json, schema, formats, date, time, email
---

The `format` keyword performs a semantic validation on data.
The value of this keyword must be a string, representig a format.
The keyword behavior depends on the data type, meaning that
the same format name for a `string` behaves differently on a `number`,
or is missing, because not all data types must implement a format and
usually different data types have different formats.

{% capture schema %}
```json
{
    "format": "date"
}
```

`date` format is available only for strings.
{:.blockquote-footer}
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"1970-01-01"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `510`{:.language-json} | *valid*{:.text-success.text-normal} - not a string|
| `"test"`{:.language-json} | *invalid*{:.text-danger.text-normal} - not a date|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

## Provided formats

Opis JSON Schema provides all the formats for `string` type defined in json schema specifications.

Please note that formats starting with `idn-` or `iri` require [PHP intl extension](http://php.net/manual/en/book.intl.php){:target="_blank"} 
in order to work correctly.
{:.alert.alert-info data-title="Important"}

### date

A string is valid against this format if it represents a date in
the following format: `YYYY-MM-DD`.

{% capture schema %}
```json
{
    "type": "string",
    "format": "date"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"1970-01-01"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"Jan. 1st, 1970"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### time

A string is valid against this format if it represents a time in
the following format: `hh:mm:ss.sTZD`.

{% capture schema %}
```json
{
    "type": "string",
    "format": "time"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"10:05:08"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"10:05:08.5"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"10:05:08-02:30"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"10:05:08Z"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"45:60:62"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `"10:05"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `"1 pm"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


### date-time

A string is valid against this format if it represents a date-time in
the following format: `YYYY:MM::DDThh:mm:ss.sTZD`.


{% capture schema %}
```json
{
    "type": "string",
    "format": "date-time"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"1970-01-01T10:05:08"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"1970-01-01T10:05:08.10"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"1970-01-01T10:05:08+01:00"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"Jan. 1st, 1970 at 1 pm"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### regex

A string is valid against this format if it represents a
valid regular expression.

{% capture schema %}
```json
{
    "type": "string",
    "format": "regex"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"^[a-z]+$"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"a/b"`{:.language-json} | *invalid*{:.text-danger.text-normal} - slash is not escaped|
| `"(a"`{:.language-json} | *invalid*{:.text-danger.text-normal} - incomplete group|
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


### email

A string is valid against this format if it represents a
valid e-mail address format.

{% capture schema %}
```json
{
    "type": "string",
    "format": "email"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"john@example.com"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"john(at)example.com"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### idn-email

A string is valid against this format if it represents a
valid idn e-mail address format.

{% capture schema %}
```json
{
    "type": "string",
    "format": "idn-email"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"실례@실례.테스트"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"john@example.com"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"1234"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


### hostname

A string is valid against this format if it represents a valid
hostname.

{% capture schema %}
```json
{
    "type": "string",
    "format": "hostname"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"www.example.com"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"xn--4gbwdl.xn--wgbh1c"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"not_a_valid_host_name"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### idn-hostname

A string is valid against this format if it represents a valid
IDN hostname.

{% capture schema %}
```json
{
    "type": "string",
    "format": "idn-hostname"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"실례.테스트"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"〮실례.테스트"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### ipv4

A string is valid against this format if it represents a valid
IPv4 address.

{% capture schema %}
```json
{
    "type": "string",
    "format": "ipv4"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"192.168.0.1"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"192.168.1.1.1"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### ipv6

A string is valid against this format if it represents a valid
IPv6 address.

{% capture schema %}
```json
{
    "type": "string",
    "format": "ipv6"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"::1"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"12345::"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### json-pointer

A string is valid against this format if it represents a valid
(absolute) json pointer.


{% capture schema %}
```json
{
    "type": "string",
    "format": "json-pointer"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"/a/b/c"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"/a/~"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### relative-json-pointer

A string is valid against this format if it represents a valid
relative json pointer.

{% capture schema %}
```json
{
    "type": "string",
    "format": "relative-json-pointer"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"0/a/b"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"5/a/b#"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"2#"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"/a/b"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### uri

A string is valid against this format if it represents a valid
uri.

{% capture schema %}
```json
{
    "type": "string",
    "format": "uri"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"http://example.com/path?qs=v&qs2[1]=3#fragment"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"http://a_example.com"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
| `"aaa/bbb.html"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### uri-reference

A string is valid against this format if it represents a valid
uri or uri reference.

{% capture schema %}
```json
{
    "type": "string",
    "format": "uri-reference"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"aaa/bbb.html"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"?a=b"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"#fragment"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"http://example.com"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"http://a_example.com"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### uri-template

A string is valid against this format if it represents a valid
uri template or uri-reference.

{% capture schema %}
```json
{
    "type": "string",
    "format": "uri-template"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"/{+file}.html"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"http://example.com/dictionary/{term:1}/{term}"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"{?q,lang}"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"http://a_example.com/file.php{?q,r}"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}

### iri

A string is valid against this format if it represents a valid IRI.

{% capture schema %}
```json
{
    "type": "string",
    "format": "iri"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"http://ƒøø.ßår/?∂éœ=πîx#πîüx"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"http://ƒøø.com/blah_(wîkïpédiå)_blah#ßité-1"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"http:// ƒøø.com"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}


### iri-reference

A string is valid against this format if it represents a valid IRI reference.

{% capture schema %}
```json
{
    "type": "string",
    "format": "iri-reference"
}
```
{% endcapture %}
{% capture examples %}
|Input|Status|
|-----|------|
| `"//ƒøø.ßår/?∂éœ=πîx#πîüx"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"#ƒrägmênt"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"http://ƒøø.com/blah_(wîkïpédiå)_blah#ßité-1"`{:.language-json} | *valid*{:.text-success.text-normal} |
| `"\\\\WINDOWS\\filëßåré"`{:.language-json} | *invalid*{:.text-danger.text-normal} |
{:.table}
{% endcapture %}
{% include tabs.html 1="Schema" 2="Examples" _1=schema _2=examples %}
