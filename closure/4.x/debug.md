---
layout: project
library: closure
version: 4.x
title: Debugging closures
description: Learn how to debug serialized closures
---

**Opis Closure** is debugging friendly and can be used in conjunction with 
error handlers like [whoops] without any special or supplemental configurations. 

```php
use function Opis\Closure\{serialize, unserialize};

// register whoops
$whoops = new \Whoops\Run();
$whoops->pushHandler(new \Whoops\Handler\PrettyPageHandler());
$whoops->register();

$closure = function() {
    throw new \Exception("You should catch me");
};

// Unserialize a serialized closure
$closure = unserialize(serialize($closure));

// Call the closure to throw the exception
$closure();
```

[![Error 1][error1]][error1]

The same works for anonymous classes.

```php
use function Opis\Closure\{serialize, unserialize};

// register whoops
$whoops = new \Whoops\Run();
$whoops->pushHandler(new \Whoops\Handler\PrettyPageHandler());
$whoops->register();

$anonymous = new class("bye") {
    public function __construct(private string $message)
    {
    }

    public function getMessage(): string
    {
        if ($this->message !== "hi") {
            throw new RuntimeException("You should say hi");
        }
        return $this->message;
    }
};

$object = unserialize(serialize($anonymous));

echo $object->getMessage();
```

[![Error 3][error3]][error3]

**Opis Closure** allows you to track the residing
source of a serialized closure by using the `#trackme` directive inside the closure. 
The `#trackme` directive provides the following information: 

- `Date` - the datetime when the closure was serialized, represented in the W3C format
- `File` - the file where the serialized closure resides
- `Line` - the line where the closure can be located inside the File

```php
use function Opis\Closure\{serialize, unserialize};

$whoops = new \Whoops\Run();
$whoops->pushHandler(new \Whoops\Handler\PrettyPageHandler());
$whoops->register();

$closure = function() {
    #trackme
    throw new \Exception("You should catch me");
};


$closure = unserialize(serialize($closure));

$closure();
```

[![Error 2][error2]][error2]

[whoops]: https://github.com/filp/whoops "Whoops"
{:rel="nofollow" target="_blank"}
[error1]: /closure/assets/error1_4.png "Debug serialized closure" 
{: .img-fluid }
[error2]: /closure/assets/error2_4.png "Debug with #trackme directive" 
{: .img-fluid }
[error3]: /closure/assets/error3_4.png "Debug serialized anonymous class"
{: .img-fluid }
