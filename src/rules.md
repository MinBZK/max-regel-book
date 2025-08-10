# Rules

```
TOC.
- example
- pure functions
- tracing
- smaller reusable bits
- common basic rules
```

Rules are the tools for deriving new facts from given facts. Those new facts encode the decisions we are interested in.

This is an example from before:

- **Rule**: If the group consists of more than 2 members, all persons younger than 12 years get 50% off.

Based on the facts: `group of people` and `the price of a ticket`, we can derive multiple new facts, such as `group size`, `number of people < 12`, `number of people >= 12`, and eventually `total price`.

![Fact set](img/discount-rule.svg =x350 center)

## Composability: Rules as operators

A rule is a function that takes an input fact set, and creates a new output fact set. That makes them *unary operators* in the mathematical sense: functions that take an argument of some type and return something of the same type. Since the input is the same form and shape as the output, this makes clicking them together easy. This makes it easier to build complex rules from smaller, simpler operators without unexpected interactions.

![Fact set](img/compose.svg =x350 center)

> "It is better to have 100 functions operate on one data structure than to have 10 functions operate on 10 data structures."
>
> -- Alan Perlis' Epigrams on Programming (1982).

Those 100 functions on your one data structure can be composed together in lots of unique ways, since they all operate on the same data structure, but you can't really mix the 10 functions on 10 data structures as well, since they were defined only to work on their particular data structure.

When you have multiple data structures, the knowledge of how these data structures are glued together with each other ends up being in functions, hence, writing functions gets harder because you will have to repeat that knowledge again and again.

Whereas if you have just one data structure, this knowledge lies inside that data structure itself, hence, freeing functions of this knowledge.

## Pure operations

In addition to the operator format, we will also make them pure function.

Pure functions always return the same result for the same inputs, with no side effects.
This means the evaluation of a rule is deterministic, making the system predictable. Predictability simplifies debugging and reasoning about the behavior of rules.

## Immutable inputs

Rules never change fact sets. Fact sets are so-called immutable: you can't change its content.
You can create new (immutable) fact sets, though. So instead of adding a fact to a factset, you create
a new factset from the original facts and the one you want to add.

This may seem like a simple choice of words, but it guarantees that when you apply a series of operations (an *algorithm*)
the original data is still intact. This is also called *pure* data processing.

It also handles external data sources, which you may read, but not change in a natural way. You may not be the owner of
an external database that is used, but you can make selections, add fields etcetera to newly derived facts. Later on in the process, we can refer back to some initial factset, and know it is still the same even though it may have past several operations.

Sometimes this "pure" approach is also challenging. If we want to rename a collection, or add some additional field, do we also need to copy over all its facts?
That seems wasteful. Luckily by implementing this in a smart way, those operations stay efficient, without giving up the guarantees that purity brings.

## Traceability

When modelling what a rule should be and how it should behave, we can take that opportunity to bolt on additional functionality. When a rule is triggered, we'll make sure the resulting facts contain a reference to that rule. When the rule engine finishes at some point, the results contain a "bibliography" of rules that aided in the construction of a fact. That helps to audit, finding out why a certain result came to be, easier.

## Metadata for rules

Just like facts, rules have metadata too. This information is also a set of arbitrary key-value pairs, for example "source: law 1.2" or "type: fraud detection". Together with tracing as described before, this allows to know even more about the facts that come out of the inference process.

## Summing up some nice properties

Making rules pure mathematical operators, with tracing capability helps achieving some of our set goals.

- **Correctness**: Pure functions make them deterministic and predictable. It also simplifies testing; you only need to test the function in isolation. Mathematical purity aligns with formal logic and algebraic laws (e.g., associativity, commutativity, distributivity where applicable). This enables formal reasoning, correctness proofs, and possibly static analysis of rules.
- **Scalability**: When operators are pure, you can replace an expression with its value without changing system behavior (known as *referential transparency*). This property is crucial for optimization, caching, and memoization. Since pure operators have no side effects, their evaluations can be run in parallel without risk of race conditions. This can greatly improve performance in large rule sets.
- **Maintainable**: Composability leads to cleaner, more modular rule systems. Pure functions are self-contained; they don't depend on external state or mutate anything.





## Rules *can* be...


- Reorganize parts
- Return the input as part of the output fact set
- Ignore any input and just return some new fact set.
- Generate facts that indicate some action (e.g. send e-mail, or generate warning)
- Use metadata. Remember that facts contain the main data (*term*) but also the metadata (*info*). Both parts are available when creating a rule. That means metadata can be part of the logic to create a result, making rules expressive. E.g. a rule that filters people based on age (in *term*) and on source (in *info*).
- they can take parameters to do their job. So we can create a rule that does many things at the same time. For example a function with the parameters `color = yellow`, `shape != round`, determines the total price for each banana in a (fact)set of available fruits. But we may already have trouble with one of our design goals: *traceability*. If a fruit is not yellow or round enough for a banana, it is harder to automatically track which of those demands was not met. The same holds for the total price of those fruits: does it make sense for the number of items that met the criteria?
  This is why we promote breaking down rules in reusable, simpler rules. Those can we individually checked for correctness and allows for running an audit log on those components.


