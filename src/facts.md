# Facts

Let's define the data structure for storing *facts*. 
Facts are the basic building block for storing data. If we make sure that input, intermediate and output data is always structured the same, it facilitates creating processing pipeline that click together easily.

We'll split storing data for facts into two parts:
- The *term*, which is the "meat" of the data. 
- The *info*, which is the metadata

Next, those parts are explained further.

## Term: core information

When dealing with rules, we want our data to be chunked in meaningful objects, containing the information you actually care about.

For example, we want to define a `person` that consists of a `name` and an `age`.

![A term](img/term.svg =x150 center)

In our terminology, we'll say that the *term* `person` has two fields. A field has a *key* (the name of the variable) and an actual *value*.

```json
person = {
  "name": "Bart",
  "age": 10
}
```

An `address` can be another term, with fields `street`, `house_number`, `city` etc.

Those simple objects are commonly referred to *domain objects*, *model objects*, *(named) tuples* or *terms*. We use the latter, "Term", to denote a defined part from the domain of discourse. 

The keys of a term, could in theory be anything, but for mental comfort we'll limit them to lower cased names, possibly separated by an underscore ( _ ).

> [!NOTE]
> **Implementations detail for programmers.**
> Terms can be implemented as mappings, dicts, tuples, objects. This framework supports them all and build on top of an abstract Term class. It also provides a general functionality for equality checks  that compares terms in an field-order independent way.

## Info: Meta data

The meta data, or simply called *info* here, is in a way of secondary importance. 

![Meta data](img/info.svg =x150 center)

The *actual* data is the *term*, but for example:

- the source of a term (is this `person` data coming from social media, an address book, a government API?)
- the date this data was created
- what previous rules provided this data, for tracing back how this person is processed in the rule engine.

The latter shows how *info* can be used by the rule engine for certain bookkeeping.

Like terms, *info* is organized in key-value pairs.

## Facts: Terms with meta data

Facts will form the unit of individual data points, for the remaining discussion of the system, since it is the combination of "normal" data (the *term*) and meta data (the *info*), and therefore contains all we need.

![A fact](img/fact-eq.svg =x150 center)