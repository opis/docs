---
layout: project
library: events
version: 5.x
title: Events
description: Opis Event description and class methods
keywords: opis, event, class, methods
---

`Opis\Events\Event` is the standard event class provided by **Opis Events**. 

## Creating events

To create a new event you have to create an instance of `Opis\Events\Event`.

```php
use Opis\Events\Event;

// Cancellable event
$event1 = new Event("event.name.1", true);

// Non-cancellable event
$event2 = new Event("event.name.2", false);
```

If an event is not cancellable, the [stop method](#stop) will have no effect
when called.

When a event is cancelled, other listeners will not be invoked.

Please note that NOT all events should be cancellable.
{:.alert.alert-info}


In order to fire (dispatch) the event, you have to pass it to the [dispatch][1]
method of the [event dispatcher][0]

You can also fire a new basic event using [emit][2], in this
case the emit method itself will create and dispatch a standard event object.
{:.alert.alert-info}

## Methods


### name

Returns the name of the event.

**signature**

```php
public function name(): string
```

**usage**

```php
if ($event->name() === 'system.init') {
    // ...
}
```

### cancelled

Checks if the event was cancelled using the [stop](#stop) method.

**signature**

```php
public function cancelled(): bool
```

**usage**

```php
if ($event->cancelled()) {
    // ...
}
```

### stop

Cancels the propagation of the event, if the event is cancellable.

**signature**

```php
public function stop(): void
```

**usage**

```php
$event->stop();
```

[0]: event-dispatcher.html
[1]: event-dispatcher.html#dispatch
[2]: event-dispatcher.html#emit
