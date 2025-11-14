# CosimoLombardi2031075

##  Interpretations of Probability

Probability has been interpreted in multiple ways, each emphasizing different aspects of uncertainty and randomness:

1. Classical Probability  
   Defined by Laplace, classical probability applies to situations with a finite number of equally likely outcomes. For an event A in a sample space S with n equally likely outcomes, the probability is

   $$
   P(A) = |A| / |S|
   $$

   This approach works well for symmetric systems like dice or cards but fails for non-uniform or infinite sample spaces.

2. Frequentist Probability  
   In the frequentist interpretation, probability is the long-run relative frequency of an event:

   $$
   P(A) = lim (number of times A occurs in n trials) / n  as n → ∞
   $$

   This is experimentally grounded but less meaningful for single-trial events or subjective assessments.

3. Bayesian Probability  
   Bayesian probability interprets probability as a degree of belief, updated via Bayes' theorem:

   $$
   P(A | B) = P(B | A) * P(A) / P(B)
   $$

   This framework accommodates prior knowledge and learning from evidence, unlike frequentist approaches in some scenarios.

4. Geometric Probability  
   Geometric probability generalizes classical probability to continuous spaces. For a measurable region A inside a larger region S:

   $$
   P(A) = measure of A / measure of S
   $$

   This approach works for continuous outcomes but depends on the choice of measure.

5. Axiomatic Probability  
   To resolve inconsistencies among interpretations, Kolmogorov formulated probability axiomatically. A probability space (Ω, F, P) consists of a sample space Ω, a sigma-algebra F, and a probability measure P satisfying:  

   1. P(Ω) = 1  
   2. P(A) ≥ 0 for all A in F  
   3. For any countable sequence of disjoint events A1, A2, ... in F:

      $$
      P(A1 ∪ A2 ∪ A3 ∪ ...) = P(A1) + P(A2) + P(A3) + ...
      $$

   This formalism unifies prior interpretations and handles infinite or non-uniform sample spaces consistently.

##  Relationship Between Probability Theory and Measure Theory

Probability theory can be rigorously formulated using measure theory:

- Sigma-Algebras (F): Collections of events closed under countable unions, intersections, and complements. They define the “measurable” subsets of the sample space Ω.  

- Probability Measures: A function P: F → [0,1] assigning a probability to each event in a sigma-algebra. Axioms ensure consistency and allow handling infinite sample spaces.

- Measurable Functions: Functions X: Ω → R are measurable if the pre-image of any interval is in F. These measurable functions are random variables, connecting abstract probability measures to numerical outcomes.

This framework generalizes from discrete to continuous probabilities and underpins expectation, variance, and stochastic processes.

##  Subadditivity and Inclusion–Exclusion Principle

###  Subadditivity

For any countable sequence of events A1, A2, ..., subadditivity states:

$$
P(A1 ∪ A2 ∪ A3 ∪ ...) ≤ P(A1) + P(A2) + P(A3) + ...
$$

Proof:  

1. Construct disjoint events:  
   $$
   B1 = A1,  B2 = A2 \ A1,  B3 = A3 \ (A1 ∪ A2), ...
   $$  
   Then Bi are disjoint and B1 ∪ B2 ∪ B3 ∪ ... = A1 ∪ A2 ∪ A3 ∪ ...

2. By additivity:  
   $$
   P(A1 ∪ A2 ∪ ...) = P(B1) + P(B2) + P(B3) + ...
   $$

3. Since Bi ⊆ Ai, monotonicity gives P(Bi) ≤ P(Ai). Therefore:  
   $$
   P(A1 ∪ A2 ∪ ...) ≤ P(A1) + P(A2) + P(A3) + ...
   $$

###  Inclusion–Exclusion Principle

For two events A and B:

$$
P(A ∪ B) = P(A) + P(B) - P(A ∩ B)
$$

Proof:  

1. Decompose A and B into disjoint parts:  
   $$
   A = (A \ B) ∪ (A ∩ B),  B = (B \ A) ∪ (A ∩ B)
   $$

2. By additivity for disjoint events:  
   $$
   P(A) = P(A \ B) + P(A ∩ B),  P(B) = P(B \ A) + P(A ∩ B)
   $$

3. Union decomposition:  
   $$
   A ∪ B = (A \ B) ∪ (B \ A) ∪ (A ∩ B)
   $$

4. Additivity gives:  
   $$
   P(A ∪ B) = P(A \ B) + P(B \ A) + P(A ∩ B) = P(A) + P(B) - P(A ∩ B)
   $$

This principle generalizes to n events using the inclusion–exclusion formula.
