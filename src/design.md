# Design

Here we lay out the key drivers to embark on the creation of a new rule engine.
By identifying design issues and choices before diving in into the weeds of the system's building blocks, the "why" should be more clear. Next we will look at goals, non-goals and consider some cross-cutting concerns[^note].

[^note]: A useful structure, that is loosely followed here, for creating design documents is described here: https://www.industrialempathy.com/posts/design-docs-at-google/

A generic rule engine can take many shapes and forms. Here we try to identify the elements that are dear to us, placing the MaxRegel rule engine in the broader space of related systems.

## Goals
Features that we aim for:


- transparency:
  - In the form of traceability when using the engine. Can you explain to a user what rules and decisions were used to come to
- correctness: break down in simple, provable (or at least testable) building blocks.

- scalable. 
  - Performant. A handwavy indication is that complex rule sets should be able to handle thousands of cases per second. Speed enables different use cases for have a rule engine in place. For example running simulations during policy design.
  - Connect to external data sets (such as databases or REST APIs).

- expressiveness. 
  - provide the tools to rework facts (add fields, filter facts on arbitrary predicates, create new facts). A lot of existing libraries do this well.
  - declarative: order independent facts and rules. This makes composition of smaller parts easier.
  
- maintainable.
  - In the implementation. implementation from ground up. pure operations. simplicity.
  - no external dependencies. independence of changes outside control. no weight of unneeded features.

- rule/data minimalization: 
- extensible: forward chaining allows to create all possible new insights and tap into new facts at different stages in the process. This makes sharing information to other systems easier, since running the inference engine did not focus on a specific outcome from the start. All new facts could potentially bu send into a message queueing system, for example.  

- layers of implementing rules: code $\to$ combinator lib $\to$ fluent api $\to$ external script.

simplicity leads to transparency and correctness. 
dependency free is part of simplicity.
expressiveness and simplicity can easily contradict each other. 


Some goals have overlapping implementation strategies, such as *transparency* 

## Non-goals

Overly generic. It is not a replacement for a full-fledged logical programming language, sucha as Prolog.

Extremely high speed. No compilation to bare metal machine code. Readability and correctness in favor of raw speed.

A end-user friendly rule editing as main goal. The idea that domain experts use a high-level *domain specific language* and/or editor is attractive. It empowers smart people to implement changes in a policy themselves. When strongly believe in supporting such a way, though making that leading from the start may become a red herring in finding a solid base. We expect rules should be shared in a machine readable format too, for example. We have faith our axiomatic way of building higher level building blocks, bit by bit, will (among other things) also allow for highlevel rule representation that suits domain experts.

It also leaves out typing of fields in domain objects. It can be very useful to spell out in great detail that the "price" information of an item costs is not simply "1.95", but that the precision is two digits, the currency is Euro and that is should be formatted as "€ 1,95". It is a complementary effort that MaxRegel can be extended with if required.

## Alternatives considered

## Cross-cutting concerns

- This system should be future-proof by its simplicity.
    - It is a small code base.
    - No fancy language features.
    - No external dependencies.
    - Ability to extent on top of high level parts.

