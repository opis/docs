---
layout: project
library: json-schema
version: 2.x
title: Pragma options
description: php opis json schema $pragma keyword
keywords: opis, php, json, schema, pragma, max-errors, cast, globals
---

The `$pragma` keyword is used to override settings for the current execution context.
The value of the `$pragma` keyword must be an object. Available properties are described below.

You can disable $pragma keyword by setting the [`allowPragmas` option](php-loader.html#parser-options) to `false`.
{:.alert.alert-info data-title="Remember"}

## globals

Sets the current [global variables](variables.html#global-variables).

```json
{
  "$pragma": {
    "globals": {
      "VALUE-1": 1,
      "OTHER-GLOBAL": "something"
    }
  }
}
```

## slots

Sets the current [slots](slots.html). 
If a slot value is passed as a string, a slot with that name is search and used if exists.

```json
{
  "$pragma": {
    "slots": {
      "part-mail": {"format": "email"},
      "part-props": "some-existing-slot-name"
    }
  }
}
```

## maxErrors

Overrides the [`maxErrors` setting](php-validator.html#max-errors). 
The value must be a positive integer.

## cast

Converts current value by applying a cast. The value must be 
a string representing a [json data type name](structure.html#data-types) (except `null`).

```json
{
  "$pragma": {
    "cast": "number"
  },
  "type": "number",
  "minimum": 5
}
```

If you send `"5.1"` (string), the value will be converted to `5.1` (float) and then validated. 
Without the cast, an error is returned by the `type` keyword.

The following table describes various casts.

If a conversion is not available, the value is converted to `null`.

| From   |      To      |  Result |
|----------|:-------------:|------:|
| null | number | `0` - zero |
| null | integer |   `0` - zero |
| null | boolean |   `false` |
| null | string |    `""` - empty string |
| null | array |    `[]` - empty array |
| null | object |    `null` |
| number | number |    same value |
| number | integer |    truncates number, `5.1` => `5` |
| number | boolean |    `false` if `0`, `true` otherwise |
| number | string |    string representation, `5.1` => `"5.1"` |
| number | array |    `5.1` => `[5.1]` |
| number | object |    `null` |
| integer | number |    same value |
| integer | integer |    same value |
| integer | boolean |    `false` if `0`, `true` otherwise |
| integer | string |    string representation, `5` => `"5"` |
| integer | array |     `5` => `[5]` |
| integer | object |    `null` |
| boolean | number |    `true` => `1`, `false` => `0` |
| boolean | integer |    `true` => `1`, `false` => `0` |
| boolean | boolean |    same value |
| boolean | string |    `true` => `"1"`, `false` => `""` - empty string |
| boolean | array |    `true` => `[true]`, `false` => `false` |
| boolean | object |    `null` |
| string | number |    try to parse the number from string, `0` if invalid number representation |
| string | integer |   try to parse the integer from string, `0` if invalid number representation |
| string | boolean |   empty string => `false`, otherwise `true` |
| string | string |    same value |
| string | array |    `"str"` => `["str"]` |
| string | object |    `null` |
| array | number |    `null` |
| array | integer |   `null` |
| array | boolean |   empty array => `false`, otherwise `true` |
| array | string |    `null` |
| array | array |    same value |
| array | object |  object representation, `[1, "abc"]` => `{"0": 1, "1": "abc"}` |
| object | number |    `null` |
| object | integer |   `null` |
| object | boolean |   empty object => `false`, otherwise `true` |
| object | string |    `null` |
| object | array |    object values, `{"key1": 1, "key2": "value 2"}` => `[1, "value 2"]` |
| object | object |  same value |
{:.table.table-striped}
