---
layout: project
library: json-schema
version: 2.x
title: Filters ($filters)
description: using custom filters in json schema to validate data
keywords: opis, json, schema, filter, $filters, validation
---

JSON Schema specification contains a lot of keywords to validate data,
but most of them are only for _range check_ (like minimum, maximum, minLength, ...).
So, what happens if you want to check if something exists in a database?
Well, there cannot be such thing in json schema because it requires a lot 
of information (hostname, username, pass, query, ...) and it will be a pain
to debug or reuse the schema, not to mention about security concerns.

That's why we created a way to [add PHP logic as filters in json schema](php-filter.html), 
by adding a new keyword named `$filters`.

You can disable $filters keyword by setting the [`allowFilters` option](php-loader.html#parser-options) to `false`.
{:.alert.alert-info data-title="Remember"}

## General structure

In a JSON Schema document, `$filters` can be: 
a string, an object or an array of strings and objects.

If your filter doesn't need any arguments (besides the value that is validated)
you can use it like a string.

```json
{
  "$filters": "myFilter"
}
```

If you need to send some arguments to filter use an object,
where `$func` keyword holds the filter name and `$vars` keyword (optional) holds
a map of arguments (see more info about [$vars](variables.html)).

```json
{
  "$filters": {
    "$func": "myFilter",
    "$vars": {
      "arg-name-1": 2,
      "arg-other": "something else" 
    }
  }
}
```

You can even use multiple filters by creating an array.

```json
{
  "$filters": [
    "firstFilter", 
    {
      "$func": "secondFilter",
      "$vars": {
        "var1": 1,
        "var2": "value"
      }
    },
    "lastFilter"
  ]
}
```

Please note that if you use an array of filters and one filter doesn't
validate the data, the remaining filters will not be called.
{:.alert.alert-warning data-title="Important"}

## Builtin filters

All filters are defined on the `default` namespace, meaning that you can use 
them without writing the namespace (`default::filter-name` is the same as `filter-name`).

### Any data type

#### equals

Check if the current value equals another value.

Arguments

- `value` (required) the value to compare with
- `strict` (optional) comparison mode (`true` for strict check)

```json
{
  "$filters": {
    "$func": "equals",
    "$vars": {
      "value": "abc",
      "strict": true
    }
  }
}
```

#### schema-exists

Checks if current value is an uri, uri-template or json-pointer, and can be resolved to a schema.

Arguments

- `vars` (optional) replacements used for uri-templates
- `ref` (optional) replace current value with the value of this argument

```json
{
  "$filters": {
    "$func": "schema-exists",
    "$vars": {
      "vars": {
        "var1": 1,
        "var2": "value"
      },
      "ref": "/some/path/{var1}?q={var2}"
    }
  }
}
```

#### data-exists

Checks if the current value is a JSON pointer that can resolve the data.

Arguments

- `ref` (optional) replaces the current value

```json
{
  "$filters": {
    "$func": "data-exists",
    "$vars": {
      "ref": "/json/pointer/applied/to/data"
    }
  }
}
```

#### global-exists

Checks if a [global variable](variables.html#global-variables) exists.

Arguments

- `var` (optional) replaces the current value
- `value` (optional) if provided,the value of the global variable must equal the value of this argument

```json
{
  "$filters": {
    "$func": "global-exists",
    "$vars": {
      "var": "GLOBAL-VAR-NAME",
      "value": "some value"
    }
  }
}
```

#### slot-exists

Checks if a [slot](slots.html) exists.

Arguments

- `slot` (optional) replaces the current value

```json
{
  "$filters": {
    "$func": "slot-exists",
    "$vars": {
      "slot": "some-slot-name"
    }
  }
}
```

#### filter-exists

Checks if a [filter](filters.html) exists.

Arguments

- `filter` (optional) replaces the current value
- `type` (optional) the filter must apply to this data type

```json
{
  "$filters": {
    "$func": "filter-exists",
    "$vars": {
      "filter": "my-filter",
      "type": "number"
    }
  }
}
```

#### format-exists

Checks if a [format](formats.html) exists.

Arguments

- `format` (optional) replaces the current value
- `type` (optional) the format must apply to this data type

```json
{
  "$filters": {
    "$func": "format-exists",
    "$vars": {
      "filter": "my-format",
      "type": "integer"
    }
  }
}
```

### String

#### regex

Checks if the current value matches a pattern.

Arguments

- `pattern` (required) the regex pattern, 
  in the format accepted by the [preg_match function](https://www.php.net/manual/en/function.preg-match.php){:rel="nofollow" target="_blank"}.

```json
{
  "$filters": {
    "$func": "regex",
    "$vars": {
      "pattern": "/^a/i"
    }
  }
}
```

#### min-date

Checks if the current value is greater than or equal to the date from the `value` argument.

Arguments

- `value` (required) the date to compare with
- `timezone` (optional) timezone to use

```json
{
  "$filters": {
    "$func": "min-date",
    "$vars": {
      "value": "1970-01-01",
      "timezone": "UTC"
    }
  }
}
```

#### max-date

Checks if the current value is lower than or equal to the date from the `value` argument.

Arguments

- `value` (required) the date to compare with
- `timezone` (optional) timezone to use

```json
{
  "$filters": {
    "$func": "max-date",
    "$vars": {
      "value": "1970-01-01",
      "timezone": "UTC"
    }
  }
}
```

#### not-date

Checks if the current value is not equal to the date from the `value` argument.

Arguments

- `value` (required) the date to compare with, can be an array to compare with multiple values
- `timezone` (optional) timezone to use

```json
{
  "$filters": {
    "$func": "not-date",
    "$vars": {
      "value": ["1970-01-01", "2021-01-01"],
      "timezone": "UTC"
    }
  }
}
```

#### min-time

Checks if the current value is greater than or equal to the time from the `value` argument.

Arguments

- `value` (required) the time to compare with

```json
{
  "$filters": {
    "$func": "min-time",
    "$vars": {
      "value": "20:21:59"
    }
  }
}
```

#### max-time

Checks if the current value is lower than or equal to the time from the `value` argument.

Arguments

- `value` (required) the time to compare with

```json
{
  "$filters": {
    "$func": "max-time",
    "$vars": {
      "value": "20:21:59"
    }
  }
}
```

#### min-datetime

Checks if the current value is greater than or equal to the datetime from the `value` argument.

Arguments

- `value` (required) the datetime to compare with
- `timezone` (optional) timezone to use

```json
{
  "$filters": {
    "$func": "min-datetime",
    "$vars": {
      "value": "1970-01-01T19:00:00Z",
      "timezone": "UTC"
    }
  }
}
```

#### max-datetime

Checks if the current value is lower than or equal to the datetime from the `value` argument.

Arguments

- `value` (required) the datetime to compare with
- `timezone` (optional) timezone to use

```json
{
  "$filters": {
    "$func": "max-datetime",
    "$vars": {
      "value": "1970-01-01T19:00:00Z",
      "timezone": "UTC"
    }
  }
}
```