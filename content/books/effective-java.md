---
title: Effective Java
author: Joshua Bloch
categories:
- programming
description: >
  How to write better Java. Has multiple tips and idioms to create saner and
  clearer applications, avoiding that feeling of drowning over all the options
  that Java offers. Recommended for everyone working with Java on a daily basis.
rating: 3
date: 2017-05-13
amazon: https://amzn.to/2Pyod1N
---

> Clarity and simplicity are of paramount importance. The user of a module
> should never be surprised by its behavior. Modules should be as small as
> possible but no smaller. Code should be reused rather than copied. The
> dependencies between modules should be kept to a minimum. — Joshua Bloch

## Creating and Destroying Objects

Consider static factory methods (not Factory Method pattern) for object
instantiation. You can cache instances to control the number of instances, and
return nonpublic subtypes exposing one public API hiding the different
implementations.

Use a static factory method when it's not clear how an object will be
constructed based on the constructor parameters. Use an expressive method name
as a hint about that particular way to instance the object.

Use a static factory method when you need two constructors with the same
signature. Never switch the order of parameters in constructors maintaining the
same parameters, it's error prone.

> Avoid the reflex to provide public constructors without first considering
> static factories. — Joshua Bloch

Prefer builders over telescoping constructors or empty constructor with multiple
setters, when object instantiation needs or will need four or more parameters.

Constructors, static factory methods and builders should create fully
initialized objects. Never build invalid objects with separate initialization
methods to complete them.

If you need a singleton class, the single-element enum type is the best
solution.

Don't forget to include private constructors for utility classes to avoid
instantiation or inheritance. Since you're explicitly adding a constructor to
avoid instantiation, include a comment explicitly saying so.

Remember to remove obsolete references to avoid having unintentional object
retentions which leads to objects not being garbage collected. This might lead
to "memory leaks" producing page swapping and degraded performance. This is
specially important when its you, not the compiler, the one that knows which
part of the data is still referenced but inactive or unimportant.

Use `try`-`finally` blocks instead of writing an object's `finalize()` method to
release resources. It's not guaranteed that the GC will execute the `finalize()`
method promptly enough.

## Methods Common to All Objects

The default `equals` method implements object identity, every instance is only
equal to itself. This is the right thing if each instance is unique, you don't
need to test for equality, or a superclass already implements it appropriately.

Override the `equals` method when the class you're designing has a notion of
equality different from object identity. This usually the case for value
classes.

The `equals` method must implement an equivalence relation. It must be
reflexive, symmetric, transitive and consistent.

Don't make `equals` depend on unreliable resources, such as network services. It
makes it very difficult to maintain consistency.

Always override `hashCode` when you override `equals`.

Always override `toString` and include all interesting information. If you
decide to specify a format (which is useful for value classes), remember to add
the format in the method's documentation.

Implement the `Comparable` interface if your objects has an obvious natural
ordering, such as alphabetical, numerical, or chronological order. It must be
anti-commutative, exception symmetric, and transitive.

## Classes and Interfaces

Clearly separate the public API of an object (what it does) with it's
implementation and internal details (how it does it).

Make each class or method as inaccessible as possible: private, package-private,
protected, or public.

One approach is to design the public API of the interface or class and to keep
it as reduced as possible. Every other method should be private unless it's
designed to be inherited or other package classes needs access to it.

If a package-private class is used only by one class, consider nesting it into
the client as a private class to restrict misuse from other classes in the
package.

Never increase the scope of a helper method above package-private when you need
it for testing purposes.

Instance variables should never be public for public classes, and never blindly
create getters and setters for them. Think if you want to expose your internals
as part of the public API.

You can expose instance variables as public in package-private or private nested
classes, assuming that's what you designed the class for.

Prefer to design immutable classes over mutable classes. Don't provide mutators,
ensure that it can't be subclassed, and make all instance variables private and
final.

Prefer a functional approach when implementing methods that mutates states.
Instead of mutating the internal state of the instance, return a new instance
with the mutated stated as a result.

Beware with consecutive operations that creates several intermediate immutable
objects only to discard them, keeping only the last result. Consider creating a
mutable package-private alternatives to improve the performance of specific
operations. For example, the immutable `String` class has a `StringBuilder`
mutable alternative.

It's usually a good idea to design immutable classes with private or
package-private constructors and a public a static factory method for several
reasons. It cannot be subclassed. The static factory method may return better
implementations in the future without breaking the public API, by returning
subclasses defined in the package. And, it gives you the option to to cache and
reuse instances instead of always creating new ones, which reduces memory
footprint and garbage collection.

Prefer composition over inheritance. Composition depends on the public API of
objects while inheritance depends on the internals of it's superclass, violating
encapsulation.

A useful idiom when using composition is to write a reusable forwarding class
which forwards every method from the interface to it's composed object and
override, in the class that extends this forwarding class, the methods you need
to modify.

Document classes designed for inheritance. Document the expectations on the
different protected methods throughout the class.

Constructors should never call protected methods. These will be called before
the subclass constructor, leading to potential problems if protected methods are
overridden and they depend on the subclass constructor.

Test inheritance by writing subclasses with the different intended use cases.

Use interfaces only to define types, not to export constants.

Use class hierarchies instead of tagging classes. Using a field to determine
what type of class an instance represents is verbose, error prone, and
inefficient.

Use static member classes for nested classes if the nested class doesn't depend
on an instance of the enclosing class.

## Generics

Don't use raw types (generics without an explicit type). Prefer
`Container<Object>` when it can hold objects of any type. Prefer unbounded
types, `Container<?>` when you don't care about the type itself but you need it
to be specific.

Fix all unchecked warnings. If you have to use `@SuppressWarnings("unchecked")`,
use the smallest scope possible, including defining temporary variables to
suppress the warning only on that new variable instead of the whole method.

Prefer `List` over arrays. Don't mix generics with arrays. Generics are
invariant and erased while arrays are covariant and reified.

Design generic classes instead of relying on casts in the public API. Use
bounded type parameters if you need to take advantage of a particular superclass
without using casts.

Design utility classes with generic methods instead of using raw types.

Use recursive type bounds when you need the generic type to be of a certain kind
of a different generic class.

```java
public static <T extends Comparable<T>> T
        max(Iterable<T> elems) { ... }
```

Use bounded wildcard types for increased flexibility when your generic type
needs to work with some subtype, for producer arguments, or supertype, for
consumer arguments, of it's type parameter.

```java
public static <T extends Comparable<? super T>> T
        max(Interable<? extends T> elems) { ... }
```

Don't use wildcard types in return types as it complicates the public API
forcing your users to also use wildcard types. Your users shouldn't have to
think about wildcard types when using your generic classes or methods.

For heterogeneous collections (usually maps), consider using a generic key
instead of a generic type with an wildcard parameter type (bounded or
unbounded). The `Class<T>` is usually helpful as key for any given parameter
type `T`.

## Enums

Enums types are final classes without accessible constructors
(instance-controlled classes) that exports one static final instance per enum
value.

Use enums whenever you need a fixed set of constants. These sets doesn't need to
be fixed forever, new values can be added in future releases.

Prefer constant-specific method implementations to provide different behaviour
per enum value. Use switches over enums only when you don't own the enum type.

If you override the `toString` method of an enum type, consider implementing the
corresponding `fromString` method.

Use instance variables instead of depending on the enum's `ordinal()` values.

Use `EnumSet` instead of bit fields. Use `EnumMap` instead of arrays indexed via
`ordinal()`.

## Annotations

Use annotations instead of naming patterns in classes or methods.

Always use the `@Override` annotation on methods that are intended to override a
method in the superclass.

Prefer marker interfaces if you want to define a type with no new methods
associated with it. Prefer marker annotations if you want to mark elements other
than classes and interfaces, or if there's a possibility that you'll need to add
more information to the marker.

## Methods

Design methods to be as general as possible, deciding to use abstractions that
requires as less restrictions as practically possible.

Write explicit checks on constructors and method parameters when you need
restrictions on their values. Document the exceptions thrown in case a
restriction is not met.

Use assertions to check for parameter restrictions in package-private or private
methods, and make sure all it's uses are valid.

Prefer using immutable components as instance variables over mutable ones. If
you have mutable instance variables, create copies when storing them internally
or returning them to clients. This avoids modifications outside of the class to
modify the internals of the class.

Be careful with overloaded methods. They are resolved at compile time, with the
type known by the compiler at that time. It's very difficult to determine which
overloaded method is called, specially with Generics and autoboxing. Avoid
overloaded methods that do different things; try to implement more specific
methods based on more generic ones.

Method overrides are resolved at runtime, with the type known by the interpreter
at that time.

Avoid designing varargs methods if possible. Use them only if you really need a
method to work on a variable-length sequence of values.

Consider returning empty arrays or collections instead of `null` values.

Document every public API element. Every method should include what it does not
how, it's preconditions and postconditions (they can be specified in the
`@throws` or `@param` tags), any side effects, it's serialized form, and it's
thread safety.

## General Programming

Use `for-each` loops instead of `for` or `while`; it works over any `Iterable`.
Consider that `for-each` loops doesn't work to filter, transform or iterate
collections in parallel.

Prefer creating new types, however trivial, instead of using `String` to store
domain information.

Interface types represent a domain object while concrete classes are particular
implementations of that domain object. If an appropriate interface type exists,
prefer it over concrete implementations.

Avoid using reflection in production applications. Reflection was designed and
works better for builder tools.

Don't forget to make your applications fast, but first design them to be good.
Consider the performance impact of your design decisions, and prefer those that
won't limit your performance.

> Measure performance before and after each attempted optimization. — Joshua
> Bloch

Never forget to measure the performance after you finished building your system.
Write repeatable performance tests so you can execute them as your system
evolves.

## Exceptions

Use exceptions in exceptional conditions. Check the correctness of the object
before performing any action, when possible.

Don't throw, or rely on objects throwing, exceptions for flow control.

Use checked exceptions for exceptional conditions, which you cannot test
beforehand, from which its very safe and easy to recover from.

Don't abuse throwing checked exceptions in your APIs. Checked exceptions makes
it harder to user your API.

Use runtime exceptions for programming errors where it cannot be recovered from.
These usually come from precondition violations. All your exceptions should
extend `RuntimeException`.

Prefer using the standard exceptions rather than creating new ones. Learn about
the different standard exceptions and when to use them.

Think which lower-level exceptions you want to let propagate, as some might not
make sense to clients of your API. Catch those lower-level exceptions and throw
exceptions appropriate to the current abstraction.

Clearly document all the exceptions that your API can throw.

Include relevant information in the exception message regarding the values that
generated an exception.

When there's an exception, try to leave your object in the state that it was
before the exception was created. This is easily achieved if you check for
exceptional conditions before mutating the object.

Don't ignore exceptions unless you have a very good reason. And, if you have it,
don't forget to document on the source code why you're ignoring that particular
exception.

## Concurrency

Use the `synchronized` keyword to declare methods that are mutually exclusive to
threads.

Use `volatile` variables or `synchronized` methods to force communication
between threads. Failing to do so, the VM might do undesired optimizations, such
as hoisting, moving expressions around making a thread unaware of shared
variable updates.

Don't invoke user defined methods from `synchronized` blocks. You don't know
what the user will do and they might end up causing exceptions, data corruption,
or deadlocks.

Always try to do as little as possible inside `synchronized` blocks. Copying
data inside a `synchronized` block just to be sure that it's not going to be
modified outside of it is a valid option to achieve thread safety.

Don't make your objects thread safe unless they are meant to always be used in a
threaded context. Make them thread safe only if you can achieve significant
performance improvements by synchronizing internals rather than having the user
synchronizing the entire object.

Always synchronize modifications of static fields. Your users can't guarantee
that everyone accessing a static field is synchronizing over it. You must
provide that guarantee for them.

Prefer using executors and tasks instead of managing the threads yourself.

Prefer using concurrent utils, such as `ConcurrentHashMap` and `BlockingQueue`,
instead of using lower level thread synchronization primitives, such as `wait`
and `notify`.

Always document if your class is or isn't thread safe. Try to determine if your
classes are immutable, unconditionally thread-safe, conditionally thread-safe,
not thread-safe or thread-hostile.

## Serialization

Beware of the hidden costs of making an object serializable. The biggest cost is
that it becomes part of the public API making it very difficult to modify them
because serialized instances of the object may be already in the wild.

Beware of the default serialized form of objects; it serializes private and
package-private fields making them part of the public API.

Specify the stream unique identifier and make sure your changes remain
compatible with the serialized form.

Always implement the `readObject` method yourself to validate the object being
created. Defensively copy private field references, check invariants, and never
invoke overridable methods.

Write tests to make sure that your changes are backwards compatible, including
deserializing objects serialized with the old version of the object.

Value classes and collections should implement serialization. Classes designed
for inheritance and classes representing active entities shouldn't implement
serialization.

Use serializable enum types if you need to control the instances.

Consider using serialization proxies to serialize instances.
