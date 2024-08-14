# Context

Facade pattern is a design pattern that simplifies the interface of a complex system/set of classes.

Imagine you are writing a library which has a lot of interacting classes, and it's very frustrating for the clients to understand and use it.

Facade pattern would be very useful in this case because it can simplify the interface so that clients can use it easily and **any changes to the underlying implementation won't affect the interface**.

# Implementation

Facade pattern can be seen as a controller, which knows everything about the system and only shows useful methods to the clients.

For example, when building a Kafka system, ie. a producer-consumer system, a simple interface for this system may consist of:

- Fields: producers, consumers, topics, etc.
- Methods: createProducer, createConsumer, createTopic, produce, consume, etc.

Refer here for [code](https://refactoring.guru/design-patterns/facade).

# Pros

- We can isolate our code from the complexity of a subsystem.
- Any changes in the subsystem does not affect the client code, because the interface does not change.

# Cons

- A facade may gets too big (called a (god object)[https://refactoring.guru/antipatterns/god-object]) which coupled all classes of a system.
  - We may get around this by breaking down the facade into smaller ones.
