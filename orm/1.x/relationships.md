---
layout: project
library: orm
version: 1.x
title: Relationships
---

**Opis ORM** allows you to define multiple types of relationships between entities.
Defining a relationship is done by calling the `relation` method, on a entity mapper instance,
and passing a name that uniquely identifies that relation on that entity mapper.
The return value of this call is a factory object that provides several methods 
that will help you to declare the type of the relationship being defined.


## Has one

Setting a `has one` relation is done with the help of the `hasOne` method.
This kind of relationship can be defined when on the child table is set a unique
foreign key pointing to the parent table's primary key. 

***users** (parent table)*

|id|name|
|--|----|
|1|john|
|2|jane|
{:.table .table-bordered .table-striped}

***profiles** (child table)*

|id|user_id|age|avatar|
|--|-------|---|------|
|100|1|24|john.jpg|
|101|2|32|jane.jpg|
{:.table .table-bordered .table-striped}

```php
class User extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->relation('profile')->hasOne(Profile::class);
    }
}
```

By default, the name of the foreign key column is derived form the [entity name]
and the name of the candidate column.
For example, an entity named `foo`, that has a candidate column `bar`, would be
referenced by a foreign key set on a column named `foo_bar`.
If the foreign key is set to a different column,
you must specify this by using the `Opis\ORM\Core\ForeignKey` class.

```php
$mapper->relation('foo')->hasOne(Bar::class, new ForeignKey([
    'candidate_column' => 'foreign_key_column'
]));
```

To better exemplify, the above is just a shortcut for the following:

```php
class User extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $foreignKey = new ForeignKey([
            // users.id => profiles.user_id
            'id' => 'user_id',
        ]);
        $mapper->relation('profile')->hasOne(Profile::class, $foreignKey);
    }
}
```

The `ForeignKey` object can also be used to reference composite primary keys.

```php
$mapper->relation('foo')->hasOne(Bar::class, new ForeignKey([
    'candidate_column_1' => 'foreign_key_column_1',
    'candidate_column_2' => 'foreign_key_column_2',
]));
```


## Has many

A `has many` relation is defined by using the `hasMany` method. This type of
relationship is very similar with the `has one` relation type. The only 
difference is that the value contained by the column on which the foreign key
was set, doesn't have to be unique to the child table.

***users** (parent table)*

|id|name|
|--|----|
|1|john|
|2|jane|
{:.table .table-bordered .table-striped}

***articles** (child table)*

|id|user_id|title|content|
|--|-------|---|------|
|100|1|Hello, World!|Content for my article|
|101|1|Second article|Content for my second article|
|102|2|First article|Content for my first article|
{:.table .table-bordered .table-striped}


```php
class User extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->relation('articles')->hasMany(Article::class);
    }
}
```

Manually setting the foreign key column is done by passing
a `ForeignKey` object as the second argument.

```php
$mapper->relation('articles')->hasOne(Article::class, new ForeignKey([
    'id' => 'user_id'
]));
```

## Belongs to

This is the inverse relation for the `has one` and `has many` relationships. It
can be defined with the help of the `belongsTo` method

```php
class Article extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->relation('author')->belongsTo(User::class);
    }
}
```

As like for the previous relation types, manually setting the foreign key column is 
done by passing a `ForeignKey` object as the second argument.

```php
$mapper->relation('author')->hasOne(User::class, new ForeignKey([
    'id' => 'user_id', 
]));
```

## Many to many

Defining `many to many` relationships is done by using either the `shareOne` or the `shareMany`
methods, depending on the context. Establishing a `many to many` relationship between two entities is done with the
help of a so called junction table.

***employees** (parent table)*

|id|name|
|--|----|
|1|John|
|2|Jane|
|3|Chris|
|4|Jeff|
{:.table .table-bordered .table-striped}

***job_titles** (child table)*

|id|name|
|--|-------|
|1|CEO|
|2|Software developer|
|3|UI/UX designer|
{:.table .table-bordered .table-striped}

***employees_job_titles** (junction table)*

|employee_id|job_title_id|
|--|----|
|1|2|
|2|2|
|3|1|
|4|3|
{:.table .table-bordered .table-striped}


In the above example John and Jane are software developers, Chris is CEO, and Jeff is an
UI/UX designer. The relation between `Employee` and `JobTitle` entities
is set with the help of the `shareOne` method. That's because two, or more, employees
can share the same job title, but every employee can have only a single job title at a 
given moment.

```php
class Employee extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->relation('job_title')->shareOne(JobTitle::class);
    }
}
```

Of course, since a job title is shared by multiple employees, the
relation between a `JobTitle` and an `Employee` entity is set with the help of the 
`shareMany` method. 

```php
class JobTitle extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->relation('employees')->shareMany(Employee::class);
    }
}
```

The name of the junction table is determined from the table names
of related entities, alphabetically sorted. So, if the table names
of the related entities are `foo` and `bar`, the junction table name
will be `bar_foo`. If your junction table is named differently, you must
specify this by using an `Opis\ORM\Core\Junction` object.

To better exemplify this, the above example is just a shortcut for the
following:

```php
class Employee extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $junction = new Junction('employees_job_titles', [
            // job_title.id => employess_job_titles.job_title_id
            'id' => 'job_title_id', 
        ]);
        $mapper->relation('job_title')->shareOne(JobTitle::class, null, $junction));
    }
}
class JobTitle extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $junction = new Junction('employees_job_titles', [
            // empoyees.id => employess_job_titles.employee_id
            'id' => 'employee_id',
        ]);
        $mapper->relation('employees')->shareMany(Employee::class, null, $junction);
    }
}
```

Which is also just a shortcut for the following:

```php
class Employee extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $junction = new Junction('employees_job_titles', [
            // job_title.id => employess_job_titles.job_title_id
            'id' => 'job_title_id', 
        ]);
        $foreignKey = new ForeignKey([
            // employees.id => employess_job_titles.employee_id
            'id' => 'employee_id'
        ]);
        $mapper->relation('job_title')->shareOne(JobTitle::class, $foreignKey, $junction));
    }
}
class JobTitle extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $junction = new Junction('employees_job_titles', [
            // empoyees.id => employess_job_titles.employee_id
            'id' => 'employee_id',
        ]);
        $foreignKey = new ForeignKey([
            // job_titles.id => employess_job_titles.job_title_id
            'id' => 'employee_id'
        ]);
        $mapper->relation('employees')->shareMany(Employee::class, $foreignKey, $junction);
    }
}
```

[entity name]: entity-mapping.html#entity-name "Entity name"