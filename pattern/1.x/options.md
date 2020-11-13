---
layout: project
library: pattern
version: 1.x
title: RegexBuilder options
description: List of the available options for Opis Pattern RegexBuilder
keywords: opis, pattern, regex, builder, options
---

All options presented on this page are available as constants on [`Opis\Pattern\RegexBuilder`](regexbuilder.html)
class.

## Separators

### SEPARATOR_SYMBOL

Indicates the character used for delimiting [segments](pattern.html#segments).

**Default value**: `/` (slash)

|  Symbol | Pattern | Segments |
|---|---|---|
| `/` | `a/b/c` | `a`, `b` and `c` |
| `.` |  `a.b.c` | `a`, `b` and `c` |
| `;` | `a;b;c` |  `a`, `b` and `c` |
{:.table.table-striped}

### CAPTURE_MODE

**Default value**: `CAPTURE_LEFT | ALLOW_OPT_TRAIL`

The value of this option must be a bitmask consisting of the following options:

##### CAPTURE_LEFT

If present, this affects segments that contain only one [optional placeholder](#opt_symbol).
If that segment has a separator on the left side, then the separator
becomes optional.

For `{a}/{b?}` pattern, the regex looks like `^{a}(/{b})?$`,
where `{a}` and `{b}` are regex patterns, and `/` is
[separator symbol](#separator_symbol).

##### CAPTURE_RIGHT

If present, this affects segments that contain only one [optional placeholder](#opt_symbol).
If that segment has a separator on the right side, then the separator
becomes optional.

For `{a?}/{b}` pattern, the regex looks like `^({a}/)?{b}$`,
where `{a}` and `{b}` are regex patterns, and `/` is
[separator](#separator_symbol).

##### ALLOW_OPT_TRAIL

If present, this option will allow a trailing
[separator symbol](#separator_symbol) in the value.

For `{a}` pattern, the regex looks like `^{a}/?$`,
where `{a}` is a regex pattern, and `/` is
[separator](#separator_symbol).

Examples

|  Pattern | Path | Matches if trailing / no trailing |
|---|---|---|
| `a/b/c` | `a/b/c` | Yes / Yes |
| `a/b/c` | `a/b/c/` | Yes / No |
| `a/b/c/` | `a/b/c` | Yes / No |
| `a/b/c/` | `a/b/c/` | Yes / Yes |
{:.table.table-striped}

## Placeholders

### START_SYMBOL

This symbol indicates where a [placeholder](pattern.html#placeholder) starts.

**Default value**: `{` (open brace)

### END_SYMBOL

This symbol indicates where a [placeholder](pattern.html#placeholder) ends.

**Default value**: `}` (close brace)

### OPT_SYMBOL

This symbol used to mark a [placeholder as optional](pattern.html#optional-placeholder).

**Default value**: `?` (question mark)

#### ASSIGN_SYMBOL

This symbol is used to mark the begining of the [inline regex](pattern.html#inline-regex)
 for placeholder.

**Default value**: `=` (equal)

### DEFAULT_REGEX_EXP

The value of this option is used when a [placeholder](pattern.html#placeholder)
doesn't have a defined regex.

**Default value**: `[^{separator}]+` where `{separator}`
is replaced with the value of the [separator symbol](#separator_symbol).

## Options for regex

### REGEX_DELIMITER

**Default value**: `~` (tilde)

The [data component](pattern.html#data) is escaped using this value.

More info about regex delimiters can be found
[here](http://php.net/manual/en/regexp.reference.delimiters.php){:target="_blank"}.


### REGEX_MODIFIER

**Default value**: `u` (PCRE_UTF8 - enables utf-8 support)

More info about regex modifiers can be found
[here](http://php.net/manual/en/reference.pcre.pattern.modifiers.php){:target="_blank"}.
