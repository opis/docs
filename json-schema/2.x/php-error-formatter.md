---
layout: project
library: json-schema
version: 2.x
title: Opis JSON Schema Error formatter
description: the opis json schema error formatter
keywords: opis, json, schema, error, format
---

We provide several helpers for formatting your errors. 
All of them can be accessed through the `Opis\JsonSchema\Errors\ErrorFormatter` class.

To present the available formatting options, let's start with the following code:

```php
use Opis\JsonSchema\{
    Validator,
    JsonPointer,
    ValidationResult,
};
use Opis\JsonSchema\Errors\{
    ErrorFormatter,
    ValidationError,
};

// Create a new validator
$validator = new Validator();

// Set max errors
$validator->setMaxErrors(5);

// Register a schema
$validator->resolver()->registerRaw(
<<<'JSON'
{
    "$id": "http://example.com/user.json",
    
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "minLength": 2,
            "maxLength": 100
        },
        "age": {
            "type": "integer",
            "minimum": 18,
            "maximum": 65
        }
    },
    
    "required": ["name", "age"]
}
JSON
);

// Our data
$data = (object)[
    'name' => 'A',
    'age' => 17,
];

/** @var ValidationResult $result */
$result = $validator->validate($data, 'http://example.com/user.json');

if ($result->isValid()) {
    // No errors here
    echo "Valid", PHP_EOL;
} else {
    // Get the error
    $error = $result->error();

    // Create an error formatter
    $formatter = new ErrorFormatter();

    // Print helper
    $print = function ($value) {
        echo json_encode(
            $value, 
            JSON_PRETTY_PRINT | JSON_UNESCAPED_SLASHES
            ), PHP_EOL;
        echo '-----------', PHP_EOL;
    };

    // Each format method is presented below
    // ...
}
```

## format method

This method will display custom errors set with [`$error` keyword](errors.html) inside schemas.
{:.alert.alert-info data-title="Remember"}

```php
// default
$print($formatter->format($error, false));
// default - multiple
$print($formatter->format($error, true));

$custom = function (ValidationError $error) use ($formatter) {
    $schema = $error->schema()->info();

    return [
        'schema' => [
            'id' => $schema->id(),
            'base' => $schema->base(),
            'root' => $schema->root(),
            'draft' => $schema->draft(),
            'path' => JsonPointer::pathToFragment($schema->path()),
            'contents' => $schema->data(),
            // see Opis\JsonSchema\Info\SchemaInfo for more properties
        ],
        'error' => [
            'keyword' => $error->keyword(),
            'args' => $error->args(),
            'message' => $error->message(),
            'formattedMessage' => $formatter->formatErrorMessage($error),
        ],
        'data' => [
            'type' => $error->data()->type(),
            'value' => $error->data()->value(),
            'fullPath' => $error->data()->fullPath(),
            // see Opis\JsonSchema\Info\DataInfo for more properties
        ],
    ];
};

$custom_key = function (ValidationError $error): string {
    return implode('.', $error->data()->fullPath());
};

// custom
$print($formatter->format($error, false, $custom, $custom_key));
// custom - multiple
$print($formatter->format($error, true, $custom, $custom_key));
```

{% capture out_1 %}
```json
{
  "/name": "Minimum string length is 2, found 1",
  "/age": "Number must be greater than or equal to 18"
}
```
{% endcapture %}


{% capture out_2 %}
```json
{
  "/name": [
    "Minimum string length is 2, found 1"
  ],
  "/age": [
    "Number must be greater than or equal to 18"
  ]
}
```
{% endcapture %}

{% capture out_3 %}
```json
{
  "name": {
    "schema": {
      "id": null,
      "base": "http://example.com/user.json#",
      "root": "http://example.com/user.json#",
      "draft": "2020-12",
      "path": "#/properties/name",
      "contents": {
        "type": "string",
        "minLength": 2,
        "maxLength": 100
      }
    },
    "error": {
      "keyword": "minLength",
      "args": {
        "min": 2,
        "length": 1
      },
      "message": "Minimum string length is {min}, found {length}",
      "formattedMessage": "Minimum string length is 2, found 1"
    },
    "data": {
      "type": "string",
      "value": "A",
      "fullPath": [
        "name"
      ]
    }
  },
  "age": {
    "schema": {
      "id": null,
      "base": "http://example.com/user.json#",
      "root": "http://example.com/user.json#",
      "draft": "2020-12",
      "path": "#/properties/age",
      "contents": {
        "type": "integer",
        "minimum": 18,
        "maximum": 65
      }
    },
    "error": {
      "keyword": "minimum",
      "args": {
        "min": 18
      },
      "message": "Number must be greater than or equal to {min}",
      "formattedMessage": "Number must be greater than or equal to 18"
    },
    "data": {
      "type": "integer",
      "value": 17,
      "fullPath": [
        "age"
      ]
    }
  }
}
```
{% endcapture %}

{% capture out_4 %}
```json
{
  "name": [
    {
      "schema": {
        "id": null,
        "base": "http://example.com/user.json#",
        "root": "http://example.com/user.json#",
        "draft": "2020-12",
        "path": "#/properties/name",
        "contents": {
          "type": "string",
          "minLength": 2,
          "maxLength": 100
        }
      },
      "error": {
        "keyword": "minLength",
        "args": {
          "min": 2,
          "length": 1
        },
        "message": "Minimum string length is {min}, found {length}",
        "formattedMessage": "Minimum string length is 2, found 1"
      },
      "data": {
        "type": "string",
        "value": "A",
        "fullPath": [
          "name"
        ]
      }
    }
  ],
  "age": [
    {
      "schema": {
        "id": null,
        "base": "http://example.com/user.json#",
        "root": "http://example.com/user.json#",
        "draft": "2020-12",
        "path": "#/properties/age",
        "contents": {
          "type": "integer",
          "minimum": 18,
          "maximum": 65
        }
      },
      "error": {
        "keyword": "minimum",
        "args": {
          "min": 18
        },
        "message": "Number must be greater than or equal to {min}",
        "formattedMessage": "Number must be greater than or equal to 18"
      },
      "data": {
        "type": "integer",
        "value": 17,
        "fullPath": [
          "age"
        ]
      }
    }
  ]
}
```
{% endcapture %}

{% include tabs.html 1="Default" 2="Default - Multiple" 3="Custom" 4="Custom - Multiple" _1=out_1 _2=out_2 _3=out_3 _4=out_4 %}


## formatOutput method

```php
$print($formatter->formatOutput($error, "flag"));
$print($formatter->formatOutput($error, "basic"));
$print($formatter->formatOutput($error, "detailed"));
$print($formatter->formatOutput($error, "verbose"));
```

{% capture out_1 %}
```json
{
    "valid": false
}
```
{% endcapture %}


{% capture out_2 %}
```json
{
  "valid": false,
  "errors": [
    {
      "keywordLocation": "#/properties",
      "instanceLocation": "#",
      "error": "The properties must match schema: name, age"
    },
    {
      "keywordLocation": "#/properties/name/minLength",
      "instanceLocation": "#/name",
      "error": "Minimum string length is 2, found 1"
    },
    {
      "keywordLocation": "#/properties/age/minimum",
      "instanceLocation": "#/age",
      "error": "Number must be greater than or equal to 18"
    }
  ]
}
```
{% endcapture %}

{% capture out_3 %}
```json
{
  "keywordLocation": "#/properties",
  "instanceLocation": "#",
  "valid": false,
  "errors": [
    {
      "keywordLocation": "#/properties/name/minLength",
      "instanceLocation": "#/name",
      "error": "Minimum string length is 2, found 1",
      "valid": false
    },
    {
      "keywordLocation": "#/properties/age/minimum",
      "instanceLocation": "#/age",
      "error": "Number must be greater than or equal to 18",
      "valid": false
    }
  ]
}
```
{% endcapture %}

{% capture out_4 %}
```json
{
  "keywordLocation": "#/properties",
  "instanceLocation": "#",
  "error": "The properties must match schema: name, age",
  "valid": false,
  "absoluteKeywordLocation": "http://example.com/user.json#/properties",
  "errors": [
    {
      "keywordLocation": "#/properties/name/minLength",
      "instanceLocation": "#/name",
      "error": "Minimum string length is 2, found 1",
      "valid": false,
      "absoluteKeywordLocation": "http://example.com/user.json#/properties/name/minLength"
    },
    {
      "keywordLocation": "#/properties/age/minimum",
      "instanceLocation": "#/age",
      "error": "Number must be greater than or equal to 18",
      "valid": false,
      "absoluteKeywordLocation": "http://example.com/user.json#/properties/age/minimum"
    }
  ]
}
```
{% endcapture %}

{% include tabs.html 1="Flag" 2="Basic" 3="Detailed" 4="Verbose" _1=out_1 _2=out_2 _3=out_3 _4=out_4 %}


## formatNested method

```php
// default
$print($formatter->formatNested($error));

$custom = function (ValidationError $error, ?array $subErrors = null) use ($formatter) {
    $ret = [
        'keyword' => $error->keyword(),
        'message' => $formatter->formatErrorMessage($error),
    ];

    if ($subErrors) {
        $ret['subErrors'] = $subErrors;
    }

    return $ret;
};

// custom
$print($formatter->formatNested($error, $custom));
```

{% capture out_1 %}
```json
{
  "message": "The properties must match schema: name, age",
  "keyword": "properties",
  "path": "/",
  "errors": [
    {
      "message": "Minimum string length is 2, found 1",
      "keyword": "minLength",
      "path": "/name"
    },
    {
      "message": "Number must be greater than or equal to 18",
      "keyword": "minimum",
      "path": "/age"
    }
  ]
}
```
{% endcapture %}

{% capture out_2 %}
```json
{
  "keyword": "properties",
  "message": "The properties must match schema: name, age",
  "subErrors": [
    {
      "keyword": "minLength",
      "message": "Minimum string length is 2, found 1"
    },
    {
      "keyword": "minimum",
      "message": "Number must be greater than or equal to 18"
    }
  ]
}
```
{% endcapture %}

{% include tabs.html 1="Default" 2="Custom" _1=out_1 _2=out_2 %}

## formatFlat method

```php
// default
$print($formatter->formatFlat($error));

$custom = function (ValidationError $error) use ($formatter) {
    return [
        'keyword' => $error->keyword(),
        'message' => $formatter->formatErrorMessage($error),

        'dataPath' => $formatter->formatErrorKey($error),
        'dataValue' => $error->data()->value(),
    ];
};

// custom
$print($formatter->formatFlat($error, $custom));
```

{% capture out_1 %}
```json
[
  "The properties must match schema: name, age",
  "Minimum string length is 2, found 1",
  "Number must be greater than or equal to 18"
]
```
{% endcapture %}

{% capture out_2 %}
```json
[
  {
    "keyword": "properties",
    "message": "The properties must match schema: name, age",
    "dataPath": "/",
    "dataValue": {
      "name": "A",
      "age": 17
    }
  },
  {
    "keyword": "minLength",
    "message": "Minimum string length is 2, found 1",
    "dataPath": "/name",
    "dataValue": "A"
  },
  {
    "keyword": "minimum",
    "message": "Number must be greater than or equal to 18",
    "dataPath": "/age",
    "dataValue": 17
  }
]
```
{% endcapture %}

{% include tabs.html 1="Default" 2="Custom" _1=out_1 _2=out_2 %}

## formatKeyed method

```php
// default
$print($formatter->formatKeyed($error));

$custom = function (ValidationError $error) use ($formatter) {
    return [
        'keyword' => $error->keyword(),
        'message' => $formatter->formatErrorMessage($error),
    ];
};

$custom_key = function (ValidationError $error): string {
    $path = implode('.', $error->data()->fullPath());
    return strtoupper($path);
};

// custom
$print($formatter->formatKeyed($error, $custom, $custom_key));
```

{% capture out_1 %}
```json
{
  "/name": [
    "Minimum string length is 2, found 1"
  ],
  "/age": [
    "Number must be greater than or equal to 18"
  ]
}
```
{% endcapture %}

{% capture out_2 %}
```json
{
  "NAME": [
    {
      "keyword": "minLength",
      "message": "Minimum string length is 2, found 1"
    }
  ],
  "AGE": [
    {
      "keyword": "minimum",
      "message": "Number must be greater than or equal to 18"
    }
  ]
}
```
{% endcapture %}

{% include tabs.html 1="Default" 2="Custom" _1=out_1 _2=out_2 %}
