---
layout: project
library: json-schema
version: 2.x
title: Custom errors
description: opis json schema custom errors
keywords: opis, json, schema, error
---

## Adding custom error messages

To add custom errors for keywords you should use `$error` as an object with the same properties as keyword names, 
and the values should be the error messages.

```json
{
  "type": "string",
  "minLength": 2,
  "pattern": "[a-zA-Z]+",
  "$error": {
    "minLength": "Too short, must be at least two letters long",
    "pattern": "Must contain only letters"
  }
}
```

## Specifying a fallback

If you don't want a custom error for every keyword present in a schema, you can specify a fallback with `*`.

```json
{
  "type": "string",
  "minLength": 2,
  "pattern": "[a-zA-Z]+",
  "$error": {
    "pattern": "Must contain only letters",
    "*": "Invalid data"
  }
}
```

## Schema error message

If you want a custom message to any schema error (no matter the keywords), just use a string as a value to `$error`.

```json
{
  "type": "string",
  "minLength": 2,
  "pattern": "[a-zA-Z]+",
  "$error": "Invalid data, give me what I expect"
}
```

You can apply the same logic for object properties (if you don't want to handle specific keywords):

```json
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "minLength": 2,
      "maxLength": 24,
      "$error": "Invalid name"
    },
    "age": {
      "type": "integer",
      "minimum": 18,
      "$error": "Invalid age"
    }
  }
}
```

## Handling required properties

To add custom errors to [`required` keyword](./object.html#required) you can use an object containing property name and
error message. Also, you can use `*` as fallback error message.

```json
{
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
  "required": ["name", "age"],
  "$error": {
    "required": {
      "name": "You must have a name",
      "age": "Age is mandatory"
    }
  }
}
```

## Handling filters

To add custom error messages to [filters](./filters.html) you should add `$error` inside filter's arguments.

```json
{
  "type": "string",
  "format": "date",
  "$filters": {
    "$func": "min-date",
    "$vars": {
      "value": "1970-01-01",
      "timezone": "UTC",
      "$error": "Cannot start before unix"
    }
  }
}
```