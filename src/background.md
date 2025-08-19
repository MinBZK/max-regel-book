# Background

Here we take a closer look into *rule engines*, or *inference systems*, in general. 
By taking a step back and become more a bit more philosophical, the choices we can make when implementing such a system become more apparent. That can then feed into the design decisions later on.

## Rule engines

A rule engine is a software system that makes decisions based on a set of "if-this-then-that" rules. Instead of writing a lot of complex code, you just list out the rules, and the engine figures out what to do based on the facts it knows.

### Example: Personal Discount Ticket

- **Rule**: If a person is a member and the total is over $100, give 10% off.

The engine checks if both conditions are true, and if so, applies the discount.

### Example: Family Discount Ticket

- **Rule**: If the group consists of more than 2 members, all persons younger than 12 years get 50% off.

The engine checks the group of people, finds the kids if there are any, and and if so, applies the discount.

### Example: Combined Discount

- **Rule**: If there is Family Discount use that, otherwise consider the personal discount. If both don't hold, give 5% discount on week days.

This rule combines the previous rules into one, and adds another case as backup.

So, why use a Rule Engine?

- Easier to change rules without touching the main code.

- Great for systems with lots of conditions, like fraud detection, insurance claims, or benefit systems.

In short, a rule engine helps automate decisions by separating logic (rules) from code. This benefits flexibility, clarity and correctness. Sometimes the term *business rules* is used, to emphasize the rules are about higher level domain knowledge, as opposed to some operational details in the operation of the software.

## Automation strategies

If we understand a problem so well, that we can *even* explain it to a computer, than we are on the right track. It means the domain knowledge, the data and rules, are formalized and can be handled by software for automatically determining the consequences.

A simple inference engine cycles through all rules, and execute them as they come along. The execution of the rules will often result in new facts or goals being added to the knowledge base, which will trigger the cycle to repeat. This cycle continues until no new facts emerge.

In case of the *Combined Discount* example, the first part checks the *Family discount* rule. That one in turn needs to check the group size first. It may emit the group size as a new fact: `count: 4`. It may then add a new fact `kids_count: 2`. Both of those new facts can yield yet another new fact: `apply_family_discount: true`. Adding more and more small facts can lead to higher level facts.  

This approach of (blindly) generating all the possible facts from given input facts is called *forward chaining*. By creating new facts from initial facts, and chain those in turn for yet another round of new facts, etcetera, all new facts show up at some point. Clearly, some facts will never emerge if the input conditions are not met. 

Another approach, *backward chaining*, looks at the facts you would like to proof first, checks what rules need to be satisfied with all currently known facts. Otherwise, it tries to find rules to satisfy *those* facts in turn first, and so on. From the goal, you "chain the rules backward" all the way and see if the required input facts are there.  

Both approaches have their pros and cons. 

| Feature    | Forward Chaining              | Backward Chaining               |
| ---------- | ----------------------------- | ------------------------------- |
| Direction  | Facts → Conclusions           | Goal → Needed facts             |
| Best for   | Real-time updates, inference  | Answering specific queries      |
| Think like | Detecting all what can happen | Finding out how to reach a goal |

Our system uses forward chaining, the more data-driven approach. It's strong points are: 

> [!WARNING]
> TODO: add policy examples

- When you start with known facts and want to discover *all possible conclusions*. This is convenient when, at different stages, intermediate conclusions are relevant for other parts of an organization, without enumerating all possible relevant facts for external or future systems. 

- When new data is constantly coming in, and you want the system to react to it automatically.

It comes at the price of being more wasteful: many facts may be generated that are irrelevant for most applications. 

> [!WARNING]
> TODO: explain what happens to irrelevant facts? link met data minimalisatie. hier niet het geval met forward chaining. requires extra thinking...


## Modelling

Next we'll see how such a system can be put together, by exploring *modelling* a bit further.

Modeling, in simple terms, means creating a simplified, structured representation of something from the real world—like a map for a city, but for ideas, systems, or processes. Instead of working with the messy, complicated real thing, you make a formal version that captures the important parts so you can understand, analyze, or predict.

In case of a rule engine, writing down rules and facts in a structured way is a model of decision-making.

The benefits of having a model are:


> [!WARNING]
> Kleine voorbeelden...

- **Communication**: It forces you to think clearly and remove ambiguity. Everyone can follow the same formal representation. A model is a common language for different stakeholders (business, developers, scientists).

- **Exploration**: You can analyse "What if?" scenarios safely—without breaking the real system.

- **Automation**: Computers can work with a formal model to simulate, optimize, or make decisions. Also mistakes can be spotted early on. When systems grow, a well-made model makes it easier to maintain and extend.

How much detail of the real world do you want to include, when designing a model? Often there is a tradeoff between:

- **Simplicity**: The simpler, the easier it is to comprehend. But if it is too simple, you won't be able to spell out the things you want.
- **Expressiveness**: The more details, the more realistic and general purpose the model is. But if it is too generic you can possibly express much more than needed, things that may not have a relation with the reality anymore. Or it carries over too much complexity to be of help in the first place.


> [!WARNING]
> Iets met link (of niet) met domein architectuur. dus geen toegang/sec/landschap, puur rekenmachine.
> TODO: ...

isomorphism. translate problem to formalism.

What are we modelling here? Things we know about the real world. Actual situation or possible ones.
And we that there are things that change this situation or that follow from this situation. 


## Automation strategies


inference: forward & backward.

What does a formal model look like.
language & Editors: aleph like. parsers. coded. lib.

This is background to design principles. We choose pro's and cons.


