---
description: Preventing issues before they reach production
---

# Overview

Aspecto is used in multiple environments, allowing you to gain insights at different stages of the development lifecycle. Here we are focusing on _issue prevention_, which relates strongly to the PR & CI part of the cycle, as shown here:   


![](.gitbook/assets/aspecto_in_prod_circle__copy%20%281%29.png)

After installing Aspecto in staging or production, Aspecto compares what you do locally to the production baseline data, giving you insights early in the development cycle: both when developing locally and when you see the Aspecto report after creating a pull request. 

Some areas in which Aspecto provides insights include:

* Detecting Breaking Changes
* Viewing Dependencies
* Flow Coverage - Which flows from production have run in each PR

Because we have the production data at the PR stage, we can produce these different types of insights. For example, an API change you made that is different than the current state in production, or what dependencies were affected by the changes you have made in your pull request.  
  
We will go into further detail in the next sections.

