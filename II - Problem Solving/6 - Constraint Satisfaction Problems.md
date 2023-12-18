In a constraint satisfaction problem (CSP) states come with a factored representation. Each state is a set of variables, each of which has a value.<br>
A problem is solved when each variable has a value that satisfies all its constraint.
# Definition
A CSP consists of three components.
1. A set of variables $X = \\\{ X_1, ..., X_n \\\}$
2. A set of domains $D = \\\{ D_1, ..., D_n \\\}$
3. A set of constraints $C = \\\{ C_1, ..., C_m \\\}$

Each domain $D_i$ consists of a set of allowable values $\\\{v_1,...v_k\\\}$ for its assigned variable $X_i$.<br>
Each constraint $C_j$ is a pair $\langle scope, relation \rangle$, where the scope defines the variables that participate in the constraint relation; a constraint example may be $\langle (X_1, X_2), X_1 > X_2 \rangle$.<br>
CSPs deal with assignments of value to variables $\\\{X_1=v_1, X_2=v_2, ...\\\}$. An assignment is consistent if it doesn't violate any constraint. An assignment is complete if all variables are assigned a value. An assignment is a solution if it is both consistent and complete.<br>
Note that constraints do not only define what legal assignments are, but they may also be preference constraints: some assignments can be preferred over others by encoding constraints as costs on variable-value pairs. These are constrained optimization problems (that use optimization search methods) that look for the best assignments combinations.
# Inference
Progress in a constraint satisfaction problem can be achieved using constraint propagation: using the constraints to reduce the domain of a variable can reduce the domain of another.<br>
This is the key idea of local consistency, in which the process of enforcing values will spread throughout all variables.<br>
At the end, the problem may not be solved, but the variables will have the number of legal values in the domain drastically reduced.<br>
Problems are visualized as a graph where each node is a variable and each edge is a constraint.<br>
There are different types of local consistency.
## Node consistency
A variable is node-consistent if all the values in its domain satisfy the variable's unary constraint.<br>
When trying to solve a CSP, the first thing to do is to reduce the domain of each variable to make it node-consistent.<br>
Note that CSP solvers usually work only with binary constraints. Thus, the next action to take is to transform n-ary constraints into binary ones.
## Arc consistency
A variable is arc-consistent if every value of its domain satisfies all variable's binary constraints with other variables.
A popular algorithm for enforcing arc consistency is AC-3.
### AC-3
The end goal of AC-3 is to make every variable arc-consistent.<br>
Initially, a queue is built by stacking all arcs of the problem graph. The first $\\\{X_i, X_j\\\}$ pair is popped, and $X_i$ is made arc-consistent with respect to $X_j$ by deleting illegal values from its domain $D_i$.<br>
If $D_i$ is unchanged the algorithm moves on, otherwise we add (or add back) to the queue all arcs $\\\{X_k, X_i\\\}$ where $X_k$ is a neighbor of $X_i$.<br>
If a domain in emptied, AC-3 immediately returns failure, otherwise it proceeds until no more arcs are in the queue. At the end, if all domains contain exactly one value, the problem is furthermore solved.<br>
AC-3 is often implemented because arc-consistent CSPs are faster to search.<br>
The complexity of AC-3, in the worst-case time scenario, is $O(cd^3)$, where $c$ is the number of binary constraints and $d$ is the size of the biggest domain.
## Path consistency
Path consistency tightens the binary constraints by looking at triplets of variables.<br>
A pair $\\\{X_i, X_j\\\}$ is path-consistent with respect to $X_k$ if for every value combination for $X_i$ and $X_j$ there is a value of $X_k$ that satisfies the constraint on $\\\{X_i, X_k\\\}$ and $\\\{X_k, X_j\\\}$.
## K-consistency
A CSP is k-consistent if, for any set of $k-1$ variables, there is a consistent value that can be applied to the last $k$th variable.<br>
K-consistency can be a generalization of previous consistencies:
1. $k=1$ is node consistency;
2. $k=2$ is arc consistency;
3. $k=3$ is path consistency.

A CSP is strongly k-consistent if it also is (k-1)-consistent, and so on all the way down to 1-consistent. In this case, the problem can be solved by going through each variable $X_i$ and looking for a value consistent with all previous variables $\\\{X_1, ..., X_{i-1}\\\}$. Run time is only $O(n^2d)$, where $n$ is the number of variables (hence $k=n$ in k-consistency).
- - -
# Global constraints
A global constraint is one involving an arbitrary number of variables.<br>
A common one is the *Alldiff* constraint, which says that all variables involved must have distinct values. For example, *Alldiff* can be applied to rows, columns and boxes of the Sudoku puzzle.<br>
Another common global constraint is the *Atmost* one, which forbids a set of variables to get values that sum up to a number bigger than an imposed limit.<br>
Lastly, we need to mention that in case of big domains, consistency-checking method are ineffective. In these cases, it's preferred to represent domains with an upper and a lower bound, and gradually narrow that range with bounds propagation techniques in order to get a CSP that is bound-consistent.<br>
A variable $X$ is bound-consistent to $Y$ if, for each value of $X$, there exists a value of $Y$ that satisfies the constraint.
# Backtracking Search
After applying constraint propagation techniques, we often still need to search for a solution, since variables are still able to get multiple values.<br>
In case of partial assignments, we use backtracking search algorithms.<br>
Backtracking searches are based on a [depth-limited](./3%20-%20Solving%20Problems%20by%20Searching.md#depth-limited-search) search where partial assignments are states, and actions extend that assignment. Leaf nodes (at depth $n$) are the solutions.<br>
This algorithm alone is very memory costly, since the number of leaves will be $n!d^n$ even though the possible assignments are only $d^n$.<br>
If we do check for commutativity, that is found in problems where the order of actions is irrelevant, the number of leaves is reduced to $d^n$.<br>
Either way, the preferred way to solve a CSP is to use backtracking: unassigned variables are chose one by one and all values in the domain are tried via recursive calls looking for solutions.<br>
Notice that backtracking search do no store the full tree representation, but keeps a single assignment that is tweaked at each iteration.<br>
Furthermore, domain-independent heuristic can be applied on backtracking searches to make improvements.
## Ordering
A good strategy for choosing the next variable is MRV (**minimum-remaining-values**), which states that a variable with the fewest legal values will cause failure soon, and thus prune the search tree.<br>
In case of tie, a good alternative can be the use of a **degree heuristic**, which prefers variables that have the largest number of constraints over other unassigned ones. This attempts to reduce the branching factor.<br>
Once a variable has been selected, we need to examine its values.<br>
The **least-containing-value** heuristic is effective by preferring values that leave more choice for neighboring variables.<br>
The reason why the selection of variables is fail-first, whilst values' is fail-last, is that every variable is to be assigned in the end, hence pruning at the start promises shorter backtrackings, and since we just need one solution, most likely values grant more probability of success.
## Inference
[AC-3](#ac-3) is efficient in reducing the domains of variables. It is then reasonable to apply it every time we make a choice.<br>
**Forward checking** establishes [arc consistency](#arc-consistency) for a variable $X$ whenever it is assigned.<br>
This can be optimized further if we use the MAC algorithm (**maintaining arc consistency**), which works exactly like AC-3 except it start with a stack of only $\\\{X_j, X_i\\\}$ pairs, where $X_j$ are all the unassigned neighbors of the just assigned $X_i$.
## Locking backward
When a branch of the search fails, **chronological backtracking** rolls back to the most recent decision. This can be further optimized.<br>
For each variable, we can keep track of the other variables that made a value impossible. We do this by building a conflict set. In other words, if an assignment $X=x$ deletes a value from $Y$'s domain, $X=x$ is added to $Y$'s conflict set.<br>
The **conflict-directed backjumping** method backtracks to the most recent assignments in the conflict set.<br>
Important to notice is that every branch pruned by backtracking is also pruned by forward checking, hence the use of both may be redundant.<br>
To explain how new conflict sets are computed, we denote $\textnormal{conf}(X_i)$ the set for variable $X_i$.<br>
If every possible value for the current variable $X_j$ fails, the algorithm backjumps to the most recent variable $X_i$ in $\textnormal{conf}(X_j)$, and the conflict set for $X_i$ is recomputed as follows. $$\textnormal{conf}(X_i)=\textnormal{conf}_{old}(X_i)\cup\textnormal{conf}(X_j)-\\\{X_j\\\}$$
In the end, further optimizations could be acquired with constraint learning, that uses the idea of finding a minimum set (no-good set) of variables that actually cause the problem, from the conflict set.