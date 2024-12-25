---
layout: project
library: closure
version: 4.x
title: Features
description: A list of library's main features
---

## Adding custom serializers

If you try to add custom serializers to your own classes, the recommended approach is to implement the standard
[`__serialize() / __unserialize()`](https://www.php.net/manual/en/language.oop5.magic.php#object.serialize) 
magic methods. Treat closures and other objects just like they are serializable, we'll do the rest.

In the next example you can safely omit `__serialize` and `__unserialize` methods, the code will still work.
However, we recommend implementing them as a best practice.

```php
use Opis\Closure\Serializer;

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

$serialized = Serializer::serialize(new MyClass(fn() => "it works"));
$object = Serializer::unserialize($serialized);
echo $object->test(); // it works
```

We often have to deal with code that we cannot control because it is from a third party.
Don't worry, you can add custom serializers for any class, or you can overwrite existing serializers,
by using the `setCustom` method. The method receives as parameters the class name, serialization function 
and deserialization function.

**Opis Closure** already provides a generic serialization method for all objects,
you are not required to implement one for every class.
{:.alert.alert-info data-title="Remember"}

```php
use Opis\Closure\Serializer;

use Some\Vendor\ExternalClass;

Serializer::setCustom(
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
 * @param Opis\Closure\SerializationHandler $handler The serialization handler (optional, advanced)
 * @return array The serialized data
 */
function (object $object, Opis\Closure\SerializationHandler $handler): array;
// note: you are free to replace object (first parameter) with the actual class name for type hinting
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
 * @param Opis\Closure\DeserializationHandler $handler The deserialization handler (optional, advanced)
 * @return object The deserialized object
 */
function (array &$data, callable $solve, ReflectionClass $class, Opis\Closure\DeserializationHandler $handler): object;
// note: you are free to replace object (return type) with the actual class name for type hinting
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
- [enum](https://www.php.net/manual/en/language.types.enumerations.php) (cannot be overwritten)
- [stdClass](https://www.php.net/manual/en/class.stdclass.php) (cannot be overwritten)
- [Closure](https://www.php.net/manual/en/class.closure.php) (cannot be overwritten)
- [ArrayObject](https://www.php.net/manual/en/class.arrayobject.php)
- [SplObjectStorage](https://www.php.net/manual/en/class.splobjectstorage.php)  
- [SplFixedArray](https://www.php.net/manual/en/class.splfixedarray.php)
- [SplDoublyLinkedList](https://www.php.net/manual/en/class.spldoublyinkedlist.php)   
- [SplStack](https://www.php.net/manual/en/class.splstack.php)   
- [SplQueue](https://www.php.net/manual/en/class.splqueue.php)
- [SplHeap](https://www.php.net/manual/en/class.splheap.php)  
- [SplMaxHeap](https://www.php.net/manual/en/class.splmaxheap.php)  
- [SplMinHeap](https://www.php.net/manual/en/class.splminheap.php)  
- [SplPriorityQueue](https://www.php.net/manual/en/class.splpriorityqueue.php)  
- [WeakMap](https://www.php.net/manual/en/class.weakmap.php) 
- [WeakReference](https://www.php.net/manual/en/class.weakreference.php)


The following internal classes are supported and don't require custom serializers:
- [DateTime](https://www.php.net/manual/en/class.datetime.php)
- [DateTimeImmutable](https://www.php.net/manual/en/class.datetimeimmutable.php)
- [DateTimeZone](https://www.php.net/manual/en/class.datetimezone.php)
- [DateInterval](https://www.php.net/manual/en/class.dateinterval.php)
- [DatePeriod](https://www.php.net/manual/en/class.dateperiod.php)

## Boxing

When **Opis Closure** serializes an object it uses a box where it puts necessary info such as class name and data.
At deserialization the data is unboxed and the object reconstructed. This may add a little overhead for some classes
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

If you own the class, you can add the `NoBox` attribute.

```php
use Opis\Closure\Attribute\NoBox;

#[NoBox]
class NumberPair {
 // ...
}
```

The other method involves using the `noBox` method, and can be used on any class.

```php
use Opis\Closure\Serializer;
use Some\Vendor\SomeClass;

Serializer::noBox(SomeClass::class);
```

All objects that directly or indirectly have references to a closure or any boxed objects,
must also be boxed.
{:.alert.alert-warning data-title="Important"}

Below is an example of class that must be boxed because the array might contain an object that 
needs boxing (such as closure).

```php
class MustBeBoxed {
    public function __construct(
        public array $arbitraryItems
    ) {}
}
```