# Scripting

We now have rules and fact sets (that rules always operate on) in our toolbox. Next, we'll define some basic rules, that are simple in itself, but prove to be useful ingredients to create higher level rules.

Those higher level rules are closer to the domain we want to model, and are therefore more interesting ultimately. 

We'll work up rules to become a programming language in itself. Not one that let's you create games, but one dedicated to create logical systems.

## Basic rules

Remember that we defined fact sets to have certain functionality baked in. Independent of whatever rules we are going to create, fact sets can do certain things themselves. 


| function name | argument  | description                                              |
|---------------|-----------|----------------------------------------------------------|
| `fs_iterate`  | -         | retrieve all fact objects contained                      |
| `fs_size`     | -         | retrieve the number of facts contained                   |
| `fs_filter`   | predicate | retrieve only the facts that pass a given predicate/test |
| `fs_get_part` | part name | retrieve only the facts from a given part name           |
| `fs_set_part` | part name | all facts now belong to a given part                     |

- some first rules to recombine any parts: from, remove, concat.
  with those you can recombine fact sets with arbitrary parts. Taking a things from here and add them there.

Building a scripting language.
- then
- assignments


- , filter, count
-
- 