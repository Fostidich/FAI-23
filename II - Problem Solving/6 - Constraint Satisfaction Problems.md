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
Note that constraints do not only define what legal assignments are, but they may also be preference constraints: some constraints may be encoded as costs on variable-value pairs, so that some assignments can be identified as preferred over others. These are constrained optimization problems (that use optimization search methods) that look for the best assignments combinations.
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
If $D_i$ is unchanged the algorithm moves one, otherwise we add (or add back) to the queue all arcs $\\\{X_k, X_i\\\}$ where $X_k$ is a neighbor of $X_i$.<br>
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