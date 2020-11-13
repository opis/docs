---
layout: project
library: database
version: 4.x
title: Expressions
description: Learn about expressions
---

Expressions can be used to build complex queries. Using an expression to add a complex 
filter condition is simply a matter of passing a closure to the condition method. 
You can use then the object passed to the closure as an argument, to build your custom expressions. 
The methods used to build expressions are `column`, `op` and `value`.



{% capture php %}
```php
$result = $db->from('numbers')
             ->where('c')->eq(function($expr){
                $expr->column('b')->op('+')->value(10);
             })
             ->select()
             ->all();
```
{% endcapture %}
{% capture sql%}
```sql
SELECT * FROM `numbers` WHERE `c` = `b` + 10
```
{% endcapture %}
{% include tabs.html 1="PHP" 2="SQL" _1=php _2=sql %}

The `column` method is used to add to the expression a value that must be treated 
as a column name. The `value` method is used to add an arbitrary value that must 
treated as user input and handled properly. The `op` method is used to add a raw 
value to the expression.

The `op` method can be replaced by curly brackets.

{% capture php %}
```php
$result = $db->from('numbers')
             ->where('c')->eq(function($expr){
                $expr->column('a')->{'+'}->column('b');
             })
             ->select()
             ->all();
```
{% endcapture %}
{% capture sql%}
```sql
SELECT * FROM `numbers` WHERE `c` = `a` + `b`
```
{% endcapture %}
{% include tabs.html 1="PHP" 2="SQL" _1=php _2=sql %}

You can also use an expression on the left-hand side of a `WHERE` condition.
Note that you must pass `true` as the second argument in order to tell the query compiler to treat
that closure as an expresion not as a group.

{% capture php %}
```php
$result = $db->from('users')
             ->where(function($expr){
                $expr->lcase('name');
             }, true)->like('%foo%')
             ->select()
             ->all();
```
{% endcapture %}
{% capture sql%}
```sql
SELECT * FROM `users` WHERE LOWER(`name`) LIKE '%foo%'
```
{% endcapture %}
{% include tabs.html 1="PHP" 2="SQL" _1=php _2=sql %}

There are situations when is desirable to use an expression only on the left-hand side of a `WHERE` condition.
You can do this by using the `nop` method.

{% capture php %}
```php
$result = $db->from('users')
             ->where(function($expr){
                $expr->op('match(')->column('name')
                     ->op(') against(')->value('expression')->op(')');
             }, true)->nop()
             ->select()
             ->all();
```
{% endcapture %}
{% capture sql%}
```sql
SELECT * FROM `users` WHERE match(`name`) against('expression')
```
{% endcapture %}
{% include tabs.html 1="PHP" 2="SQL" _1=php _2=sql %}