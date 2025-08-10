# Introduction

This document serves as an introduction to the concepts, design and implementation of our rule system, named MaxRegel.

A rule engine is a software system that makes decisions based on a set of rules. Instead of writing a lot of complex code, you list the rules, and the engine figures out what to do based on the facts it knows.

The overall idea is that complex business logic becomes better maintainable and processes become scalable while guaranteeing correctness.

The organization of this document is as follows:

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

Sometimes you may encounter so-called JSON to express data. In case you are not familiar with this notation, we'll trust you imagination will get across the main idea. Otherwise, it is briefly discussed in an appendix to help that imagination. 

