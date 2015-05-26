[TOC]

# Logic

## Propositional Logic

Logic symbols:

1. $\lnot$: \lnot
2. $\land$: \land
3. $\lor$: \lor
4. $\oplus$: \oplus
5. $\Rightarrow$: \Rightarrow
6. $\Leftrightarrow$: \Leftrightarrow
7. $\exists$: \exists
8. $\forall$: \forall
7. $\rightarrow$: \rightarrow
8. $\leftrightarrow$: \leftrightarrow
10. $\vdash$: \vdash, entails
11. $\vDash$: \vDash, models


A proposition is a declarative sentence (a sentence that declares a fact) that is either true or false, but not both.

Propositional variables (statement variables): variables that represent propositions

Propositional calculus or propositional logic: the area of logic that deals with propositions

Compound propositions: new propositions that areformed from existing propositions using logical operators

Note that in logic the word “**but**” sometimes is used instead of “**and**” in a conjunction. For example, the statement “The sun is shining, but it is raining” is another way of saying “The sun is shining and it is raining.”

The use of the connective or in a disjunction corresponds to one of the two ways the word
or is used in English, namely, as an **inclusive or**, eg. “Students who have taken calculus or computer science can take this class.”

The **exclusive or** of p and q, denoted by $p \oplus q$, is the proposition that is true when exactly one of p and q is true and is false otherwise.

$p \rightarrow q$: implication, "if $p$ then $q$", is false when $p$ is true and $q$ is false, and true otherwise. Other ways to express implication:

- “if $p$, then $q$” 
- “$p$ implies $q$”
- “if $p$, $q$” 
- “$p$ only if $q$”
- “$p$ is sufficient for $q$” 
- “a sufficient condition for $q$ is $p$”
- “$q$ if $p$” 
- “$q$ whenever $p$”
- $q$ when $p$” 
- “$q$ is necessary for $p$”
- “a necessary condition for $p$ is $q$” 
- “$q$ follows from $p$”
- “$q$ unless $\lnot p$”

A useful way to understand the truth value of a conditional statement is to think of an obligation or a contract. For example, the pledge many politicians make when running for office is

“If I am elected, then I will lower taxes.”

It is only when the politician is elected but does not lower taxes that voters can say that the politician has broken the campaign pledge.

- converse: $q \rightarrow p$ is called the converse of $p \rightarrow q$
- contrapositive: the contrapositive of $p \rightarrow q$ is $\lnot q \rightarrow \lnot p$
- inverse: the inverse of $p \rightarrow q$ is $\lnot p \rightarrow \lnot q$

Contrapositive always has the same truth value as $p \rightarrow q$.

When two compound propositions always have the same truth value we call them **equivalent**.

Biconditional/bi-implication statement: $p \leftrightarrow q$ is true when $p$ and $q$ have the same truth values, and is false otherwise. Note that the statement $p \leftrightarrow q$ is true when both
the conditional statements $p \rightarrow q$ and $q \rightarrow p$ are true and is false otherwise. Other expressions are:

- "$p$ if and only if $q$"
- “$p$ is necessary and sufficient for $q$”
- “if $p$ then $q$, and conversely”
- “$p$ iff $q$”

You should be aware that biconditionals are not always explicit in natural language. In particular, the “if and only if” construction used in biconditionals is rarely used in common language. Instead, biconditionals are often expressed using an “if, then” or an “only if” construction. The other part of the “if and only if” is implicit. That is, the converse is implied, but not stated. For example, consider the statement in English“If you finish your meal, then you can have dessert.”

