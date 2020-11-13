---
layout: project
library: pattern
version: 1.x
title: Pattern components
description: Components of Opis Pattern (segments, placeholders, data, inline regex).
keywords: opis, pattern, regex, component, placeholder, segment
---

A pattern is composed from one or more [segments](#segments) delimited by
 [separators](options.html#separator_symbol).

**Examples**

| Pattern | Separator | Segments |
|---|---|---|
| `/a/b/c` | `/` | `a`, `b`, `c` |
| `a/b` | `/` | `a`, `b` |
| `a.b.c/d` | `.` | `a`, `b`, `c/d` |
{:.table.table-striped}

## Segments

A segment can be one of the following:
- [data](#data)
- [placeholder](#placeholder)
- combination of data and placeholders

When a segment contains only one [optional placeholder](#optional-placeholder) and nothing more,
then the [separator symbol character](options.html#separator_symbol)
 also becomes optional, depending on the [capture mode](options.html#capture_mode).

The pattern `a//{p}/{p1?}/test-{p2}-for-{p3?}`
has the following segments
- `a` - data
- empty data
- `{p}` - placeholder
- `{p1?}` - optional placeholder
- `test-{p2}-for-{p3?}` - combination of data and placeholders

Above, the separator symbol used was `/` (slash), use the
[separator symbol option](options.html#separator_symbol)
to change it.

## Data

Data inside a [segment](#segments) is automatically escaped, depending
on the [regex delimiter option](options.html#regex_delimiter).

**Examples**

|Delimiter|Data|Escaped|
|---|---|---|
|`~`|`a~b`|`a\~b`|
|`#`|`a#b`|`a\#b`|
{:.table.table-striped}

## Placeholder

The placeholder is always delimited by the
[start symbol](options.html#start_symbol) and the [end symbol](options.html#end_symbol).
To change the placeholder delimiters please use those options.

Any placeholder is composed from:
- [name](#placeholder-name) (optional)
- [optional symbol](#optional-placeholder) (optional)
- [inline regex](#inline-regex) (optional if a name is present, required otherwise)

If a placeholder doesn't have a name, it is considered to be anonymous and must have
an inline regex. An anonymous placeholder can also be marked as optional.
{:.alert.alert-warning}

**Examples**

| Placeholders|Name|Optional|Regex|
|---|---|---|---|
| `{var}`|`var`|No|[default](options.html#default_regex_exp)|
|`{opt?}`|`opt`|Yes|[default](options.html#default_regex_exp)|
|`{num=\d+}`|`num`|No|`\d+`|
|`{num?=[a-z]{5,10}}`|`num`|Yes|`[a-z]{5,10}`|
|`{=[abc]*}`| *anonymous* |No|`[abc]*`|
|`{?=[abc]+}`| *anonymous* |Yes|`[abc]+`|
{:.table.table-striped}

### Placeholder name

The placeholder name must begin with a letter, and after the first letter
 can contain any number of letters, digits or underscores.

The name of the placeholder must be unique across the pattern.
{:.alert.alert-warning}

**Examples**

| Pattern | Valid | Reason |
|---|---|---|
| `{abc}` | Yes |  |
| `{A1B2c3}` | Yes |  |
| `{a_1_B_2}` | Yes |  |
| `{=\d}` | Yes |  |
| `{?=[a-z]{2, 3}}` | Yes |  |
| `{123A}` | No | Must start with a letter |
| `{_a_b}` | No | Must start with a letter |
| `{A-B}` | No | Contains `-` (minus sign) |
| `{?=}` | No | Must have inline regex |
| `{}` | No | Must have inline regex |
| `{=}` | No | Must have inline regex |
| `{a}/{b}/{a}` | No | Duplicate name `a` |
| `{a}/{b}/{A}` | Yes |  |
{:.table.table-striped}

### Optional placeholder

A placeholder can be marked as optional using the
[optional symbol](options.html#opt_symbol)
right after the placeholder [name](#placeholder-name).

If the placeholder is optional and is the only thing in the
[segment](#segments), it will also make the adjacent [separator](#options.html#separator_symbol) optional,
depending on the [capture mode](options.html#capture_mode).

**Example**

To make `{var}` optional, just add `?` after the name, resulting
`{var?}`.

### Inline regex

A placeholder can contain an inline regex using the
[assign symbol](options.html#assign_symbol) right after the
placeholder [name](#placeholder-name) if the placeholder is not anonymous, 
or after the [optional symbol](options.html#opt_symbol) if the placeholder is marked
as optional.

If a named placeholder doesn't have an inline regex, it uses the
[default regex expression](options.html#default_regex_exp).
{:.alert.alert-info}

**Examples**

To add an inline regex for `{var}`, add `=` and the regex, just after the name,
resulting `{var=my-regex}`.

To add an inline regex for `{var?}`, add `=` and the regex, just after the `?`,
resulting `{var?=my-regex}`.

To add an anonymous placeholder with inline regex, use `{=my-regex}` 
or `{?=my-regex}` if you want to mark this placeholder as optional.
