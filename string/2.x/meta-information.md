---
layout: project
library: string
version: 2.x
title: Meta information methods
description: Methods used to obtain meta information about multibyte string instance.
keywords: opis, string, multibyte, unicode, utf-8, lower case, upper case, length, ascii 
---

## length

Returns the length of the string.

**signature**

```php
public function length(): int;
```

**usage**

```php
echo $str->length(); //> 10
```

## isEmpty

Checks if the current string is empty.

**signature**

```php
public function isEmpty(): bool;
```

**usage**

```php
if($str->isEmpty()){
    //...
}
```

## isLowerCase

Checks if the current string is in lower case.

**signature**

```php
public function isLowerCase(): bool;
```

**usage**

```php
if($str->isLowerCase()){
    //...
}
```

## isUpperCase

Checks if the current string is in upper case.

**signature**

```php
public function isUpperCase(): bool;
```

**usage**

```php
if($str->isUpperCase()){
    //...
}
```

## isAscii

Checks if the current string contains only ASCII characters.

**signature**

```php
public function isAscii(): bool;
```

**usage**

```php
if($str->isAscii()){
    //...
}
```

## chars

Returns an array of chars.

**signature**

```php
/**
 * @return string[]
 */
public function chars(): array;
```

## charAt

Returns the char at the specified position.
Position can be negative.
If position is invalid, an empty string is returned.

**signature**

```php
public function charAt(int $position): string;
```

**usage**

```php
$char = $str->charAt(3);
$char = $str->charAt(-1); // same as $str->charAt($str->length() - 1)
```

You can also access chars using the `str[index]` syntax.
{:.alert.alert-info data-title="Important"}

```php
$char = $str[3]; // same as $str->charAt(3)
$char = $str[-1]; // same as str->charAt(-1)
```

## codePoints

Returns an array of code points.

**signature**

```php
/**
 * @return int[]
 */
public function codePoints(): array;
```

## codePointAt

Returns the code point at the specified position.
Position can be negative.
If position is invalid, `-1` is returned.

**signature**

```php
public function codePointAt(int $position): int;
```

**usage**

```php
$cp = $str->codePointAt(3);
$cp = $str->codePointAt(-1); // same as $str->codePointAt($str->length() - 1)
```

You can also access code points using the `str(index)` syntax.
{:.alert.alert-info data-title="Important"}

```php
$cp = $str(3); // same as $str->codePointAt(3)
$cp = $str(-1); // same as str->codePointAt(-1)
```
