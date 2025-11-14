

# CosimoLombardi2031075 


##  Parallel Between the Two Theories

Measure theory and probability theory share the same structural foundations. Probability theory is simply a special case of measure theory in which the total measure equals 1.

### Basic Components

**Measure Theory**  
- A measurable space is a pair (X, M), where X is a set and M is a sigma-algebra of subsets of X.  
- A measure is a function mu: M → [0, +infinity].  
- The measure satisfies:  
  1. mu(empty set) = 0  
  2. sigma-additivity: if A1, A2, A3, ... are pairwise disjoint, then  
     mu(A1 union A2 union A3 ...) = sum over n of mu(An).

**Probability Theory**  
- A probability space is a triple (Omega, F, P), where Omega is the sample space, F is a sigma-algebra, and P is a probability measure.  
- A probability measure satisfies the same axioms as a general measure, plus the normalization condition:  
  P(Omega) = 1.

### Interpretation Connection

- Set → Event  
- Measure mu(A) → Probability P(A)  
- Integral of a function f → Expected value of a random variable X

Probability is therefore a normalized measure, and all standard results (monotonicity, subadditivity, continuity from above/below) follow from the same axioms.

---

##  Deriving Subadditivity From the Axioms

Subadditivity states that for any sets A and B:

mu(A union B) ≤ mu(A) + mu(B).

This property follows directly from the sigma-additivity axiom.

### Derivation

Define:
- A1 = A  
- A2 = B minus A (the part of B not overlapping with A)

Then A1 and A2 are disjoint, and:

A union B = A1 union A2.

By sigma-additivity:

mu(A union B) = mu(A1) + mu(A2).

Since A2 is a subset of B:

mu(A2) ≤ mu(B).

Therefore:

mu(A union B) ≤ mu(A) + mu(B).

### Finite Subadditivity

For sets A1, A2, ..., An:

mu(A1 union A2 ... union An)  
≤ mu(A1) + mu(A2) + ... + mu(An).

### Countable Subadditivity

For a sequence A1, A2, A3, ...:

mu(union over n of An)  
≤ sum over n of mu(An).

This holds because each union can be decomposed into disjoint parts and sigma-additivity applies.

---

##  Inclusion–Exclusion Principle

Subadditivity provides an inequality. The inclusion–exclusion principle gives an exact formula for the measure of a union by correcting for overlaps.

### Two Sets

For sets A and B:

mu(A union B)  
= mu(A) + mu(B) − mu(A intersect B).

The subtraction removes the double-counted intersection.

### Three Sets

For A, B, and C:

mu(A union B union C)  
= mu(A) + mu(B) + mu(C)  
  − mu(A intersect B)  
  − mu(A intersect C)  
  − mu(B intersect C)  
  + mu(A intersect B intersect C).

### General Finite Case

For sets A1, A2, ..., An:

mu(union of Ai from i = 1 to n)  
= sum of all single measures  
  − sum of all pairwise intersections  
  + sum of all triple intersections  
  − ...  
  + (−1)^(k−1) times the sum of all k-fold intersections  
  + ...  
  + (−1)^(n−1) times the intersection of all n sets.

This expression is fundamental in both combinatorics and probability.

---

##  Interpretation in Probability Theory

The same formulas hold if we replace mu with P:

- P(A union B) = P(A) + P(B) − P(A intersect B)  
- General inclusion–exclusion applies identically  
- Subadditivity becomes:  
  P(union of Ai) ≤ sum of P(Ai)

These principles are applied in evaluating the probability that at least one of several events occurs.


