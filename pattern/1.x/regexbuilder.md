---
layout: project
library: pattern
version: 1.x
title: RegexBuilder class
description: Usage of RegexBuilder from Opis Pattern
keywords: opis, pattern, regex, builder, class, regular expression
---

The `Opis\Pattern\RegexBuilder` class will help you convert
patterns to regular expressions and extract placeholder values or names.

**usage**

```php
use Opis\Pattern\RegexBuilder;

$options = []; // see available options
$builder = new RegexBuilder($options);
// ...
````

For a list of available options, please check out the
[options page](options.html).
{:.alert.alert-info}

## Methods

### getRegex

Converts the [pattern](pattern.html) to a regex, using the placeholders as substitutions.

If a placeholder doesn't have a corresponding regex, the inline regex
of the placeholder will be used, and if it doesn't have an inline regex,
the [default regex](options.html#default_regex_exp) will be used.

An anonymous placeholders (without name) must always have an inline regex.
{:.alert.alert-warning}

**signature**

```php
/**
 * @param string $pattern
 * @param string[] $placeholders Optional, an array having on key the
 * placeholder name, and on value a regex expression
 *
 * @return string
 */
public function getRegex(string $pattern, array $placeholders = []): string
```

**usage**

```php
$pattern = "/{a}/{b=\d}/{c?=[a-z]+}";

echo $builder->getRegex($pattern);
// > ~^(?:/(?P<a>(?:[^/]+)))(?:/(?P<b>(?:\d)))(?:/(?P<c>(?:[a-z]+)))?/?$~u


echo $builder->getRegex($pattern, [
    'b' => '', // this will overwrite the inline regex
]);
// > ~^(?:/(?P<a>(?:[^/]+)))(?:/(?P<b>(?:[a-z]+)))(?:/(?P<c>(?:[a-z]+)))?/?$~u

// Anonymous placeholders
echo $builder->getRegex("{=\d}/{?=[a-z]}");
// > ~^(?:\d)(?:/(?:[a-z]))?/?$~u
```

### getNames

Returns a list with placeholder names from a [pattern](pattern.html).

**signature**

```php
/**
 * @param string $pattern
 * @return string[]
 */
public function getNames(string $pattern]): array
```

**usage**

```php
$names = $builder->getNames("/{a}/{b=\d}/{c?=[a-z]+}");
// ["a", "b", "c"]

$names = $builder->getNames("{=\d}/{abc?=[a-z]}/{?=a|b}");
// ["abc"]
```

### getValues

Extracts the values of the named placeholders used in path.

**signature**

```php
/**
 * @param string $regex
 * @param string $path
 * @return string[]
 */
public function getValues(string $regex, string $path): array
```

**usage**

```php
$pattern = "{=a|b}/{a}/{b=\d}/{c?=[a-z]+}";

$regex = $builder->getRegex($pattern);

$values = $builder->getValues($regex, "a/test/2/abc");
// ["a" => "test", "b" => "2", "c" => "abc"]

$values = $builder->getValues($regex, "b/test/2");
// ["a" => "test", "b" => "2"]

$values = $builder->getValues($regex, "c/test/2/abc");
// []
```

### getOptions

Returns the options used by the builder.

**signature**

```php
public function getOptions(): array
```

**usage**

Please check out the [available options](options.html).

### matches

Indicates if a path matches a regex.

**signature**

```php
public function matches(string $regex, string $path): bool
```

**usage**

```php
$pattern = "/{a}/{b=\d}/{c?=[a-z]+}";

$regex = $builder->getRegex($pattern);

if ($builder->matches($regex, "/test/5/abc")) {
    // ...
}
```

A call to this method is equivalent to a call to `preg_match`

```php
$builder->matches($regex, $path) === (bool)preg_match($regex, $path)
````
