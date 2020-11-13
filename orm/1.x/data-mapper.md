---
layout: project
library: orm
version: 1.x
title: Data mapper
---

A data mapper is an instance of a class that implements the `Opis\Orm\IDataMapper` interface.
This interface provides several methods that can be used to manipulate the data
associated with an entity. This data represents a row that is or will be stored into a table.

## Getting column values

Getting the value of a column is done with the help of the `getColumn` method.

```php
class User extends Entity
{
    public function name(): string
    {
        return $this->orm()->getColumn('name');
    }
}
```

If a casting type was specified for this column, the method will take care of
casting the value to match the specified casting type. Also, if a 
getter callback was set for this column, the callback will be called by passing
the value as an argument and the result return by the callback will be considered the
new value of the column. At the end, the value of the column will be cached and
finally returned. Any subsequent calls to this method will result in returning
the cached value.

To clear the cached value, and force a rebuild, use the `clearColumn` method

```php
$this->orm()->clearColumn('name');
```

Getting the columns' raw value is done by using the `getRawColumns` method.

```php
$columns = $this->orm()->getRawColumns();
```

## Setting column values

Setting the value of a column is done by using the `setColumn` method.
This method will first call the setter callback (if any), then it
will cast the resulted value to match the specified type (if such casting rule was defined),
and in the end, the resulted value will be set as the raw value of the column. 

```php
class User extends Entity
{
    public function setName(string $value): void
    {
        $this->orm()->setColumn('name', $value);
    }
}
```

If you want to set the raw value directly, use the `setRawColumn` method

```php
$this->orm()->setRawColumn('foo', $value);
```

You can also perform a mass assignment of values, by using the `assign` method.

```php
$this->orm()->assign([
    'name' => 'John Doe',
    'age' => 24
]);
``` 

Getting a list of modified columns is done by calling the `getModifiedColumns` method

```php
$modified = $this->orm()->getModifiedColumns();
```

## Handling relationships

Getting a related entity is done by calling the `getRelated` method.
This method takes a single argument, representing the name of the relationship.

```php
class User extends Entity
{
    public function profile(): Profile
    {
        return $this->orm()->getRelated('profile');
    }
}
```

The value returned by calling this method will be cached, 
and subsequent calls to this method will return the cached value.
The cached value can be cleared by using the `clearRelated` method.

```php
$this->orm()->clearRelated('profile');
```

Setting a related entity value is done by using the `setRelated` method. The first argument
that this method takes, represents the relationship name, while the second argument
must be an instance of an entity or `null`.

```php
class User extends Entity
{
    public function setProfile(Profile $value): void
    {
        $this->orm()->setRelated('profile', $value);
    }
}
```

### Linked entities

If the type of the relationship between two entities is either `shareOne` or `shareMany`,
then setting a related entity is done with the help of the `link` method. 

```php
class User extends Entity
{
    public function addHobby(Hobby $value): void
    {
        $this->orm()->link('hobies', $value);
    }
}
```

This kind of relationships allow you to remove the link between two entities
and make them unrelated. This can be accomplished with the help of the `unlink` method.

```php
$this->orm()->unlink('hobbies', $value);
```

## Reload data

If you want to force a data mapper hydration, you can simply specify that
the values contained by the data mapper are stale.

```php
$this->orm()->stale();
```

## Meta information

Checking if the data mapper was altered, by either setting a column's value or 
by setting a relationship, is done with the help of the `wasModified` method.

```php
if($this->orm()->wasModifed()) {
    // do something
}
```

If you want to check if the row referenced by the data mapper was persisted or not into
a database, simply use the `isNew` method

```php
if ($this->orm()->isNew()) {
    // never persisted
}
```

Checking if the row referenced by the data mapper was deleted is done with the help of the
`isDeleted` method.

```php
if ($this->orm()->isDeleted()) {
    // row was deleted
}
```

You can also check if the row referenced by a data mapper is available only for reading

```php
if ($this->orm()->isReadOnly()) {
    // just for reading
}
```

