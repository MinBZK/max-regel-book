# Design

Here we lay out the key drivers to embark on the creation of a new rule engine.
By identifying design issues and choices before diving in into the weeds of the system's building blocks, the "why" should be more clear. Next we will look at goals, non-goals and consider some cross-cutting concerns.

A generic rule engine can take many shapes and forms. Here we try to identify the elements that are dear to us, placing the MaxRegel rule engine in the broader space of related systems.

## Goals

When choosing or building a rule engine, organizations care about more than just “does it work?” They need systems that are trustworthy, adaptable, and sustainable. The design goals of this engine focus on exactly those concerns: **transparency** ensures decisions can be explained to users and auditors; **correctness** builds confidence that outcomes are reliable; **scalability** makes the system practical for real-world volumes of data and rules; **expressiveness** allows complex business logic to be modeled without friction; **maintainability** keeps long-term costs low and avoids dependency risks; minimalization prevents bloat and confusion; **extensibility** makes the system future-proof and integrable with other tools; **rule/data minimalization** avoids retrieving more data than needed to get answers; and **layered rule definition** offers flexibility for both technical and non-technical teams. Together, these principles align the engine with the priorities of organizations that value clarity, performance, and long-term resilience.

We will look at those goals in more detail:

> [!WARNING]
> komt voort uit concat factsets... je hoeft niet hele databases te copieren, maar je kan ze transparant uitlezen wanneer nodig, maar behandelen .....   aaaaaah. 


### Transparency

The engine should make its reasoning visible. Users should be able to see why a decision was made.

This means full traceability: a clear record of which rules fired, in what order, and which facts were used to reach a conclusion.

Not all users need the same level of transparency. A customer getting a discount may be happy enough with "seasonal discount", where a customer support "user" of the rule engine would like to see more details of why this customer was or was not eligible in order to resolve disputes. A marketing team would like to make clear what customer journeys lead to what discounts for improved targeting. 

### Correctness

The engine should be trustworthy. Rules should work as expected and give predictable results.

Achieved by breaking logic into small, testable building blocks. Each part can be proven correct individually, reducing errors in the larger system. Smaller blocks can be more easily understood and checked if they encode the desired behavior. It is a "divide and conquer" approach to correctness.

### Scalability

The engine should handle large workloads without slowing down. It should work quickly even with many rules and lots of data.

- Performance: Complex rule sets should process thousands of cases per second, enabling use cases like running simulations during policy design.
- Connectivity: Can pull in data from external sources (databases, REST APIs, etc.) as part of its reasoning.

### Expressiveness

The engine should let you describe logic in powerful ways.

- Tools to work with facts: You can transform, filter, and combine data however you need to create new insights (fact sets) in order to capture the logic you want.
- Declarative style: The order of rules and facts doesn't matter, making it easier to compose small pieces into larger systems. For example, *"people under 12 are young"* and *"young people get a discount"*, leads to the same conclusions if the order of those rules is changed: *"young people get a discount"* and *"people under 12 are young"*. That is quite obvious here, but in larger systems this makes it easier to read rules by themselves, instead of knowing the order of their contextual rules. 

### Maintainability

The system should be easy to understand, change, and extend over time. Built simply, without unnecessary complexity.

- From the ground up: pure operations, minimal moving parts. For organizations this is a way to keep long-term costs low.
- No external dependencies: avoids breaking changes from outside libraries and avoids bloat from unused features. It also is a safety when a software vendor runs out of business; rule engines quickly become core to an organization and external risks should be minimized.

### Rule/Data Minimalization

The engine should keep rules and data as simple and focused as possible. Avoid processing data that may not be needed for improved privacy.

For example, when we can already see a customer is a member, and therefore will receive a discount, another rule checking the same discount based on a more privacy-sensitive age data can be omitted.

Lean models and concise rules reduce maintenance and improve clarity. When an (intermediate) conclusion can already be made, there is no need to collect more (unnecessary) data. 

### Extensibility

The engine should support new ideas and integrations without major rewrites. One can add new functionality and connect to other systems with ease.

Forward chaining generates all possible new facts, not just those needed for one outcome. 
These facts can be shared with other systems (e.g., via message queues) at any stage of processing.

In our discount example, when determining the number of kids in a family, this is done for determining the discount. But that number may itself be fed into a fraud detection system. E.g. "more than 100 children in one family generates an alert of possible fraud". Or, for people receiving a discount, send some happy email to offer them next month another benefit.

### Multiple Abstraction Levels for Writing Rules

We know simplicity and expressiveness are at fundamental odds with one another for many domains.
A very complicated rule in real life would require the rule engine to have advanced and complicated features to capture the logic accurately. But complicated features make it error-prone, and limit maintainability and transparency.

Therefore, a goal is to support multiple ways of writing rules, using at different levels of abstraction. That allows you to use a clearer rule, say 90% of the time. But there is an escape hatch for the other complicated 10%. Those rule can then be coded in a less user-friendly way, but gets the job done. It allows one to write rules in a way that fits your needs.

| Level                    | Expressiveness                  | simplicity                      | Write a rule...                                                                                                                                         |
|--------------------------|---------------------------------|---------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| raw programming language | \\( \star \star \star \star \\) | \\( \star \\)                   | in a full fletched programming language with all possibilities and pitfalls.                                                                            |
| combinator library       | \\( \star \star \star \\)       | \\( \star \star \\)             | using reusable function of the inference engine in code.                                                                                                |
| fluent API               | \\( \star \star \\)             | \\( \star \star \star \\)       | like an embedded Domain Specific Language (DSL) within code.                                                                                            |
| rule scripting language  | \\( \star \star \\)             | \\( \star \star \star \star \\) | in the most human readable, yet formal, way possible as external scripts, not requiring the hosting programming language the rule engine is written in. |

A special mention is a rule exchange format, in a way that external DSL editors (such as Jetbrains Meta Programming System, MPS). This is out of scope for this document, but would likely tap into the *combinator library* level.


> [!WARNING]
> Niet echte "goal", maar voorsorteren op oplossing (expressivenes vs transparency/simplicity)

> [!WARNING]
> Beleid: Extensibility gegarandeerd. je kan altijd uitbreidingen maken en nieuwe regels invoeren...

---

Some goals can easily be married using the right implementation strategy. For example, an external-dependency-free approach, is part of simplicity, which aids *transparency* and *correctness*. But our need for expressiveness can clearly compete with other goals.

Also note that our choice of "forward chaining" inference engine possibly uses and creates more data than strictly needed for an application. Our goal of *data minimization* contradicts the forward chaining approach[^note-dm].

[^note-dm] In the "Scripting" chapter you'll learn that rules can be cut short (or prioritized) by using `return_if` rules to support a level of data minimization.

All in all, designing this system is therefore a balancing act.

## Non-goals

Overly generic. It is not a replacement for a full-fledged logical programming language, such as Prolog. General programming languages allow you to build anything, from rule engines to games. It comes at the price complexity with little aid to spell out you specific business logic.

Extremely high speed. No compilation to bare metal machine code. Readability and correctness in favor of raw speed.

> [!WARNING]
> wie kan beleid schrijven en vertaling naar code process makkelijk. niet per se code zo makkelijk dat iedereen het meteen gebruikt voor beleid maken. geen realistisch scenario. 

A end-user friendly rule editing as main goal. The idea that domain experts use a high-level *domain specific language* and/or editor is attractive. It empowers smart people to implement changes in a policy themselves. When strongly believe in supporting such a way, though making that leading from the start may become a red herring in finding a solid base. We expect rules should be shared in a machine readable format too, for example. We have faith our axiomatic way of building higher level building blocks, bit by bit, will (among other things) also allow for highlevel rule representation that suits domain experts.

It also leaves out typing of fields in domain objects. It can be very useful to spell out in great detail that the "price" information of an item costs is not simply "1.95", but that the precision is two digits, the currency is Euro and that is should be formatted as "€ 1,95". It is a complementary effort that MaxRegel can be extended with if required.

## Cross-cutting concerns

> [!WARNING]
> plaats in voorliggende secties

- This system should be future-proof by its simplicity.
    - It is a small code base.
    - No fancy language features.
    - No external dependencies.
    - Ability to extent on top of high level parts.~~

