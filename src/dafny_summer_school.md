# Dafny Summer School Notes

## Chapter 1: Dafny Mechanics

Forgot to take notes (:

## Chapter 2: Specification

“If we’re trying to tease out the protocol in dafny, we should not worry about implementation details or optimization, focus instead on buiulding the simplest correct model of your protocol” -John Howell 


method are imperative and for implementation, whereas functions are **pure** and used for verification (predicates are functions that return the type bool).


**nat, int** are infinite sized/precision object. 
Thus the canonical method of proving properties for fixed size numbers (say *uint32_t*), you can create additional preconditions that the proof ensures no overflow.

Ways to specify what the program should do:
- C-style assertions
- Postconditions
- Properties/invariants
- Refinement: Refining by showing that your system is equivalent to a much simpler system

To prove statements about forall/exists statements, we might need to write **witness statements** as hints to Dafny

**Example:**
```
// Wether or not y divides x
predicate Divides(x: nat, y: nat) 
{
    x % y == 0
}

predicate IsPrime(x: nat)
{
    && candidate > 1
    && forall y: nat | 1 < y < x :: !Divides(x, y)
}

assert !IsPrime(4); // Dafny cannot verify this (or moreso is not convinced)
```
This can instead be fixed by introducing a "triggering assert" (also called a "witness assert")
```
assert Divides(4, 2);
assert !IsPrime(4);
```
For large proofs we hope that witnesses might only be needed in small parts of a proof and that dafny can generate the rest of the proof tree on its own.

**Question:** Ok, but how exactly will we know when we need a witness in these much more complicated systems?

## Lecture 3: State Machines

A **state** is an assignment of values to variables.
An **execution** is a sequence of states.
We're going to capture executions with **state machines**.

**Note**: state machines can't model all possible behaviour (think uncomputable functions)

??triggers tell Dafny to look deeper??

Jay Normal Form: As you begin writing more interesting specs, their proofs will be nontrivial. 
Pull all the nondeterminism into one place and ask for it to give you a receipt (use match construct) which you can then pattern match against when writing your correctness proof.

### New Let Operator:
```
// This let operator tells Dafny to "pick" a book, patron which sattisfies this
// instance of the CheckOut predicate.
// Note: This will cause complaints by Daphny if no such values exist.
var book, patron :| CheckOut(v, v', book, patron);
```







