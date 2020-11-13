---
layout: project
library: string
version: 1.x
title: Conversion methods
description: Methods used to convert multibyte strings to lowercase, uppercase and ASCII.
keywords: opis, string, multibyte, unicode, utf-8, lowercase, uppercase, ascii
---

## toLower

Returns the lowercase version of the current string.

**signature**

```php
public function toLower(): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('ABC')->toLower(); //> abc
```

## toUpper

Returns the uppercase version of the current string.

**signature**

```php
public function toUpper(): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abc')->toUpper(); //> ABC
```

## toAscii

Returns the ASCII version (if possible) of the current string.

**signature**

```php
public function toAscii(): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('ăĂâÂîÎşŞţŢ')->toAscii(); //> aAaAiIsStT
```
