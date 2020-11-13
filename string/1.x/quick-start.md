---
layout: project
library: string
version: 1.x
title: Quick start
description: Examples about how to create and how to use multibyte strings 
 in PHP, with the help of Opis String library
keywords: opis, string, multibyte, unicode, utf-8, encoding, examples, quick start
---

Creating a new multibyte string is done by using the static method `from`.

```php
use Opis\String\UnicodeString as wstring;

$str = wstring::from('ăĂâÂîÎşŞţŢ');
```

By default, it's assumed that your string is encoded using UTF-8. 
If your string is encoded using another encoding, 
you can pass the name of the encoding as the second argument of the `from` method.

```php
use Opis\String\UnicodeString as wstring;

$str = wstring::from($string, 'ISO-8859-1');
```

Once created, you can use the resulted object in the same manner in which you would use a regular string.

```php
echo $str; //> ăĂâÂîÎşŞţŢ
echo 'foo ' . $str . ' bar'; //> foo ăĂâÂîÎşŞţŢ bar
echo $str[0]; //> ă
echo $str[4]; //> î
```

You can chain multiple methods to perform operations against a string.

```php
use Opis\String\UnicodeString as wstring;

$str = wstring::from('ăĂâÂîÎşŞţŢ');

echo $str->substring(3, 4)
         ->toUpper(); //> ÂÎÎŞ
```

The `UnicodeString` instances are immutable and work 
in a similar manner as C# or Java strings.
{:.alert.alert-info data-title="Important"}

```php
use Opis\String\UnicodeString as wstring;

$str = wstring::from('abcd');

echo $str->toUpper(); //> ABCD
echo $str; //> abcd
```

