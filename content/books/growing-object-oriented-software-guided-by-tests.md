---
title: Growing Object Oriented Software, Guided by Tests
author: Steve Freeman and Nat Pryce
category: programming
description: >
  Using a simple language, clear ideas, and concrete examples, it builds a
  wealth of knowledge. It's very deep, touching hard topics of software
  maintainability, with real life examples. It'll get you to the next level.
rating: 5
date: 2016-08-30
amazon: https://amzn.to/2Nmb617
---

Unless performance is a requirement, optimize for readability. Code is read far
more than it's written.

Team productivity is mostly improved by a readable codebase.

External quality measures the level on which the system meets its customers and
users needs. Internal quality measures the level on which the system meets its
developers and administrator needs.

When developing always consider the external quality trade-offs as well as
internal quality trade-offs. Never forget about the internal quality.

> Code isn't sacred just because it exists. — Steve Freeman and Nat Pryce

The best way to cope with system complexity is to constantly improve the
internal quality.

Code that works is not the same as finished code. Always consider that the first
version might have everything in place, but it won't express the intentions
clearly.

> We cannot emphasize strongly enough that “first-cut” code is not
> finished. — Steve Freeman and Nat Pryce

Code that just works might be an initial success, but it won't withstand the
pass of time.

Alternate between adding code and reflecting upon the resulting code.

Refactoring is improving code without changing functionality. Wait until you've
sorted out the functionality before refactoring.

If a name isn't right, change it. You'll be helping others (and your future
self) avoid confusion.

## Object Oriented Design

Objects depends on other objects. These relationships can be loosely categorized
into:

* Dependencies: Objects needed to fulfill its responsibilities. Must be passed
  explicitly upon construction.
* Notifications: Objects notified while performing different actions. Can be
  added or removed throughout its lifecycle.
* Adjustments: Objects used to adjust its behavior. Needed upon construction but
  can start with default implementations and changed later on.

Focus on the communication between objects rather than on the objects
themselves. This approach create objects defined in terms of the domain, instead
of the implementations.

Object can work only with its local variables or those passed explicitly. It
makes them independent of the context, are easier to reuse, and move around.

Make explicit and name interactions between objects and collaborators.

“Tell, Don't Ask” helps to name interactions hidden between chains of getters.

> If it's explicit, we can name it. — Steve Freeman and Nat Pryce

Strive for easy to understand unit tests and listen to them. It helps to design
objects with explicit dependencies that can be substituted, and clear
responsibilities that can be verified.

Functionality that's difficult to test will also be difficult to maintain in the
future.

Use the tests to discover supporting roles and collaborators that the objects
needs. They can be mocked or faked on tests. Real implementations can be written
afterwards on its own TDD process.

It's difficult to predict which parts of the system will need to change in the
future. Gather together code that will change for the same reason.

Design objects whose responsibilities can be described without any conjunctions.

Objects should exhibit a simpler behavior than all of its components grouped
together.

## Domain Concepts

Work hard to extract domain concepts via value objects or communication
relationships. Even if they don't do much.

These are some techniques to extract domain concepts from existing code:

* Breaking out: When you find that an object is becoming too complex.
* Budding off: When you make explicit a new domain concept in the code.
* Bundling up: When you notice that a group of values are always used together.

Beware of strings, they are perfect to hide domain concepts. Prefer creating
objects to represent domain concepts even if they are trivial.

Trying to test exact matches on strings might indicate a missing domain concept.

## Value Objects

Think hard on when an object is actually a value object.

It may be a value object if its values are immutable.

Creating an interface just to have interfaces, is duplication. If you can't find
a good name for the interface, you may have a value object and don't need the
interface.

## Automatic Tests

Pay attention to code in tests as you do to production code. The styles will
differ, though.

Refactor test code, but not to the point that they get too abstract to stop
understanding what they do. Tests should include the values being tested, even
if there's some duplication.

Make the intention of every test as clear as possible. Differentiate the tested
code, with supporting infrastructure and object structure.

Use tests to clarify the ideas behind your code. Use them as examples on how to
use your code.

The book defines the following types of tests:

* Acceptance tests: Tests from the outside to check the system as a whole. Tests
  whether the system does what it's supposed to do. They can only use
  terminology from the application's domain.
* Integration tests: Tests against code that we can't change. Helps to increase
  your understanding of third party APIs.
* Unit tests: Tests to see if individual objects does the right thing and are
  convenient to work with. Unit test behaviors, not methods.

Limit test scopes to keep them understandable.

Test names are the first hint on what the test do. Name it in terms of what the
object does, not what it is. The name can include the expected result, action
and context.

Use imperative mood for the name of methods that trigger events, and indicative
mood for the name of methods that asserts functionality.

Keep the team's performance up by keeping high quality tests. High quality tests
are better achieved with a high internal quality codebase.

Striving for readable, descriptive and expressive tests gives feedback about the
internal quality of the codebase. Make it read like a declarative description.

Forget about one assertion per test, or one method per test. It's better to test
one behavior per test, which might include a handful of methods and assertions.

Don't think you'll be able to test all configuration combinations and paths.
Professional testers exists for a reason. Never stop working towards better and
better tests.

Consider creating builders before mocking complicated value objects or objects
under test. It reduces duplication and makes it more declarative.

## Test Driven Development

The general approach is to start with an outside event that triggers a behavior,
and work downwards one object at a time.

Start a new project by writing an acceptance test of a “walking skeleton”. Just
enough code to work out how to build and deploy your project. It may take a
surprising amount of effort, but better now than later.

Make as few decisions as possible. Just enough to start the TDD process.

Start a new feature with an acceptance test. It helps to know for certain when
you are done.

While writing new tests, ignore the fact that it won't run. Just concentrate on
the text, tested functionality, and how it looks.

When the test reads well, write the needed test infrastructure to make it fail
due to missing functionality.

If the test fails in a way you didn't expect, fix that. It means that you
misunderstood something.

Having the “right” failure is very helpful for future diagnostics.

> The point of a test is not to pass but to fail. — Steve Freeman and Nat Pryce

Prefer starting with a simple successful case. It helps to define the structure
of the functionality and keep motivation up. Then decide if you prefer to
continue with successful cases or start with error handling.

Learn how to slice up functionality for the incremental development process. It
should be significant enough to tell when it's done, but small enough to be able
to finish it quickly.

The design can be sorted out while doing the development. You just have to keep
a flexible attitude towards new design ideas due to new understanding of the
features.

After finishing a new feature, try to clean it up now that it's fresh.

## Commonly Missed Features and Dependencies

Support logging (errors and info) is a feature and part of the user interface.
Its requirements should be driven by those that depend on it, and it should be
tested.

Time is an implicit dependency. Isolate the concept of time and make its
dependency explicit to those objects that depends on time.
