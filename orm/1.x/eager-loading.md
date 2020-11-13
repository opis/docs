---
layout: project
library: orm
version: 1.x
title: Eager loading
---

Loading related records may cause sometimes the `N + 1` query problem.
To illustrate this problem, let's consider an `Article` entity and
a `User` entity.

```php
class User extends Entity implements IMappableEntity
{
    /**
     * @return string
     */
    public function name(): string
    {
        return $this->orm()->getColumn('name');
    }
    
    /**
     * @return Articles[]
     */
    public function articles(): array
    {
        return $this->orm()->getRelated('articles');
    }
    
    /**
     * @inheritdoc
     */
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->relation('articles')->hasMany(Article::class);
    }
}

class Article extends Entity implements IMappableEntity
{
    /**
     * @return string
     */
    public function title(): string
    {
        return $this->orm()->getColumn('title');
    }
    
    /**
     * @return User
     */
    public function author(): User
    {
        return $this->orm()->getRelated('author');
    }
    
    /**
     * @inheritdoc
     */
    public static function mapEntity(IEntityMapper $mapper)
    {
        $mapper->relation('author')->belongsTo(User::class);
    }
}
```

Now, let's query for the first 10 articles and display their title and their author's name.

```php
$articles = $orm(Article::class)
                ->orderBy('title')
                ->limit(10)
                ->all();

foreach($articles as $article) {
    echo $article->title(), ' by ', $article->author()->name();
}
```

The above code will execute 1 query to fetch the articles, then for each of the 10 articles
fetched, it will execute another query to fetch its author, resulting in a total of 11 queries
executed. 

We can reduce this operation to just 2 queries by using eager loading. 
Specifying which relationships to be eager loaded is done with the help of the `with` method.

```php
$articles = $orm(Article::class)
                ->with('author')
                ->orderBy('title')
                ->limit(10)
                ->all();

foreach($articles as $article) {
    echo $article->title(), ' by ', $article->author()->name();
}
```