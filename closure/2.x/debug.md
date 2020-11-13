---
layout: project
library: closure
version: 2.x
title: Debugging closures
description: Learn how to debug serialized closures
canonical: /closure/3.x/debug.html
---

**Opis Closure** is debugging friendly and can be used in conjunction with 
error handlers like [whoops] without any special or supplemental configurations. 

```php
$whoops = new \Whoops\Run;
$whoops->pushHandler(new \Whoops\Handler\PrettyPageHandler);
$whoops->register();

$closure = function(){
    throw new \Exception();
};

$closure = unserialize(serialize(new SerializableClosure($closure)))->getClosure();

$closure();
```

[![Error 1][error1]][error1]

Starting with version 2.0.0, **Opis Closure** allows you to track the residing
source of a serialized closure by using the `#trackme` directive inside the closure. 
The `#trackme` directive provides the following information: 

- `Date` - the date when the closure was serialized, represented in the W3C format
- `Timestamp` - an integer representation of the Date
- `File` - the file where the serialized closure resides
- `Line` - the line where the closure can be located inside the File

```php
$whoops = new \Whoops\Run;
$whoops->pushHandler(new \Whoops\Handler\PrettyPageHandler);
$whoops->register();

$closure = function(){
    #trackme
    throw new \Exception();
};

$closure = unserialize(serialize(new SerializableClosure($closure)))->getClosure();

$closure();
```

[![Error 1][error2]][error2]

[whoops]: https://github.com/filp/whoops "Whoops"
[error1]: /assets/closure/error1.png "Error 1" 
{: .img-fluid }
[error2]: /assets/closure/error2.png "Error 2" 
{: .img-fluid }


