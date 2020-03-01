---
title: Adversarial Search
tags: [Artificial Intelligence]
mathjax: true
---

### Game

#### Components

- initial state $s_0$
- states
- players: $PLAYER(s)$ defines which player has the move in state $s$

- actions: $ACTIONS(s)$ returns set of legal moves in state $s$
- transition model: $RESULT(s, a)$ returns state that results from the move $a$ in state $s$
- terminal test $TERMINAL(s)=true$ iff game end
- utility function $UTILITY(s, p)$: final numeric value for a game that ends in terminal state $s$ for a player $p$

### Strategies

#### Player Strategy

A strategy $s$ for player $i$: what will player $i$ do at every node of the tree that they make a move in

#### Winning Strategy

A strategy $s_1^*$ for player 1 is called winning if for any strategy $s_2$ by player 2, the game ends with player 1 as the winner

A strategy $t_1^*$ for player 1 is called non-losing if for any strategy $s_2$ by player 2, the game ends in either a tie or a win for player 1.

### Minimax

$Minimax(s) = max_{a\in Actions(s)}Minmax(Result(s,a))$ if $Player(s)=MAX$

$Minimax(s) = min_{a\in Actions(s)}Minmax(Result(s,a))$ if $Player(s)=MIN$

- MAX chooses move to maximize the minimum payoff
- MIN chooses move to minimize the maximum payoff

| Complete | Yes (if game tree if finite) |
| -------- | ---------------------------- |
| Optimal  | Yes (optimal gameplay)       |
| Time     | $O(b^m)$                     |
| Space    | Like DFS: $O(bm)$            |

Runs in time polynomial in tree size

Returns a sub-perfect Nash equilibrium: the best action at every choice node

##### Drawbacks

Game trees are huge

- $\alpha-\beta$ pruning

Impossible to expand the entire tree

- evaluation functon = estimated expected utility of state
- cutoff test: e.g., depth limit

### $\alpha-\beta$ Pruning

Maintain a lower bound $\alpha$ and upper bound $\beta$ of the values of, respectively, MAX's and MIN's nodes seen thus far

Prune subtrees that will never affect minimax decision

MAX node $n$: $\alpha(n)$ is highest **observed** value found on path from $n$; initially $\alpha(n) = -\infin$

MIN node $n$: $\beta(n)$ is the lowest observed value found on path from $n$; initially $\beta(n)=+\infin$

##### Pruning

- given a MIN node $n$, stop searching below $n$ if there is some MAX ancestor $i$ of $n$ with $\alpha(i) ≥ \beta(n)$

- given a MAX node $n$, stop searching below $n$ if there is some MIN ancestor $i$ of $n$ with $\beta(i)≤\alpha(n)$

### Heuristic Minimax Value

Run minimax until depth d; then start using the evaluation function to choose nodes

#### Evaluation Function

An evaluation function is a mapping from game states to real values: $f: S \rightarrow \R$

$f(s)=UTILITY(s)$ if $TERMINAL(s)$

$f(s)=0$ otherwise, i.e. no information on quality of non-terminal nodes

For non-terminal states, must be strongly correlated with actual chances of winning

#### Cutting Off Search

Modify minimax or $\alpha-\beta$ pruning algorithms by replacing

- TERMINAL(s) with CUTOFF(s, d)
- UTILITY(s) is replaced by EVAL(s)

Can also be combined with iterative deepening