# Implementation

Now we have a design, a model for capturing logic with factsets and rules, it can be implemented using any programming language. Up until this point, we did not bother with any code, and we will keep it that way. Still we can make some suggestions for any implementation. 

## General datastructures vs Classes

The things that contain our data (terms, wrapped in facts) can be implemented in different ways. One way is to use generic data structures like (hash)maps/dictionaries found in all programming languages. This data structure does not make assumptions about its content and that makes them very versatile. But you may prefer strict objects for your data. A `person` can not have a property `screen_brightness` (for humans are no computer screens, after all). Many languages have "classes" in one way or more. This strictness helps preventing certain mistakes. 

In our reference implementation, *terms* can be implemented with both of those strategies. Both can be beneficial in the right context. A term `interface` guards that those implementations can be treated the same. For example, they can both enumerate their field names, and equality checks happen in an order independent way. 

## Factsets

Factsets are things that support a certain set of function (count, iterate facts, filter, etc). Multiple implementations exist, covered by an interface, to allow efficient processing of data.

The most general factset implementation is a class that embeds a hashmap to do its bookkeeping. Keys of this map are the part names and the values are lists of facts. In a way you don't need more. 

A special implementation allow just for a single part, and just stores the facts in a list. Internally this is used a lot, and some operations become cheaper to perform. 

An even more reduced implementation is the *empty* factset. As you can guess, no facts are contained at all. This is a very common value, to denote "no result", and is worth optimizing for.

A *concatenated factset* was already mentioned before. Being able to combine factsets in constant time, \\(\mathcal{O}(n\log n)\\), simply means instantaneous results, independent of the number of facts contained.

Another special mention is the *SQL factset*. Instead of storing facts in memory, it translates operations into SQL, so a connected database can be queried. A `count` becomes `SELECT COUNT(*) AS n FROM TABLE 'partname';` under the hood. Filtering requires translation of the predicate objects to proper `WHERE` expressions. When special operations can't be translated into SQL, all data will have to be copied into an in-memory, general, factset. 

Remember factset are immutable. In case of a database backed fact set, that is very evident, as one is often not allowed to change data, but only to read from it. 

Also, a combined (concatenated) factset can be made up from different implementations (a database, REST API, in memory data), without a user having to care about this. 

## Rules

Rules are pure functions leaving the input intact. That means, if the slightest tweak to the data is to be performed, the data will need to be copied (or referenced) into a new factset object. The "setpart" operation, assigning a new name to a factset is used often. It is a building block used every time you use an assignment. But changing the name modifies the factset, right? Cheap copies of factset, just overwriting the name, is an optimization that pays off quickly and was implemented for that reason. 

Since the interface `factset -> factset` for a rule is so uniform, we can benefit from making high level tools to validate and represent rules in different ways in a general way. Apart from a rule's `apply()` function, there is an `ast()` function that generates an Abstract Syntax Tree (AST) of the rule, so its components can be visited in a uniform way. 


