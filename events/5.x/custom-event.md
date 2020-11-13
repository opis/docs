---
layout: project
library: events
version: 5.x
title: Custom events
description: Creating and dispatching custom events using Opis Events
keywords: opis, event, custom, dispatch
---

If an [event][0] must carry additional data, or must have a custom behavior,
 you should extend the `Opis\Events\Event` class to add the functionality.

The next example describes a custom event which can hold some arbitrary data.

```php
use Opis\Events\Event;

class DataEvent extends Event 
{
    protected $data;
    
    public function __construct(string $name, $data, bool $cancelable = false) 
    {
        $this->data = $data;
        parent::construct($name, $cancellable);
    }
    
    public function data()
    {
        return $this->data;
    }
}
```

Custom events can be fired only by using the [dispatch][1]
method of the [event dispatcher][2] object.

```php
use Opis\Events\EventDispatcher;

$dispatcher = new EventDispatcher();

$dispatcher->handle('data.print', function (DataEvent $event) {
    print $event->data();
});

$event = new DataEvent("data.print", "some text");

$dispatcher->dispatch($event);

//> some text
```

[0]: event.html
[1]: event-dispatcher.html#dispatch
[2]: event-dispatcher.html