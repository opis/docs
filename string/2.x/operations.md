---
layout: project
library: string
version: 2.x
title: Generic operation methods
description: Methods used to perform generic operations on multibyte strings, like replace, substring, split, append, prepend.
keywords: opis, string, multibyte, unicode, utf-8, replace, suubstring, split, reverse, repeat  
---

## append

Appends the given string to the current string.

**signature**

```php
/**
 * @param string|self $text
 * @param int $mode Optional, case type
 * @return self
 */
public function append($text, int $mode = self::KEEP_CASE): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abc')->append('def'); //> abcdef
```

## prepend

Prepends the given string to the current string.

**signature**

```php
/**
 * @param string|self $text
 * @param int $mode Optional, case type
 * @return self
 */
public function prepend($text, int $mode = self::KEEP_CASE): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abc')->prepend('def'); //> defabc
```

## insert

Inserts the given string at the specified position.

**signature**

```php
/**
 * @param string|self $text
 * @param int $position
 * @param int $mode Optional, case type
 * @return self
 */
public function insert($text, int $position, int $mode = self::KEEP_CASE): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abcdef')->insert('x', 3); //> abcxdef
```

## replace

Replaces the first occurrence of the given string with another one.

**signature**

```php
/**
 * @param string|self $subject
 * @param string|self $replace
 * @param int $offset Optional
 * @param bool $ignoreCase
 * @return self
 */
public function replace($subject, $replace, int $offset = 0, bool $ignoreCase = false): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abcabc')->replace('abc', 'foo'); //> fooabc

// Using an offset
echo wstring::from('abcabc')->replace('abc', 'foo', 2); //> abcfoo

// Ignore case
echo wstring::from('ABCabc')->replace('abc', 'foo', 0, true); //> fooabc
```

## replaceAll

Replaces all occurrences of the given string with another one.

**signature**

```php
/**
 * @param string|self $subject
 * @param string|self $replace
 * @param int $offset
 * @param bool $ignoreCase
 * @return self
 */
public function replaceAll($subject, $replace, int $offset = 0, bool $ignoreCase = false): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abcabc')->replaceAll('abc', 'foo'); //> foofoo
```

## remove

Removes an arbitrary amount of chars starting at a given position.

**signature**

```php
public function remove(int $index, int $length): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abcdef')->remove(2, 3); //> abf
```

## substring

Copies the specified portion of the string to another string.

**signature**

```php
public function substring(int $start, int $length = null): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abcdef')->substring(3); //> def
echo wstring::from('abcdef')->substring(3, 2); //> de
```

## split

Splits the current string using the given delimiter and returns an array of UTF-8 strings. 
If no delimiter was given, an UTF-8 string will be created for every char in the current string.

**signature**

```php
/**
 * @param string|self $char
 * @param bool $ignoreCase
 * @return self[]
 */
public function split($char = '', bool $ignoreCase = false): array;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

$str = wstring::from('a,b,c');
$tmp = '';

foreach($str->split(',') as $part){
    $tmp .= $part;
}

echo $tmp; //> abc
```

## reverse

Returns a string containing all the characters from the current string, but in reversed order.

**signature**

```php
public function reverse(): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abcdef')->reverse(); //> fedcba
```

## repeat

Repeats the current string an arbitrary number of times.

**signature**

```php
public function repeat(int $times = 1): self;
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abc')->repeat(); //> abcabc
echo wstring::from('abc')->repeat(2); //> abcabcabc
```