---
title: Constraint Satisfication Problems
tags: [Artificial Intelligence]
mathjax: true
---

### Constraint Satisfication Problems

##### Variables

$\vec{X}=X_1,...,X_n;$ each variable $X_i$ has a domain $D_i$

##### Constraints $\vec{C}$

Constraint scope: which variables are involved

constraint relation: what is the relation between them

###### Objective

Find a legal assignment ($y_1,...,y_n$) of values to variables

- $y_i\in D_i$ for all $i \in [n]$

- constrains are all satisfied

##### Binary CSP

Each constraint relates two variables

##### Constraint graph

Nodes are variables, links are constraints

#### CSP Variants

##### Discrete variables

- Finite domains

$n$ variables, domain size $d \rightarrow O(d^n)$ complete assignment

- Infinite domains

integers, strings, etc.

need a constraint language

##### Continuous variables

Linear programming problems(i.e., with linear constraints) can be solved in polynomial time

##### Unary constraints

Single variable

##### Binary constraints

##### Global (i.e., higher-order) constraints

3 or more variables

### Standard Search Formulation (Incremental)

Each state is a partial variable assignment

##### Initial state

The empty assignment

##### Transition function

Assign a **valid** value to an unassigned variable => fail if no valid assignments

##### Goal test

All variables have been assigned

### Backtracking Search

Order of variable assignment is irrelevant

At every level, consider assignments to a single variable

DFS for CSPs with single-variable/level assignments is called backtracking search

##### Improving backtracking efficiency

General-purpose heuristics can give huge time gains

- SELECT-UNASSIGNED-VARIABLE: Which variable should be assigned next
- ORDER-DOMAIN-VALUES: In what order should its values be tried
- INFERENCE: How can domain reductions on unassigned variables be inferred

#### Most Constrained Variable

Choose the variable with fewest legal values

**Minimum-remaining-values (MRV) heuristic**

#### Most Constraining Variable

Choose the variable with most constraints on remaining unassigned variables

**Degree heuristic**

#### Least Constraining Value

Given a variable, choose the value that rules out the **fewest** values for neighboring unassigned variables

### Inference in CSPs

#### Forward Checking

- Keep track of remaining legal values for unassigned variables
- Terminate search when any variable has no legal values

#### Constraint Propagation

Repeatedly locally enforces constraints.

#### Inference in CSPs

Try to infer illegal values for variables by performing constraint propagation

#### Arc Consistency

$X_i$ is arc-consistent wrt $X_j$ (the $arc (X_i, X_j)$ is consistent) iff for every $x \in D_i$ there exists some value $y \in D_j$ that satisfies the binary constraint on the $arc(X_i, X_j)$.

- If $X \leftarrow x$ makes a constraint impossible to satisfy, remove $x$ from consideration

- If $X_i$ loses a value, neighbors of $X_i$ need to be rechecked

  Reducing $D_i$ may result in domain reduction for others.

Arc consistency propagation detects failure **earlier than forward checking**.

Can be run as a preprocessing step or after each assignment.

**Time Complexity**: $O(n^2d^3)$

- CSP has at most $n^2$ directed arcs
- Each $arc(X_i, X_j)$ can be inserted at most $d$ times because $X_i$ has at most $d$ values to delete
- REVISE: Checking the consistency of an arc takes $O(d^2)$ time

It is a 2-consistency, can be extended to k-consistency.

### Maintaining AC

#### Min-conflicts Heuristic

Choose value that violates the fewest constraints

#### CSPs with Binary Constraints as Graphs

If CSP constraint graph is a tree, then we can compute a satisfying assignment (or decide that one does not exist) in $O(nd^2)$ time

##### Step

1. Make parents arc-consistent with children
   - $n$ steps taking $O(d^2)$ time each

2. If step 1 succeeds, assign values from root down
   - $n$ steps taking $O(d)$ time each

Different topological sort possibly give different solutions, but it cannot be that one topo sort returns false and another returns true.

This algorithm cannot count the number of solutions.