---
layout: project
library: pattern
version: 1.x
title: Quick start
description: Creating a simple router using Opis Pattern
keywords: opis, pattern, regex, router, route
---

To illustrate the usage of **Opis Pattern** we will create a simple router which
will be able to register route patterns with corresponding callable functions,
and execute the matching route. 

If you are interested in an advanced routing library, check out [Opis Routing](https://www.opis.io/routing).
{:.alert.alert-info}

## Router class

```php
use Opis\Pattern\RegexBuilder;

class Router {
    protected $builder;
    protected $routes = [];

    public function __construct()
    {
        $this->builder = new RegexBuilder();
    }

    public function register(string $route, callable $action): self
    {
        // Generate a regex for the route
        $regex = $this->builder->getRegex($route);

        // Save the regex and the action
        $this->routes[$regex] = $action;

        return $this;
    }

    public function execute(string $path)
    {
        // We reverse the routes order, so the last registered
        // is the first called
        $ordered_routes = array_reverse($this->routes, true);
        
        // Loop through all routes until one is matched
        foreach ($ordered_routes as $regex => $action) {
            if ($this->builder->matches($regex, $path)) {
                // Get the values of placeholders
                $values = $this->builder->getValues($regex, $path);

                // Invoke the action
                $data = $action($path, $values);
                
                // If the action returned false, 
                // we continue to search for another route
                if ($data === false) {
                    continue;
                }
                
                return $data;
            }
        }

        // Nothing matched
        return false;
    }
}
```

### __construct

We are just creating a `Opis\Pattern\RegexBuilder` using the
default [options](options.html),
this means that the [separator symbol](options.html#separator_symbol) is `/` (slash),
the capture mode is [left](options.html#capture_left)
with [trailing allowed](options.html#allow_opt_trail).

**usage**

```php
$router = new Router();
```

### register

This method will assign a callable to a pattern,
by [converting](regexbuilder.html#getregex) the pattern to a regex and storing them
into an internal mapper.

**usage**

```php
$router->register($pattern, function (string $path, array $values) {
    // ...
});
```

### execute

Will search for first regex that matches the path, and invoke
the corresponding callback with the path and [placeholder values](regexbuilder.html#getvalues)
as parameters. If the callback returns `false`, the search for a callback
will continue until something different than `false` is returned.
The search starts from the last registered route.

**usage**

```php
$result = $router->execute($path);
```

## Usage example

Lets start by creating a router that can perform an action to a target,
so the pattern will look like this: `action/target`.

```php
$router = new Router();

// Registering routes

$router->register('greet/friend', function () {
    return "Hi";
});

$router->register('call/mom', function () {
    return "Dial 0123456789";
});

// Invokig

echo $router->execute('greet/friend'); //> Hi
echo $router->execute('call/mom'); //> Dial 0123456789
``` 

We have a call `call` action which currently can phone to
a single person only. Lets use the contact list.

```php
$router->register('call/{person}', function (string $path, array $values) {
    $contacts = [
        'mom' => '0123456789',
        'dad' => '9876543210',
        'bro' => '9999999999',
    ];
    
    $person = $values['person'];
    
    if (!isset($contacts[$person])) {
        // We don't have the person in the contact list
        return false;
    }
    
    return "Dial " . $contacts[$person];
});

// Invokig

echo $router->execute('call/mom'); //> Dial 0123456789
echo $router->execute('call/data'); //> Dial 9876543210
echo $router->execute('call/bro'); //> Dial 9999999999
``` 

Well, we should be able to dial a number directly, without being in contact list,
so a new route can handle that.

```php
$router->register('call/{number=\d+}', function (string $path, array $values) {
    $number = $values['number'];
    
    return "Dial " . $number;
});

// Invoking

echo $router->execute('call/991111'); //> Dial 991111
echo $router->execute('call/111122'); //> Dial 111122

echo $router->execute('call/mom'); //> Dial 0123456789
```

As you can see, we restricted the number placeholder to match only digits.

Now lets handle the `greet` action. We consider the target as being the person's
name.

```php
$router->register('greet/{name}', function (string $path, array $values) {
    return "Hello, " . $values['name'] . '!';
});

// Invokig

echo $router->execute('greet/World'); //> Hello, World!
echo $router->execute('greet/developers'); //> Hello, developers!
``` 

It will be cool if we can use the time of the day to have a personalized
greeting.

```php
$router->register('greet/{name}/{time_of_day}', function (string $path,
 array $values) {
    $map = [
        'morning' => 'Good morning',
        'noon' => 'Good afternoon',
        'evening' => 'Good evening',
        'night' => 'Good night',
    ];
    
    $time = $values['time_of_day'];
    
    if (!isset($map[$time]) {
        return false;
    }
    
    return $map[$time] . ',' . $values['name'] . '!';
});

// Invokig

echo $router->execute('greet/World/morning'); //> Good morning, World!
echo $router->execute('greet/people/evening'); //> Good evening, people!
``` 

If you want to handle all unhandled actions, 
you should register a route right after the router creation.

```php
$router = new Router();

$router->register('{action}/{target?}', function (string $path, array $values) {
    if (isset($values['target']) && $values['target'] !== '') {
        return "I don't know how to " 
                . $values['action'] . " " 
                . $values['target'] . ".";
    }
    
    return "I can't " . $values['action'] . ".";
});


// Rest of the code ...


$router->execute('stop/war'); //> I don't know how to stop war.
$router->execute('sleep'); //> I can't sleep.
```
