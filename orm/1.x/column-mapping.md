---
layout: project
library: orm
version: 1.x
title: Column mapping
---

## Type casting

You can cast column values to specific types by using the `cast` method.
The following types are supported: `integer` or `int`, `float`, `boolean` or `bool`,
`string`, `date`, `json`, and `json-assoc`.

```php
class User extends Entity implements IEntityMapper
{
    public static function mapEntity(EntityMapper $mapper)
    {
        $mapper->cast([
            'age' => 'integer', // To integer number (alternatively int)
            'height' => 'float', // To float number
            'active' => 'boolean', // To boolean value (alternatively bool)
            'name' => 'string', // To string value
            'birthday' => 'date', // To DateTime object,
            'data1' => 'json', // To JSON (native objects)
            'data2' => 'json-assoc', // To JSON (associative arrays)
        ]);
    }
}
```

If your column allows `null` values you can specify this by prefixing the casting type
with an `?` sign.

```php
$mapper->cast([
    'description' => '?string'
]);
```

## Getters and setters

For each column, you can specify a callback that will be called by the data mapper,
when the value is read from a column or when a value is set to a column.

Setting a callback when a column's value is set, can be done with the help of 
the `setter` method. This callback is called before casting the value's type.

```php
$mapper->setter('password', function(string $value){
    return password_hash($value);
});
```

Adding a getter callback is done by using the `getter` method. This callback
is called after casting the value's type.

```php
$mapper->getter('name', function(string $value){
    return strtolower($value);
});
```

## Assignable and guarded columns

The `assignable` method allows developers to specify which columns can be mass-assigned with
the help of the data mapper's `assign` method.

```php
$mapper->assiganble(['name', 'description', 'age']);
```

The `guarded` method allows developers to specify which columns can not be mass-assigned with
the help of the data mapper's `assign` method.

```php
$mapper->guarded(['title', 'content']);
```

When mapping an entity, you should use only one of the above methods.

