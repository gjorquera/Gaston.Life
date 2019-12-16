---
title: Access Modifiers as Design
category: programming
tags:
- programming
description: >
  The public interface of a class should be treated with as much delicacy as a
  public web service API. The visibility of class' members shouldn't be a
  consequence of how consumers are using the class; it must be deliberately
  designed.
date: 2019-12-14
draft: true
---

Looking at Guava's [`@VisibleForTesting`
annotation](https://github.com/google/guava/blob/master/guava/src/com/google/common/annotations/VisibleForTesting.java#L18-L19)
code we see an interesting comment: "only for use in test code".

Why should we have production code that behaves differently when executed under
tests? What is the problem that this annotation is trying to solve? Why not
declare everything as public instead?

## Why is this a problem?

Maybe the real problem is that we are not using access modifiers properly,
making us think that we need to expose internals only for testing.

Most programming languages have access modifiers with strong semantic meaning.
Using Java as an example, it's not enough to think of access modifiers only as:

|                                 | private | package-private | protected | public  |
| ---                             | ---     | ---             | ---       | ---     |
| Same class                      | **Yes** | **Yes**         | **Yes**   | **Yes** |
| Same package, subclass          | No      | **Yes**         | **Yes**   | **Yes** |
| Same package, non subclass      | No      | **Yes**         | **Yes**   | **Yes** |
| Different package, subclass     | No      | No              | **Yes**   | **Yes** |
| Different package, non subclass | No      | No              | No        | **Yes** |

This table is missing the context of the class' responsibilities, behaviors, and
expected use cases for each keyword.

### Public

The public interface of a class is the class' contract. It's how you tell
consumers how to use it.

Both the [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) and
[Tell, Don't Ask](https://www.martinfowler.com/bliki/TellDontAsk.html)
heuristics are related to this same concept. Design classes so that you don't
need to chain getters (Law of Demeter) because consumers can tell the class to
do things instead of asking for information for consumers to work with (Tell,
Don't Ask).

### Protected

If a class is designed to be extensible through hierarchies and inheritance
(like most of the design patterns we all know well) then the protected keyword
becomes part of public interface.

Designing a class for inheritance is a deliberate choice, just as designing a
class to be used without inheritance. It's so important that the class should
document on how protected methods should be used by subclasses.

### Package Private

Package private, or file level visibility, or module level visibility, or
compilation unit visibility are all similar. They specify that something is an
implementation detail, not intended to be used by consumers.

They allow separation of concerns into classes but without allowing consumers to
use these internal details.

This access modifier enables developers to design internal details but safely
refactor them in the future without breaking consumers and, therefore, without
major version bumps.

### Private

Similar to package private, private methods and instance variables are
implementation details of a class. This is usually the encapsulated data,
exposed through public behaviors.

This access modifier also enables developers to do refactors and improvements
without breaking consumers.

## What should I do?

### Tests can help the design only if you treat tests as the first consumer

Unit tests provide valuable feedback to production code only if production code
has sensible public interfaces that can be called without "test-only" code.

If you need to raise the visibility of carefully designed classes to test it,
then that's a hint that you need a new abstraction with its own public interface
and its own tests.

### Avoid classes with private instance fields and public getters and setters

This is not encapsulation. Semantically, it's the same as having public instance
fields which we all know we shouldn't. You might be missing an abstraction that
encapsulates the data and exposes it through behaviors. The exception are data
classes designed to contain only data and no behavior.

### Avoid sharing code through inheritance and protected methods

This is not right encapsulation. Make instance fields private and design the
classes to be inherited. Reviewing well known object oriented patterns, such as
Template Method Pattern, might help.

### Don't abuse the fact that test and production code shares the same package

Don't use package-private or protected methods to write unit tests. Don't use
@VisibleForTesting on private methods to write unit tests. By avoiding the
public interface of the class, you're not using the class as it's intended to be
used.
