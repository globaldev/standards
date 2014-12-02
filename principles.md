# Development Principles

## General

* Avoid redundancy: **Don't Repeat Yourself** (DRY) if you're writing code
  yourself, and **don't duplicate the functionality of a built-in library**.
* **Keep It Simple (KISS)**: simple code takes less time to write, has fewer
  bugs, is easier to modify, and easier to understand.
* Part of KISS is to do the simplest thing that could possibly work, and don't
  write code that guesses at future functionality. In other words: **practice
  YAGNI**.
* Follow the **principle of least surprise**: people generally make quick
  assumptions about blocks of code, so try and make this impression accurate.
* **Don't program by coincidence**: be deliberate about what you build.
* Remember that someone else will probably end up maintaining your code, so
  **write code for that future maintainer**.
* **Exceptions should be exceptional**, so don't use them in place of
  validations.  Also, **don't swallow exceptions**.

## Object-Oriented Design

* **Single Responsibility Principle**: Objects should have a single
  responsibility.
* **Open-Closed Principle**: classes should be open for extension, and closed
  for modification.
* **Liskov Substitution Principle**: subtypes must be substitutable for their
  base types.
* **Law of Demeter**: code components should only communicate with their direct
  relations.
* Avoid global variables.
* Avoid long parameter lists.
* Limit collaborators of an object (entities an object depends on).
* Limit an object's dependencies (entities that depend on an object).
* Favour composition, delegation and aggregation over inheritance.
* Favour small methods: between one and five lines is best.
* Favour small classes with a single, well-defined responsibility: when a class
  exceeds 100 lines, it may be doing too many things
