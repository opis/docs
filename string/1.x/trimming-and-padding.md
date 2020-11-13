---
layout: project
library: string
version: 1.x
title: Trimming and padding methods
description: Methods used to perform trimming and padding on multibyte strings.
keywords: opis, string, multibyte, unicode, utf-8, trim, pad, prefix, suffix  
---

## trim

Trims the characters specified in the character mask from both sides of the current string.

**signature**

```php
/**
 * @param string|self $character_mask Optional
 * @return self
 */
public function trim($character_mask = " \t\n\r\0\x0B"): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('  abc  ')->trim(); //> abc
echo wstring::from('xxxabcxxx')->trim('x'); //> abc
```

## trimLeft

Trims the characters specified in the character mask from the left side of the current string.

**signature**

```php
/**
 * @param string|self $character_mask Optional
 * @return self
 */
public function trimLeft($character_mask = " \t\n\r\0\x0B"): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('xxxabcxxx')->trimLeft('x'); //> abcxxx
```

## trimRight

Trims the characters specified in the character mask from the right side of the current string.

**signature**

```php
/**
 * @param string|self $character_mask Optional
 * @return self
 */
public function trimRight($character_mask = " \t\n\r\0\x0B"): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('xxxabcxxx')->trimRight('x'); //> xxxabc
```

## padLeft

Pads the left side of the current string to a given length using the specified text.

**signature**

```php
/**
 * @param int $length
 * @param string|self $char Optional
 * @return self
 */
public function padLeft(int $length, $char = " "): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abc')->padLeft(5, 'x'); //> xxabc
```

## padRight

Pads the right side of the current string to a given length using the specified text.

**signature**

```php
/**
 * @param int $length
 * @param string|self $char Optional
 * @return self
 */
public function padRight(int $length, $char = " "): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abc')->padRight(5, 'x'); //> abcxx
```

## ensurePrefix

Makes sure that the current string is prefixed with the given text. 
The checking can be done both in a case sensitive and case insensitive manner.

**signature**

```php
/**
 * @param string|self $text
 * @param bool $ignoreCase Optional
 * @return self
 */
public function ensurePrefix($text, bool $ignoreCase = false): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

// Case sensitive
echo wstring::from('abc')->ensurePrefix('a'); //> abc
echo wstring::from('abc')->ensurePrefix('A'); //> Aabc

//Case insensitive
echo wstring::from('abc')->ensurePrefix('A', true); //> abc
echo wstring::from('Abc')->ensurePrefix('a', true); //> Abc
```

## ensureSuffix

Makes sure that the current string is suffixed with the given text. 
The checking can be done both in a case sensitive and case insensitive manner.

**signature**

```php
/**
 * @param string|self $text
 * @param bool $ignoreCase Optional
 * @return self
 */
public function ensureSuffix($text, bool $ignoreCase = false): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

// Case sensitive
echo wstring::from('abcd')->ensureSuffix('d'); //> abcd
echo wstring::from('abcd')->ensureSuffix('d'); //> abcdD

//Case insensitive
echo wstring::from('abcd')->ensureSuffix('D', true); //> abcd
echo wstring::from('abcD')->ensureSuffix('d', true); //> abcD
```