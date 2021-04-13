---
layout: project
library: string
version: 2.x
title: Helper methods
description: Various string helpers.
keywords: opis, string, multibyte, unicode, utf-8, code points
---

## getCharFromCodePoint

Returns the string associated with the code point.

**signature**

```php
public static function 
    getCharFromCodePoint(int $code): string;
```

## getCharsFromCodePoints

Returns the chars associated with the specified code points.

**signature**

```php
public static function 
    getCharsFromCodePoints(array $codes, int $mode = self::KEEP_CASE): array;
```

## getCodePointsFromString

Returns the code points of the string.

**signature**

```php
public static function 
    getCodePointsFromString(string $str, int $mode = self::KEEP_CASE): array;
```

## getStringFromCodePoints

Converts all code points to chars and returns the resulted string. Invalid code points are ignored.

**signature**

```php
public static function 
    getStringFromCodePoints(array $codes, int $mode = self::KEEP_CASE): string
```

## getMappedCodePoints

Maps code points to other string case.

**signature**

```php
public static function 
    getMappedCodePoints(array $codes, int $mode): array;
```

## isValidCodePoint

Checks if a code point is valid.

**signature**

```php
public static function isValidCodePoint(int $codePoint): bool;
```

## walkString

Walks a string.

**signature**

```php
public static function walkString(string $str): iterable;
```

The key represents the current char index. 

Value is a two element array
- first element is an integer representing the code point
- second element is an array of integers (length 1 to 4) representing bytes

```php
use Opis\String\UnicodeString as wstring;

foreach (wstring::walkString('abc') as $charIndex => [$codePoint, $bytes]) {
    // ...
}
```
