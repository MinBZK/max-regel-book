# Design

Here we lay out the key drivers to embark on the creation of a new rule engine.
By identifying design issues and choices before diving in into the weeds of the system's building blocks, the "why" should be more clear. Next we will look at goals, non-goals and consider some cross-cutting concerns.

A generic rule engine can take many shapes and forms. Here we try to identify the elements that are dear to us, placing the MaxRegel rule engine in the broader space of related systems.

## Goals
Features that we aim for:

### Transparency

The engine should make its reasoning visible. Users should be able to see why a decision was made.

This means full traceability: a clear record of which rules fired, in what order, and which facts were used to reach a conclusion.

### Correctness

The engine should be trustworthy. Rules should work as expected and give predictable results.

Achieved by breaking logic into small, testable building blocks. Each part can be proven correct individually, reducing errors in the larger system.

### Scalability

The engine should handle large workloads without slowing down. It should work quickly even with many rules and lots of data.

- Performance: Complex rule sets should process thousands of cases per second, enabling use cases like running simulations during policy design.
- Connectivity: Can pull in data from external sources (databases, REST APIs, etc.) as part of its reasoning.

### Expressiveness

The engine should let you describe logic in powerful ways. 

- Tools to rework facts: You can transform, filter, and combine data however you need to create new fact sets.
- Declarative style: The order of rules and facts doesn't matter, making it easier to compose small pieces into larger systems.

### Maintainability

The system should be easy to understand, change, and extend over time. Built simply, without unnecessary complexity.

- From the ground up: pure operations, minimal moving parts.
- No external dependencies: avoids breaking changes from outside libraries and avoids bloat from unused features.

### Rule/Data Minimalization

The engine should keep rules and data as simple and focused as possible. Avoid processing data that may not be needed for improved privacy.

Lean models and concise rules reduce maintenance and improve clarity. When an (intermediate) conclusion can already be made, there is no need to collect more (unnecessary) data. 

### Extensibility

The engine should support new ideas and integrations without major rewrites. One can add new functionality and connect to other systems with ease.

- Forward chaining generates all possible new facts, not just those needed for one outcome.
- These facts can be shared with other systems (e.g., via message queues) at any stage of processing.

### Multiple Layers for Writing Rules

The engine should be usable at different levels of abstraction. You can choose the way to write rules that fits your needs.

Options range from code → combinator library → fluent API → external scripting, allowing flexibility for different teams and skill sets.

---

Some goals can easily be married using the right implementation strategy. For example, an external-dependency-free approach, is part of simplicity, which aids *transparency* and *correctness*.
But our need for expressiveness can clearly compete with other goals. Designing this system is therefore a balancing act.

## Non-goals

Overly generic. It is not a replacement for a full-fledged logical programming language, sucha as Prolog.

Extremely high speed. No compilation to bare metal machine code. Readability and correctness in favor of raw speed.

A end-user friendly rule editing as main goal. The idea that domain experts use a high-level *domain specific language* and/or editor is attractive. It empowers smart people to implement changes in a policy themselves. When strongly believe in supporting such a way, though making that leading from the start may become a red herring in finding a solid base. We expect rules should be shared in a machine readable format too, for example. We have faith our axiomatic way of building higher level building blocks, bit by bit, will (among other things) also allow for highlevel rule representation that suits domain experts.

It also leaves out typing of fields in domain objects. It can be very useful to spell out in great detail that the "price" information of an item costs is not simply "1.95", but that the precision is two digits, the currency is Euro and that is should be formatted as "€ 1,95". It is a complementary effort that MaxRegel can be extended with if required.

## Cross-cutting concerns

- This system should be future-proof by its simplicity.
    - It is a small code base.
    - No fancy language features.
    - No external dependencies.
    - Ability to extent on top of high level parts.

