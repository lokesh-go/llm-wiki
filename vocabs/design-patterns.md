# Design Patterns Vocabulary
# Source: https://refactoring.guru/design-patterns (22 classic GoF patterns)

# CREATIONAL PATTERNS — object creation mechanisms

## singleton
Ensures a class has only one instance and provides a global access point.
Use carefully: hides dependencies, makes testing harder, global state risk.
Related: [[factory]]

## factory-method
Defines an interface for creating an object, but lets subclasses decide which class to instantiate.
Related: [[abstract-factory]], [[builder]]

## abstract-factory
Creates families of related objects without specifying concrete classes.
Use when: system must be independent of how its products are created.
Related: [[factory-method]], [[singleton]]

## builder
Constructs complex objects step by step. Separates construction from representation.
Use when: object has many optional parameters. Avoids telescoping constructors.
Related: [[factory-method]]

## prototype
Creates new objects by cloning an existing object.
Use when: object creation is expensive and a similar object already exists.
Related: [[singleton]]

# STRUCTURAL PATTERNS — assembling objects and classes into larger structures

## adapter
Converts an incompatible interface into one a client expects.
Wraps an existing class to make it work with others without modifying source.
Related: [[facade]], [[decorator]]

## bridge
Decouples an abstraction from its implementation so the two can vary independently.
Use when: you want to avoid permanent binding between abstraction and implementation.
Related: [[strategy]], [[abstract-factory]]

## composite
Composes objects into tree structures to represent part-whole hierarchies.
Use when: you want clients to treat individual objects and compositions uniformly.
Related: [[decorator]], [[iterator]]

## decorator
Attaches additional responsibilities to an object dynamically.
Flexible alternative to subclassing for extending functionality.
Related: [[composite]], [[strategy]]

## facade
Provides a simplified interface to a complex subsystem.
Reduces coupling between client code and subsystem internals.
Related: [[adapter]], [[mediator]]

## flyweight
Uses sharing to support a large number of fine-grained objects efficiently.
Use when: large number of objects consuming too much memory (e.g., game particles, chars in text editor).
Related: [[composite]]

## proxy
Provides a surrogate or placeholder for another object to control access.
Types: virtual (lazy load), remote, protection, caching, logging.
Related: [[decorator]], [[facade]]

# BEHAVIORAL PATTERNS — communication between objects

## chain-of-responsibility
Passes a request along a chain of handlers. Each decides to process or pass on.
Use for: middleware pipelines, event handling chains.
Related: [[command]], [[observer]]

## command
Encapsulates a request as an object, allowing queuing, undo/redo, parameterisation.
Related: [[CQRS]], [[event-sourcing]], [[chain-of-responsibility]]

## iterator
Provides a way to sequentially access elements of a collection without exposing internals.
Related: [[composite]]

## mediator
Defines an object that encapsulates how objects interact. Reduces direct dependencies.
Related: [[facade]], [[pub-sub]]

## memento
Captures and externalises an object's internal state for later restoration (undo/redo).
Related: [[command]]

## observer
Defines a one-to-many dependency — when one object changes, all dependents notified.
Foundation of event systems, reactive programming, GUI frameworks.
Related: [[pub-sub]], [[mediator]], [[event-driven-architecture]]

## state
Allows an object to alter its behaviour when its internal state changes.
Appears to change its class. Good alternative to large if/switch statements.
Related: [[strategy]], [[command]]

## strategy
Defines a family of algorithms, encapsulates each, makes them interchangeable.
Use when: you want to switch behaviour at runtime.
Related: [[state]], [[bridge]], [[decorator]]

## template-method
Defines the skeleton of an algorithm in a base class, letting subclasses override specific steps.
Related: [[strategy]], [[factory-method]]

## visitor
Lets you add further operations to objects without modifying them.
Use when: you need to perform many distinct operations on an object structure.
Related: [[composite]], [[iterator]]
