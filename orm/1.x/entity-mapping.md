---
layout: project
library: orm
version: 1.x
title: Mapping entities
---

## Entity mappers

By default, every entity class has an associated entity mapper that provides
information about how that specific entity class maps to the SQL table that it represents.
The entity mapper is an instance of a class that implements the `Opis\ORM\IEntityMapper` 
interface and its purpose is to provide a simple and effective way of describing
the relationship between an entity class and its corresponding table or between a
group of related entities.

Changing the default settings of an entity mapper is done by registering a callback
function to the entity manager. The library provides multiple ways of doing that.
The first way - which is the easiest and also the recommended way of registering a callback -
is by implementing the `Opis\ORM\IMappableEntity` on an entity class.

```php
use Opis\ORM\{
    Entity, IEntityMapper, IMappableEntity
};

class User extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        // Map entity here
    }
}
```

Another way of registering this callback, is at the construct-time of the entity manager.

```php
use Opis\Database\Connection;
use Opis\ORM\{
    EntityManager, IEntityMapper
};
use MyBlog\{
    User, Article
};

// Define a database connection
$connection = new Connection("dsn:mysql;dbname=test", "root", "secret");

// callbacks
$mappers = [
    User::class => function(IEntityMapper $mapper) {
        // map entity here
    },
    Article::class => function(IEntityMapper $mapper) {
        // map entity here
    }
];

// Create an entity manager
$orm = new EntityManager($connection, $mappers);
```

The third way of doing it, is by calling the `registerMappingCallback` method
on the entity manager object instance.

```php
$orm->registerMappingCallback(User::class, function(IEntityMapper $mapper){
  // map entity here
});
```

## Entity name

The name of an entity is automatically derived from its class name. Therefore, if
the class of an entity is `My\Blog\Foo`, the resulted entity name will be `foo`.
If the entity's class is `My\Blog\FooBar`, the resulted entity name will be `foo_bar`.
The entity name is important because is used to help automate the mapping process.
Because in PHP class names are case-insensitive, there would be some situations
when you will want to set the entity name explicitly. This is done with the help
of the `entityName` method.

```php
class User extends Entity implements IEntityMapper
{
    public static function mapEntity(EntityMapper $mapper)
    {
        $mapper->entityName('user');
    }
}
```


## Changing table name

The corresponding table of an entity is derived from the entity name plus the `s` suffix. 
Therefore, if a class `My\Blog\User` represents an entity, then
the corresponding table would be `users`. 
Of course, this is not always a desired behavior. Changing the table's name is
simply a matter of calling the `table` method on the entity mapper instance.

```php
class User extends Entity implements IEntityMapper
{
    public static function mapEntity(EntityMapper $mapper)
    {
        $mapper->table('registred_users');
    }
}
```

## Changing primary key

By default, **Opis ORM** assumes that your table's primary key column is named `id`. 
If your primary key column has a different name, you must specify this by using the
`primaryKey` method.

```php
class User extends Entity implements IEntityMapper
{
    public static function mapEntity(EntityMapper $mapper)
    {
        $mapper->primaryKey('user_id');
    }
}
```

The library provides support for composite primary keys as well

```php
class Foo extends Entity implements IEntityMapper
{
    public static function mapEntity(EntityMapper $mapper)
    {
        $mapper->primaryKey('column_1', 'column_2', 'column_n');
    }
}
```

## Primary key generator

You can use a generator to automatically assign a value for your primary key.
Registering a generator is done by passing a callback to the `primaryKeyGenerator` method.

```php
class User extends Entity implements IEntityMapper
{
    public static function mapEntity(EntityMapper $mapper)
    {
        $mapper->primaryKeyGenerator(function(){
            // Generate PK
            return generate_some_pk();
        });
    }
}
```

If you need to generate a composite primary key, simply return a key-value mapped array

```php
class Foo extends Entity implements IEntityMapper
{
    public static function mapEntity(EntityMapper $mapper)
    {
        $mapper->primaryKeyGenerator(function(){
            // Generate PK
            return [
                'column_1' => 'value1',
                'column_2' => 'value2',
            ];
        });
    }
}
```


## Sequence object

**Opis ORM** allows you to create entities without explicitly adding a value for
the [primary key](#changing-primary-key). This is useful when you are relying on the 
auto-increment mechanism of a database system to assign an ID for your newly created
record. To retrieve the ID of the newly created record, **Opis ORM** uses the PDO's 
`lastInsertId` method, which require - for some database systems, like PostgreSQL - to
provide the name of the sequence object from which the ID will be returned. 
By default, this name is constructed using the following pattern: *table name* + `_` +
*primary key* + `_seq`. For an entity `My\Blog\User` with a primary key column named `id`,
the name of the sequence object would be `users_id_seq`. If your sequence object is named
differently, you must specify this by using the `sequence` method.

```php
class User extends Entity implements IEntityMapper
{
    public static function mapEntity(EntityMapper $mapper)
    {
        $mapper->sequence('table_users_seq');
    }
}
```
