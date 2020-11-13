---
layout: project
library: container
version: 3.x
title: How to use it
description: Learn how to use the container
---

Creating a new dependency injection container is simply a matter of creating a new object of type
`Opis\Container\Container`. The constructor of this class takes no arguments.

```php
use Opis\Container\Container;

$container = new Container();
```

Once the container was created, you could start using it and instantiate new objects with the help of the `make`
method.

```php
use Opis\Container\Container;
use Foo\Bar;

$container = new Container();

// New instance of Foo\Bar
$bar = $container->make(Bar::class);
```

You can also instantiate objects by invoking the container itself.

```php
// New instance of Foo\Bar
$bar = $container(Bar::class);
```

## Binding types

Binding an abstract type to a concrete type is done by using the `bind` method. 
The method takes two string arguments: the first one represents the abstract type, 
and the second one represents the concrete type.

```php
use Opis\Container\Container;

interface GreetingInterface 
{
    public function greet(): string;
}

class Greeting implements GreetingInterface
{
    private $text;
    
    public function __construct(string $text = 'Hi')
    {
        $this->text = $text;
    }
    
    public function greet(): string
    {
        return $this->text;
    }
}

$container = new Container();

$container->bind(GreetingInterface::class, Greeting::class);

echo $container(GreetingInterface::class)->greet();//> Hi
```

When binding types, you can also specify arguments that will be passed
to the constructor of the concrete type.

```php
$container->bind(GreetingInterface::class, Greeting::class, ['Hello']);

echo $container(GreetingInterface::class)->greet(); //> Hello
```

Alternatively, you can pass as the second argument to the `bind` method 
an anonymous callback function that will be used to resolve the abstract type. 

```php
$container->bind(GreetingInterface::class, function(){
    return new Greeting("Good day");
});

echo $container(GreetingInterface::class)->greet(); //Good day
```

The callback receive as arguments the container instance itself and an array
of values that were passed to the `bind` method to be used as arguments for the concrete type.

```php
$container->bind(Foo::class, function(Container $container, array $arguments){
    //
}, ['foo', 'bar']);
```

If the second argument for the `bind` method is omitted, 
or `null` is passed as argument's value,  then the type passed as the first 
argument will be both abstract and concrete type.

```php
$container->bind(Foo::class);
// equivalent of
$container->bind(Foo:class, Foo::class);

$container->bind(Bar::class, null, ["baz"]);
// equivalent of
$container->bind(Bar::class, Bar::class, ["baz"]);
```

### Singletones

Occasionally is desirable that some types to be resolved once, and the same object to be returned 
on subsequent calls into the container. To achieve this behavior, simply bind the abstract type 
using the `singleton` method. This method takes same arguments as the `bind` method.

```php
use Opis\Container\Container;

class Counter
{
    protected $count = 0;
    
    public function increment(): int
    {
        return $this->count++;
    }
}

$container = new Container();

$container->singleton(Counter::class);

echo $container(Counter::class)->increment() //> 0;
echo $container(Counter::class)->increment() //> 1;
echo $container(Counter::class)->increment() //> 2;
```

## Dependency injection

The container handles dependency injection automatically. Just build an instance of a class by using the
`make` method and it will just work.

```php
class User
{
    private $name;
    private $greeting;
    
    function __construct(GreetingInterface $greeting, string $name = 'Joe')
    {
        $this->greeting = $greeting;
        $this->name = $name;
    }
    
    public function saySomething(): string
    {
        return $this->greeting->greet() . ', my name is '. $this->name;
    }
}

echo $container(User::class)->saySomething(); // Hi, my name is Joe
```

You can specify a custom value for `$name` by using either the `bind` method or the `singleton` method.

```php
$container->bind(User::class, null, [
    // 0 => Automatically resolved by the container
    1 => 'Sam', // $name value
]);

echo $container(User::class)->saySomething(); // Hi, my name is Sam
```

## Extend types

Sometimes, the dependencies of a class type are not passed through its constructor, but through one or
more class methods. We can set these dependencies by using the `extend` method.

```php
class User
{
    private $greeting;
    private $name;
    
    public function __construct(string $name = 'Joe')
    {
        $this->name = $name;
    }
    
    public function saySomething(): string
    {
        return $this->greeting->greet() . ', my name is '. $this->name;
    }
    
    public function setGreeting(GreetingInterface $greeting)
    {
        $this->$greeting = $greeting;
    }
}

$container->extend(User::class, function(User $user, Container $container){
    $user->setGreeting($container(GreetingInterface::class));
});

echo $container(User::class)->saySomething(); // Hi, my name is Joe
```

We can also use the `extend` method if we want to replace the instance of a class with another object that inherits
from the same class.

```php
class Foo
{
    public function text(): string
    {
        return 'bar';
    }
}

class MyFoo extends Foo
{
    private $instance;
    
    public function __construct(Foo $instance)
    {
        $this->instance = $instance;
    }
    
    public function text(): string
    {
        return strtoupper(parent::text());
    }
}

$container->bind(Foo::class)
          ->extend(Foo:class, function(Foo $instance){
              return new MyFoo($instance);
          });
          
echo $container(Foo::class)->text(); // BAR
```

## PSR Containers

The container provides support for PSR-11 by implementing the `Psr\Container\ContainerInterface`. This interface
defines two methods: `get` and `has`. Both methods take as their single argument a string that is an opaque identifier
for a type. Mapping an identifier to a type is done by using the `alias` method;

```php
$container->alias('foo', GreetingInterface::class);

if ($container->has('foo')) {
    echo $container->get('foo')->greet(); // Hi
}
```
