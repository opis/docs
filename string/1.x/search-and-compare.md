---
layout: project
library: string
version: 1.x
title: Search and compare methods
description: Methods used to perform comparisons between multibyte strings, as well as for searching for substrings.
keywords: opis, string, multibyte, unicode, utf-8, comparision, searching  
---

## equals

Checks if two strings contain the same sequence of characters.

**signature**

```php
/**
 * @param string|self $text
 * @param bool $ignoreCase Optional
 * @return bool
 */
public function equals($text, bool $ignoreCase = false): bool;
```

**usage**

```php
if($str->equals('abcd')){
    //...
}

// Case insensitive checking
if($str->equals('aBcD', true)){
    //...
}
```

## compareTo

Compares the current string with a given string and returns `0` if the strings are equal, 
`1` if the current string is greater and `-1` otherwise.

**signature**

```php
/**
 * @param string|self $text
 * @param bool $ignoreCase Optional
 * @return int
 */
public function compareTo($text, bool $ignoreCase = false): int;
```

**usage**

```php
echo $str->compareTo('abcd'); //> 1
echo $str->compareTo('aBcD', true); //> 1
```

## contains

Checks if the current string contains the given substring.

**signature**

```php
/**
 * @param string|self $text
 * @param bool $ignoreCase Optional
 * @return bool
 */
public function contains($text, bool $ignoreCase = false): bool;
```

**usage**

```php
if($str->contains('abcd')){
    //...
}

// Case insensitive checking
if($str->contains('aBcD', true)){
    //...
}
```

## startsWith

Checks if the current string starts with the given substring.

**signature**

```php
/**
 * @param string|self $text
 * @param bool $ignoreCase Optional
 * @return bool
 */
public function startsWith($text, bool $ignoreCase = false): bool;
```

**usage**

```php
if($str->startsWith('abcd')){
    //...
}

// Case insensitive checking
if($str->startsWith('aBcD', true)){
    //...
}
```

## endsWith

Checks if the current string ends with the given substring.

**signature**

```php
/**
 * @param string|self $text
 * @param bool $ignoreCase Optional
 * @return bool
 */
public function endsWith($text, bool $ignoreCase = false): bool;
```

**usage**

```php
if($str->endsWith('abcd')){
    //...
}

// Case insensitive checking
if($str->endsWith('aBcD', true)){
    //...
}
```

## indexOf

Finds the first occurrence of the given substring within the current string.
If no occurrence was found, `false` is returned.

**signature**

```php
/**
 * @param string|self $text
 * @param int $offset Optional
 * @param bool $ignoreCase Optional
 * @return int|bool
 */
public function indexOf($text, bool $ignoreCase = false);
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('abcabc')->indexOf('a'); //> 0
echo wstring::from('abcabc')->indexOf('a', 2); //> 3
echo wstring::from('abcabc')->indexOf('Ca', 2, false); //> 2
```

## lastIndexOf

Finds the last occurrence of the given substring within the current string.
If no occurrence was found, `false` is returned.

**signature**

```php
/**
 * @param string|self $text
 * @param int $offset Optional
 * @param bool $ignoreCase Optional
 * @return int|bool
 */
public function lastIndexOf($text, bool $ignoreCase = false);
```

**usage**

```php
use Opis\String\UnicodeString as wstring;

echo wstring::from('AbcAbcabc')->lastIndexOf('A'); //> 3

// Case insensitive
echo wstring::from('AbcAbcabc')->lastIndexOf('A', true); //> 6
```
