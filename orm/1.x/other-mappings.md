---
layout: project
library: orm
version: 1.x
title: Other mappings
---

## Timestamps

**Opis ORM** allows you to automatically set timestamps to a row by using the
`useTimestamps` method. In order for this method to work, the table must
have a column, named `created_at`, and a another column that allows `null` values, 
named `updated_at`. The type of these columns must be set as being 
`date` and `?date`, respectively.

```php
class Article extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->cast([
            'created_at' => 'date',
            'updated_at' => '?date'
        ]);
        
        $mapper->useTimestamp();
    }
}
```

Now, every time you create and persist a new entity, the `created_at` column is automatically
filled, and every time you update an existing entity the `updated_at` column is
automatically filled for you.

```php
$article = $orm(Article::class)->create();
// Do something with this entity
// ...
// Save it
$orm->save($article); 
// Change something
$article->setTitle('Updated');
// Persist chnages
$orm->save($article);
```

## Soft deletion

Soft deletion is technique used to mark a row as being deleted, without actually deleting it.
In order to be able to use soft deletes, we must use the `useSoftDelete` method.
Having a column named `deleted_at`, that accepts `?date` values, is also mandatory.

```php
class Article extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->cast([
            'deleted_at' => '?date'
        ]);
        
        $mapper->useSoftDelete();
    }
}
```

Now, evey time you delete an entity that uses soft deletion, instead of being 
permanently removed, it will only be marked as deleted.

```php
$article = $orm(Article::class)->find(123);
// Soft deleted
$orm->delete($article);

// Soft delete unpublished articles
$orm(Article::class)
    ->where('published')->is(false)
    ->delete();
```

Of course, you can force a permanently deletion of an entity

```php
$article = $orm(Article::class)->find(123);
// Permanently deleted
$orm->delete($article, true);

// Permanently delete unpublished articles
$orm(Article::class)
    ->where('published')->is(false)
    ->delete(true);
```

## Filters

You can add predefined filters that can be used when querying your entities.
Adding such a filter is done by using the `filter` method. The callback passed
to this method will receive as its first argument an instance of the `Opis\ORM\Core\Query`
class. The second argument is optional and can be used to help building more dynamic queries.

```php
class Article extends Entity implements IMappableEntity
{
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->filter('published', function(Query $query, bool $data = true){
            $query->where('published')->is($data);
        });
        
        $mapper->filter('author', function(Query $query, User $author){
            $query->where('user_id')->is($author->id());
        });
    }
}

$publishedArticles = $orm(Article::class)
                        ->filter('published')
                        ->all();
                        
$unpublishedArticles = $orm(Article::class)
                        ->filter(['published' => false])
                        ->all();
```

Of course, you may apply as many filters as you want.

```php
$articles = $orm(Article::class)
                ->filter(['published', 'author' => $user)
                ->limit(10)
                ->all();
```




