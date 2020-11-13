---
layout: project
library: events
version: 5.x
title: EventDispatcher class
description: The event dispatcher's role is to registers event listeners and to dispatch events.
keywords: opis, event, target, dispatcher, listener, serializable, class, methods
---

An event dispatcher is in charge of registering event listeners and dispatching events to them.

## Methods

### handle

Registers an event listener and returns an `\Opis\Routing\Route` object.

**signature**

```php
/**
 * @param string $event A pattern for event name
 * @param callable $callback Invoked callback
 * @param int $priority An optional priority
 * @return \Opis\Routing\Route;
 */
public function handle(string $event, callable $callback, int $priority = 0): Route
```

**usage**

```php
use Opis\Events\{
    Event,
    EventDispatcher
};

$dispatcher = new EventDispatcher();

$route = $target->handle("system.{action}", function (Event $event) {
    // Do something ...
}, 1);

$route->where("action", "start|stop");
// ...
```

For more info about routes you can check the
[Opis Routing Framework](https://www.opis.io/routing).

### dispatch

Dispatches an event (can be a [custom event](custom-event.html)).

**signature**

```php
/**
 * @param \Opis\Events\Event $event
 * @return \Opis\Events\Event The dispatched event
 */
public function dispatch(Event $event): Event
```

 **usage**

```php
use Opis\Events\{
 Event,
 EventDispatcher
};

$dispatcher = new EventDispatcher();

$event = new Event("system.update", true);

$dispatcher->dispatch($event);
```

### emit

This method acts as a shortcut for dispatching [standard events](event.html). 

**signature**

```php
/**
 * @param string $name Event name
 * @param bool $cancellable Optionally, mark the event as cancellable
 * @return \Opis\Events\Event
 */
public function emit(string $name, bool $cancellable = false): Event
```

**usage**

```php
use Opis\Events\EventDispatcher;

$dispatcher = new EventDispatcher();

$event = $dispatcher->emit("system.update", true);
```

## Serialization

An `Opis\Events\EventTarget` object is fully serializable
thanks to [Opis Closure](https://www.opis.io/closure).

```php
use Opis\Events\{
 Event,
 EventDispatcher
};

$dispatcher = new EventDispatcher();

$dispatcher->handle("system.start", function () {
  echo "System started";
});

$dispatcher->handle("system.stop", function () {
  echo "System stopped";
});


$dispatcher = unserialize(serialize($target));

$dispatcher->emit("system.start");
$dispatcher->emit("system.stop");

// >
// System started
// System stopped
```
