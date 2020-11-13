---
layout: project
library: events
version: 5.x
title: How to use
description: Creating and dispatching events to listeners using Opis Events.
keywords: opis, event, target, dispatcher, listener
---

## Dispatching events

Event dispatching is a mechanism that is used to inform a a listener, or a group of listeners, 
that something happened or is about to happen. 

Dispatching an event is done by calling the `dispatch` method of an event dispatcher, and passing
an event object as an argument.

```php
use Opis\Events\{
    Event,
    EventDispatcher
};

$dispatcher = new EventDispatcher();

$dispatcher->dispatch(new Event('some.event'));

```

An event object is an instance of `Opis\Events\Event` class and its constructor takes as
arguments a name for the event and, optionally, a boolean value that tells if the event is cancelable or not.

```php
$event = new Event("event.name", true);
```

An event can also be dispatched by using the `emit` method

```php
use Opis\Events\{
    Event,
    EventDispatcher
};

$dispatcher = new EventDispatcher();

$dispatcher->emit('event.name', true);
// Shortcut for
$dispatcher->dispatch(new Event('event.name', true));
```

## Registering listeners

In order to intercept an event, you have to register a listener by associating a callback
with an event name. This is done by using the `handle` method.

```php
use Opis\Events\{
    Event,
    EventDispatcher
};

$dispatcher = new EventDispatcher();

// Registering listeners

$dispatcher->handle("system.boot", function (Event $event) {
    echo "The system is now started";
});

$dispatcher->handle("system.stop", function (Event $event) {
    echo "The system will shotdown";
});

// Dispatching events

$dispatcher->emit("system.boot");
$dispatcher->emit("system.stop");

// >
// The system is now started
// The system will shotdown
 
```

## Wildcards

Events are identified by a name and can be "namespaced" by using a `.` (dot) symbol as separator.
This way you can group events according to a context.

**Examples**

- user related events: `user.created`, `user.updated`, `user.signout`, ...
- system related events: `system.boot`, `system.stop`, `system.update`, ...
- detailed system events: `system.will.boot`, `system.was.booted`, `system.will.stop`, `system.was.stopped`, ...

The library allows us to use wildcards 
if we want to handle multiple events with one listener.

```php
use Opis\Events\{
    Event,
    EventDispatcher
};

$dispatcher = new EventDispatcher();

// Registering listeners

$dispatcher->handle("system.{action}", function (Event $event) {
    echo "Received a system event: ", $event->name();
});

// Dispatching events

$dispatcher->emit("system.boot");
$dispatcher->emit("system.stop");

// >
// Received a system event: system.boot
// Received a system event: system.stop
``` 

## Filters

You can filter which events to intercept by using the `whereIn` method

```php
use Opis\Events\{
    Event,
    EventDispatcher
};

$dispatcher = new EventDispatcher();

// Registering listeners

$dispatcher->handle("system.{action}", function (Event $event) {
        echo "Received a system event: ", $event->name();
    })
    ->whereIn('action', ['boot', 'update']);

// Dispatching events

$dispatcher->emit("system.boot");
$dispatcher->emit("system.prepare");
$dispatcher->emit("system.update");
$dispatcher->emit("system.stop");

// >
// Received a system event: system.boot
// Received a system event: system.update
``` 

You can do the same filtering by adding a regular expression constraint,
with the help of the `where` method.

```php
use Opis\Events\{
    Event,
    EventDispatcher
};

$dispatcher = new EventDispatcher();

// Registering listeners

$dispatcher->handle("system.{action}", function (Event $event) {
        echo "Received a system event: ", $event->name();
    })
    ->where('action', 'boot|update');

// Dispatching events

$dispatcher->emit("system.boot");
$dispatcher->emit("system.prepare");
$dispatcher->emit("system.update");
$dispatcher->emit("system.stop");

// >
// Received a system event: system.boot
// Received a system event: system.update
``` 

Another way to filter events is by using an inline regex expression constraint.

```php
use Opis\Events\{
    Event,
    EventDispatcher
};

$dispatcher = new EventDispatcher();

// Registering listeners

$dispatcher->handle("system.{=boot|updated}", function (Event $event) {
    echo "Received a system event: ", $event->name();
});

// Dispatching events

$dispatcher->emit("system.boot");
$dispatcher->emit("system.prepare");
$dispatcher->emit("system.update");
$dispatcher->emit("system.stop");

// >
// Received a system event: system.boot
// Received a system event: system.update
``` 

If you want to know all about the patterns check out [Opis Pattern](https://www.opis.io/pattern).
*The separator used for pattern is `.` and capture mode is left with no trailing allowed.*

## Priorities

Usually multiple listeners will be registered for the same event,
and the order in which they are invoked depends on the priority.
When registering a listener you can also pass a priority as integer,
the higher the number is the earlier the listener will be invoked.
If two listeners are registered with the same priority, they will be
invoked in the same order in which they were registered.
Default priority is `0`.

```php
use Opis\Events\{
    Event,
    EventDispatcher
};

$dispatcher = new EventDispatcher();

// Registering listeners

$dispatcher->handle("event", function () {
    echo "Registered first"
});

$dispatcher->handle("event", function () {
    echo "Registered second, but with a higher priority"
}, 1);

$dispatcher->handle("event", function () {
    echo "Registered last"
});


// Dispatching events

$dispatcher->emit("event");

// >
// Registered second, but with a higher priority
// Registered first
// Registered last
``` 

## Cancellation

Sometimes you just want to have the last word and prevent other
listeners to be invoked. You can do this with ease by using the `stop` method. 
This method works only if the dispatched event was instantiated as a cancellable event.
As you'll see, the priority is important.

```php
use Opis\Events\{
    Event,
    EventDispatcher
};

$dispatcher = new EventDispatcher();

// Registering listeners

$dispatcher->handle("event", function () {
    echo "I'll never be called"
});

$dispatcher->handle("event", function (Event $event) {
    // Stop the propagation
    $event->stop();
    echo "I rule"
}, 1); // Set a higher priority

$dispatcher->handle("event", function () {
    echo "I'll never be called too"
});


// Dispatch cancellable event

$dispatcher->emit("event", true);

// >
// I rule
```
