

# MaxRegel - Building a Rule Engine from the Ground Up

This document serves as an introduction to the concepts, design and implementation of our rule system, named MaxRegel (See: [max-regel-core](https://github.com/zvasva/max-regel-core) library).

A rule engine is a software system that makes decisions based on a set of rules. Instead of writing a lot of complex code, you list the rules, and the engine figures out what to do based on the facts it knows.

A rule engine helps organizations make decisions automatically and consistently. Instead of relying on people to remember and apply many rules by hand, the engine checks information against clear “if this, then that” instructions. This means decisions can be made faster, with fewer mistakes, and in the same way every time. Another benefit is flexibility: rules can be updated or added without changing the whole system, making it easier to adapt when policies, regulations, or business needs change. Overall, a rule engine saves time, reduces errors, and ensures that important decisions follow the right guidelines.

Rule engines are a great place for multidisciplinary teams, connecting coders and policymakers.
By translating a policy to formal rules, you can validate if an automated process is matching the intended goal. That makes rule engines a communication medium for different roles in an organization.

This document is structured as follows:

* **Background**: covering general information about rule systems.
* **Design**: what goals, non-goals, insight make up the wishlist of our system.
* **Modelling**: structuring the data we need to process, and the processing itself.

Then some implementation notes and further concerns are covered.

The design is presented in a bottom-up fashion. First principles are established on which we can build up more and more functionality.
At the risk of being more tedious, it exposes the way of thinking that went into creating the system.

In a way it also tries to promote an axiomatic, bottom-up, way of approaching such a system.
It shows paying attention to detail early on, making strict choices, saves work later on, and keeping the rigor.

## Audience

A demonstration to designing a formal reasoning system and a way to implement it in software.

This is of interest to anyone that deals with, or creates policies. What makes a good policy?
Can we design it in such a way it can be easily executed by computers?

The actual implementation in software is not covered in depth, so you don't have to be a programmer.



## Installation

```bash
brew install mdbook
brew install mactex

brew install rust
cargo install mdbook-alerts
cargo install mdbook-pandoc --locked
cargo install mdbook-image-size

```

### Build
```bash
mdbook build -d docs
```
