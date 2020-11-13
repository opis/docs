---
layout: project
library: string
version: 1.x
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

## codePoints

Returns an array of code points.

**signature**

```php
/**
 * @return int[]
 */
public function codePoints(): array;
```
