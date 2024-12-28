---
layout: project
library: closure
version: 4.x
title: Features
description: A list of library's main features
---

## Adding custom serializers

If you try to add custom serializers to your own classes, the recommended approach is to implement the standard
[`__serialize() / __unserialize()`](https://www.php.net/manual/en/language.oop5.magic.php#object.serialize){:rel="nofollow" target="_blank"}
magic methods. Treat closures and other objects just like they are serializable, we'll do the rest.

In the next example you can safely omit `__serialize` and `__unserialize` methods, the code will still work.
However, we recommend implementing them as a best practice.

```php
use function Opis\Closure\{serialize, unserialize};

class MyClass {
   private Closure $callback;
   
   public function __construct(Closure $fn) {
        $this->callback = $fn;
   }
   
   public function test() {
        $fn = $this->callback;
        return $fn(); 
   }
   
   public function __serialize() : array {
        return ["callback" => $this->callback];
   }
   
   public function __unserialize(array $data) : void {
        $this->callback = $data["callback"];
   }
}

$serialized = serialize(new MyClass(fn() => "it works"));
$object = unserialize($serialized);
echo $object->test(); // it works
```

We often have to deal with code that we cannot control because it is from a third party.
Don't worry, you can add custom serializers for any class, or you can overwrite existing serializers,
by using the `register()` function. The function receives as parameters the class name, 
the serialization function and the deserialization function.

**Opis Closure** already provides a generic serialization method for all objects,
you are not required to implement one for every class. Here's the order we use: 
1. registered custom serializer using `register()`
2. magic methods inside class [`__serialize() / __unserialize()`](https://www.php.net/manual/en/language.oop5.magic.php#object.serialize){:rel="nofollow" target="_blank"}
3. fallback to a builtin generic object serialization / deserialization


```php
use Some\Vendor\ExternalClass;
use function Opis\Closure\register;

register(
    ExternalClass::class,
    static function (ExternalClass $object): array {
        // we must always return an array from a serializer
        return [
            "a" => $object->getA(),
            "b" => $object->getB(),
        ];
    },
    static function (array &$data, callable $solve): ExternalClass {
        $object = new ExternalClass();
        // as soon as we have our object we have to solve
        // self references from data
        $solve($object, $data);
        // set the required properties
        $object->setA($data["a"]);
        $object->setB($data["b"]);
        // return the deserialized object
        return $object;
    },
);
```

### Serialization function

The serialization function receives an object as input and must return an array of serializable data. 
It has the following signature:

```php
/**
 * @param object $object The object that must be serialized
 * @return array The serialized data
 */
function (object $object): array;
// note: you are free to replace object (first parameter)
// with the actual class name for type hinting
```

### Deserialization function

Unlike serialization, the deserialization of objects requires a little more code in order to make it work
with self references or circular references.
The function has the following signature:

```php
/**
 * @param array $data A reference data that comes from the serialization function
 * @param callable $solve A callback used to solve references, must be invoked right after we have the object 
 *                        and before we set any properties to it (optional, but recommended)
 * @param ReflectionClass $class Class reflection, useful when this function is generic (optional)
 * @return object The deserialized object
 */
function (array &$data, callable $solve, ReflectionClass $class): object;
// note: you are free to replace object (return type)
// with the actual class name for type hinting
```

The deserialization function receives as input a reference to the serialized array and a callback used to 
solve references. The idea is to invoke the callback as soon as we have the object and before any possible self-reference 
properties are set to it. We also send the data where to solve the references for the object.

The solve callback has the following signature:

```php
/**
 * @param object|null $object The object that was built for deserialization.
 * @param mixed $value Usually the data that might contain references to the object (optional) 
 * @return void
 */
function(?object $object, mixed &$value = null): void;
```

## Default object serializers

By default, we have custom serializers for the following PHP classes: 
- [enum](https://www.php.net/manual/en/language.types.enumerations.php){:rel="nofollow" target="_blank"} (cannot be overwritten)
- [stdClass](https://www.php.net/manual/en/class.stdclass.php){:rel="nofollow" target="_blank"} (cannot be overwritten)
- [Closure](https://www.php.net/manual/en/class.closure.php){:rel="nofollow" target="_blank"} (cannot be overwritten)
- [ArrayObject](https://www.php.net/manual/en/class.arrayobject.php){:rel="nofollow" target="_blank"}
- [SplObjectStorage](https://www.php.net/manual/en/class.splobjectstorage.php){:rel="nofollow" target="_blank"}
- [SplFixedArray](https://www.php.net/manual/en/class.splfixedarray.php){:rel="nofollow" target="_blank"}
- [SplDoublyLinkedList](https://www.php.net/manual/en/class.spldoublyinkedlist.php){:rel="nofollow" target="_blank"}
- [SplStack](https://www.php.net/manual/en/class.splstack.php){:rel="nofollow" target="_blank"}
- [SplQueue](https://www.php.net/manual/en/class.splqueue.php){:rel="nofollow" target="_blank"}
- [SplHeap](https://www.php.net/manual/en/class.splheap.php){:rel="nofollow" target="_blank"}
- [SplMaxHeap](https://www.php.net/manual/en/class.splmaxheap.php){:rel="nofollow" target="_blank"}
- [SplMinHeap](https://www.php.net/manual/en/class.splminheap.php){:rel="nofollow" target="_blank"}
- [SplPriorityQueue](https://www.php.net/manual/en/class.splpriorityqueue.php){:rel="nofollow" target="_blank"}
- [WeakMap](https://www.php.net/manual/en/class.weakmap.php){:rel="nofollow" target="_blank"}
- [WeakReference](https://www.php.net/manual/en/class.weakreference.php){:rel="nofollow" target="_blank"}


The following internal classes are supported and don't require custom serializers:
- [DateTime](https://www.php.net/manual/en/class.datetime.php){:rel="nofollow" target="_blank"}
- [DateTimeImmutable](https://www.php.net/manual/en/class.datetimeimmutable.php){:rel="nofollow" target="_blank"}
- [DateTimeZone](https://www.php.net/manual/en/class.datetimezone.php){:rel="nofollow" target="_blank"}
- [DateInterval](https://www.php.net/manual/en/class.dateinterval.php){:rel="nofollow" target="_blank"}
- [DatePeriod](https://www.php.net/manual/en/class.dateperiod.php){:rel="nofollow" target="_blank"}


## Object boxing

When **Opis Closure** serializes an object it uses a box where it puts necessary info such as class name and data.
At deserialization the data is unboxed and the object reconstructed. This may add a little overhead for some objects
that don't have references to closures or any other data that has a custom serializer.

Here is an example

```php
class NumberPair {
    public function __construct(
        public int $a, 
        public int $b
    ) {}
    
    public function __serialize() : array{
        return [$this->a, $this->b];
    }
    
    public function __unserialize(array $data) : void{
        [$this->a, $this->b] = $data;
    }
}
```

The above class always uses only integers in the serialized data array, so in this case the boxing can be avoided.

If you own the class, you should add the `PreventBoxing` attribute.

```php
use Opis\Closure\Attribute\PreventBoxing;

#[PreventBoxing]
class NumberPair {
 // ...
}
```

The other method involves using the `prevent_boxing()` function, and can be used on any class.

```php
use Some\Vendor\SomeClass;
use Other\Vendor\SomeOtherClass;
use function Opis\Closure\prevent_boxing;

prevent_boxing(SomeClass::class, SomeOtherClass::class, ...);
```

All objects that directly or indirectly have references to a closure or any boxed objects,
**must not** prevent boxing.
{:.alert.alert-warning data-title="Important"}

Below is an example of class that must not prevent boxing because the array might contain an object that 
is boxed (such as closure).

```php
class MustBeBoxed {
    public function __construct(
        public array $arbitraryItems
    ) {}
}
```

By default, all non-internal objects are boxed. When an object has boxing prevented, **Opis Closure** will not know about
it's serialized data.