---
layout: project
library: orm
version: 1.x
title: Quick start
---

This is a quick overview of the library and of its features. 
Don't worry if you don't understand every single line of code; 
everything will be explained later in detail.
{:.alert.alert-info}

## The entity manager

The main function of the [entity manager] is to provide methods for creating, fetching,
updating, and deleting an entity. The very first thing you must do, 
in order to be able to use the entity manager, is to define a connection to your 
database. The connection is defined with the help of the `Opis\Database\Connection` 
class, on whom you can find out more [here][0]. 

Once the connection has been defined, it's time to instantiate the entity manager.

```php
use Opis\Database\Connection;
use Opis\ORM\EntityManager;

// Define a database connection
$connection = new Connection("dsn:mysql;dbname=test", "root", "secret");

// Create an entity manager
$orm = new EntityManager($connection);
```

## Defining entities

[Entities][1] are an object-oriented representation of a table row.
They are represented with the help of classes and each instance of such 
a class is a direct mapping to a table's record. Entity classes inherit
from `Opis\ORM\Entity` base class, which provides a single method, named
`orm`, that returns a [data mapper] object. 
The data mapper object has various methods that allow us to interact 
with the entity's records.

Let's consider the following tables

***users** table*

|column|description|
|------|-----------|
|id|integer, primary key, autoincrement|
|name|varchar|
{:.table .table-bordered .table-striped}

***articles** table*

|column|description|
|------|-----------|
|id|integer, primary key, autoincrement|
|user_id|integer, foreign key to *users.id*|
|title|varchar|
|content|text|
{:.table .table-bordered .table-striped}


For each of these tables we'll define an entity: `My\Blog\User` for the *users* table and
`My\Blog\Article` for the *articles* table.

### User entity

This class will contain, at first, only three methods: 
a method for getting the user's ID, a method for setting the name of
the user, and another one for getting its name. As you can see, this is done with the help of 
the `getColumn` and `setColumn` methods.

```php
namespace My\Blog;

use Opis\ORM\Entity;

class User extends Entity
{
    /**
     * Get user's ID
     * @return int
     */
    public function id(): int
    {
        return $this->orm()->getColumn('id');
    }
        
    /**
     * Get user's name
     * @return string
     */
    public function getName(): string
    {
        return $this->orm()->getColumn('name');
    }

    /**
     * Set user's name
     * @param string $name
     * @return User
     */
    public function setName(string $name): self
    {
        $this->orm()->setColumn('name', $name);
        return $this;
    }
}
```

### Article entity

The class for the *Article* entity is pretty much similar with the previous one.

```php
namespace My\Blog;

use Opis\ORM\Entity;

class Article extends Entity
{
    /**
     * Get article's ID
     * @return int
     */
    public function id(): int
    {
        return $this->orm()->getColumn('id');
    }
    
    /**
     * Get article's title
     * @return string
     */
    public function getTitle(): string
    {
        return $this->orm()->getColumn('title');
    }

    /**
     * Get article's content
     * @return string
     */
    public function getContent(): string
    {
        return $this->orm()->getColumn('content');
    }

    /**
     * Set articles's title
     * @param string $title
     * @return Article
     */
    public function setTitle(string $title): self
    {
        $this->orm()->setColumn('title', $title);
        return $this;
    }

    /**
     * Set articles's content
     * @param string $content
     * @return Article
     */
    public function setContent(string $content): self
    {
        $this->orm()->setColumn('content', $content);
        return $this;
    }
}
```

### Mapping entities

As we can see, the `id` method, of both entity classes, returns a value
whose type is `int`. 
The code will work fine as long as `declare(strict_types=1)` is not used.
To prevent any unpleasant errors, in case of a strict typing constraint, we must
specify that the value of the `id` column needs to be casted to an integer before 
it's returned by the `getColumn` method.
We do this by using an [entity mapper]. The most simple way of accessing the entity mapper
for a specify entity class,
is by implementing the `Opis\ORM\IMappableEntity` interface on that entity class.   

```php
namespace My\Blog;

use Opis\ORM\{
    Entity, 
    IEntityMapper,
    IMappableEntity
};

class User extends Entity implements IMappableEntity
{
    // ... other methods here
    
    /**
     * @inheritdoc
     */
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->cast([
            'id' => 'integer'
        ]);
    }
} 
```

The same thing needs to be done for the *Article* entity

```php
namespace My\Blog;

use Opis\ORM\{
    Entity, 
    IEntityMapper,
    IMappableEntity
};

class Article extends Entity implements IMappableEntity
{
    // ... other methods here
    
    /**
     * @inheritdoc
     */
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->cast([
            'id' => 'integer'
        ]);
    }
} 
```

### Relationships

Now that we have access to the entity mapper, is time to set the relationship
between our two entities. As it was shown, the *articles* table has a foreign key,
on a column named `user_id`, that points to the *users* table.
The value of the `user_id` column don't have uniqueness constraints, so the same
value might appear multiple times. In plain words, this means that a user can have multiple
articles. We can express this relationship by using the `relation` and `hasMany` methods.

```php
class User extends Entity implements IMappableEntity
{
    // ... other methods here
    
    /**
     * @inheritdoc
     */
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->cast([
            'id' => 'integer'
        ]);
        
        // A relation named "articles"
        $mapper->relation('articles')->hasMany(Article::class);
    }
} 
``` 

Once the relation has been defined, we could use it inside our class.
This is done by passing the name of the relation to the `getRelated` method.

```php
class User extends Entity implements IMappableEntity
{
    // ... other methods here
    
    /**
     * Get user's articles
     * @return Article[]
     */
    public function getArticles(): array
    {
        return $this->orm()->getRelated('articles');
    }
    
    /**
     * @inheritdoc
     */
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->cast([
            'id' => 'integer'
        ]);
        
        // A relation named "articles"
        $mapper->relation('articles')->hasMany(Article::class);
    }
} 
```

All that remains to do now is to set the relation between the *Article* entity
and the *User* entity. Since every article must belong to a user, we will use the
`belongsTo` method to express this relationship.

```php
class Article extends Entity implements IMappableEntity
{
    // ... other methods here
    
    /**
     * @inheritdoc
     */
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->cast([
            'id' => 'integer'
        ]);
        
        // A relation named "author"
        $mapper->relation('author')->belongsTo(User::class);
    }
} 
```

That's it! Now we can use the `setRelated` and `getRelated` methods for setting
and getting the author of an article.

```php
class Article extends Entity implements IMappableEntity
{
    // ... other methods here
    
    /**
     * Get article's author
     * @return User
     */
    public function getAuthor(): User
    {
        return $this->orm()->getRelated('author');
    }
    
    /**
     * Set article's author
     * @param User $user
     * @return Article
     */
    public function setAuthor(User $user): self
    {
        $this->orm()->setRelated('author', $user);
        return $this;
    }
    
    /**
     * @inheritdoc
     */
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->cast([
            'id' => 'integer'
        ]);
        
        // A relation named "author"
        $mapper->relation('author')->belongsTo(User::class);
    }
} 
```

## Working with entities

Creating a new entity is accomplished by calling the `create` method on the entity manager
instance. The newly created entity will not be persisted into the database until the
`save` method is called.

```php
use Opis\Database\Connection;
use Opis\ORM\EntityManager;
use My\Blog\{User, Article};

// Define a database connection
$connection = new Connection("dsn:mysql;dbname=test", "root", "secret");

// Create an entity manager
$orm = new EntityManager($connection);

// Create a new entity
$user = $orm->create(User::class);
// Set user's name
$user->setName('Admin');
// Persist our entity
$orm->save($user);

// Create article
$article = $orm->create(Article::class);

// Setup article
$article->setTitle('first My article') // oops
        ->setContent("This is my article's content")
        ->setAuthor($user);
        
// Persist our article
$orm->save($article);

// Fix it
$article->setTitle('My first article');
// Update it
$orm->save($article);
```

[0]: /database/4.x/connections.html "Opis Database Connections"
[1]: entities.html "Entities"
[entity manager]: entity-manager.html "Entity manager"
[data mapper]: data-mapper.html "Data mapper"
[entity mapper]: entity-mapping.html#entity-mappers "Entity mappers"
