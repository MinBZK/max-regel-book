# Fact sets

In our rule engine, you would think we make *facts* the core unit to reason about. 
After all, it contains all data (and metadata) you want to consider when inferring more facts.

MaxRegel makes *fact sets* the central data element of computation.
A fact set is a collection of facts and can contain any number of elements.
Sets can be empty, have a single fact, ... a million.

A fact set can be visualized as a box containing the facts.

![Fact set](img/factset.svg =x150 center)

This adds extra expressivity to the things we can later ask about data, making it possible to write more complex logic in our rule system.

Not only can we ask: "Is this person older than 12 years old", considering just a single fact, but also:
"How many people older that 12 are in this group of people", considering a (sub)groups of facts.

Without support for fact sets, this last example would rely on some predefined, hard coded rule to extract those people. This logic would then reside outside the rule system, making it inflexible. Another take could add a field `lives_at_address_and_is_older_than_35` for each individual, making it an intrinsic property. That feels also very arbitrary and is equally inflexible.

## Parts: Organizing fact sets

So went from facts to fact sets as data unit for rules to process. We'll add another feature to those fact sets: a way to (rather superficially) organize *parts* in a factset.

A part in a factset is essentially a subset, i.e. a section of facts that belong together under a label (the part name). For example, a factset may contain `persons`, `locations`, `vehicles`, ... Think of them as lists of facts that can be easily selected.

![Fact set with parts](img/factset-labeled.svg =x150 center)

We now visualize single factset as a stack of boxes. Those boxes with different colors are the parts. The individual facts (spheres) are not drawn anymore: the boxes still contain any number of facts, though.

Later on, when we introduced different tools to filter or join fact sets, we can use parts to add new subsets under different labels.

Note that parts are the names that exist in the fact set, but that this name is not a part of the Fact's term or info. In that sense, it is just an additional label. 

## Unified data: one format to rule them all

The possibility to have parts in a factset is a nice addition, but it is not an optional one. The structure of terms, wrapped in facts, wrapped in factsets with parts is a mandatory way of working. And that may seem superfluous for simple cases. What if you want to make a simple fact, indicating a user is admitted to some program, according to law? The corresponding rule would have to return `true` or `false`, right?

```json
{
  // the factset
  "result": [ // all facts for the part called "result"
    { // a fact
      "term": {
        "admitted": true // the thing we actually try to say
      },
      "info": { // some meta data
        "source": "law"
      }
  ]
}
```

As you can see here the actual value is wrapped in a term, that is wrapped in a fact, that is contained in a fact set with part name "result". That is a lot of wrapping, but it is the only data structure you'll ever have to deal with. No other datastructures, such as lists/arrays, maps/dictionaries, tuples, you name it, are used on the level of the rule engine. And that makes clicking them together, staying in the same "realm of thought", much easier.


## Basic operations: what fact sets can do

Without having defined yet how we can create rules on this data container yet, we can define some useful functions for fact sets. Those will be the basic functionality we can rely on when building larger chunks of logic.


Operations on fact sets:

| function name    | argument  | returns  | description                                                           |
|------------------|-----------|----------|-----------------------------------------------------------------------|
| `fs_create`      | facts     | factset  | create a new factset from the facts provided (as list, iterator, ...) |
| `fs_iterate`     | -         | facts[]  | retrieve all fact objects contained                                   |
| `fs_count`       | -         | integer  | retrieve the number of facts contained                                |
| `fs_filter`      | predicate | factset  | retrieve only the facts that pass a given predicate/test              |
| `fs_part_names`  | -         | string[] | retrieve all available part names this factset contains               |
| `fs_get_part`    | part name | factset  | retrieve only the facts from a given part name                        |
| `fs_set_part`    | part name | factset  | all facts now belong to a given part                                  |
| `fs_remove_part` | part name | factset  | a factset containing all parts, but the one provided                  |

## Implementation a-specific

The operations just specified in fact define what a factset *is*: something that supports "getting a part", "filtering out facts", or "determining the number of facts in it" makes up a *fact set*.

When we start programming this system, you can go about it in different ways. Should the fact objects be in a list data structure, or in some map/dict? Maybe the facts already exist as rows in a database. Can we leave them there, and create a fact set wrapper that translates the basic operations into SQL? What is there is an online API, can we handle that as a fact set?

It turns out that we can combine different implementations of a factset, all with their own strengths: this one is fast for small sets, this one is good for giant datasets, and another one can work with on-demand data retrieval... But those nitty-gritty details don't matter when we want to think about the data in higher level terms. We can use a database in the same way as some simple facts coded directly in the system. Those fact sets can be used interchangeably, without having to know how it works under the hood, but forming a single factset to reason with.

## Concatenating Factset

In many occasions, we would want to merge fact sets into a single factset. A factset with children and another with parents can make up a "family" factset for example. One could iterate over the two fact sets individually and create a new one. All the underlying facts are then copied into a new data structure. Since we foresee merging two fact sets will happen often, this may become too resource intensive to make it fast. 
A dedicated fact set implementation can be created, a *concatenated factset*, that does this in an efficient way. Instead of copying over all facts, it stores two references to the initial (and remember: immutable!) fact sets. When it is asked to determine its count, it will ask the underlying fact sets to give their count and add them up. The same for iteration and filtering facts. This concatenated fact set is said to be a *lazy implementation*, as it postpones the actual work until the last moment that it is requested. 

So, even though this could be implemented more directly, this special case is partly the success to a fast inference engine. And the good thing is: you can almost forget about it again, since it is just another fact set.

## Some last notes

When designing this data structure, you have to make choices of what *is* and what *is not* allowed for a factset. For our system the choice was made for a factset to allow: 

- **duplicate facts**: you can add, say, a person fact twice.
- **heterogeneous sets**: you can add a fact about a person and a fact of a car (different types) to the same fact set (and part).

It gives the system more expressiveness.

