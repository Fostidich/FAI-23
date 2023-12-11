A problem-solving agent searches for a correct sequence of action that reaches the goal state. This can be not immediately obvious, so it is to be planned ahead. Representation used in this case are atomic, and for the environment it's easier to consider it episodic, single agent, fully observable, deterministic, static, discrete and known.<br>
Furthermore, two types of algorithms can be distinguished:
- in the **informed algorithms** the agents is able to estimate how far the goal is;
- in the **uninformed algorithms** the agent has no such estimation.
# Problem-solving agents
Assuming the agent always has access to information about the world, a four-phase process can be followed for problem-solving:
1. the **goal formulation** is useful for limiting objectives and thus actions to be considered;
2. the **problem formulation** wants to provide a mathematical abstract model to describe states and actions necessary to reach the goal. Abstraction is the process of removing not useful details from the real world problem;
3. before any action is taken, a **search** is made by simulating sequences of actions in the model until a solution path reaching the goal is found;
4. when a solution is found, the **execution** of the actions takes place. If the system is "open-loop", actions can be taken blindfolded (ignoring percepts), since the environment won't change; otherwise, if the environment is nondeterministic the percepts have to be monitored, and the system is therefore "closed-loop".

A search problem can be defined formally by providing its components.
- **State space**: this is the set of possible states the environment can be in.
- **Initial state**: it is where the agent does start.
- **Goal states**: these (if they are multiple indeed) are defined in a set, and can be identified by the `isGoal()` method.
- **Actions**: a finite set of applicable actions, given a state, can be retrieved from an `action(s)` method.
- **Transition model**: it describes what each action does. The method `result(s, a)` is to return the state resulting by taking action *a* in state *s*.
- **Action cost function**: denoted by `actionCost(s, a, s')`, which gives the numeric cost of applying action *a* in state *s* to reach state *s'*.

An optimal solution is one that has the lowest path cost among all other solutions.<br>
The state space can be represented as a graph, but the search itself requires other data structures.
# Search algorithms
Search algorithms takes a search problem as input and returns a solution (or a failure). These are algorithms that superimpose a search tree over the state-space graph.<br>
Each node corresponds to a state and each edge corresponds to an action. The root is the initial state.<br>
The search tree may have multiple paths to any given state, but each state has a unique path to the root.<br>
The region of the tree where unexpanded nodes lay is called the frontier. How we decide which node of the frontier is to be expanded next is what defines each and every algorithm itself.
## Search data structure
A node from the tree is represented by a data structure containing:
- `node.state` stores the corresponding state of the node,
- `node.parent` stores the node that generated this node,
- `node.action` is the action that lead from the parent node to this,
- `node.pathCost` is the total cost of the path from the initial state ($g(node)$).

The data structure for the frontier has its appropriate choice in a queue of some kind. The operation it requires are:
- `isEmpty(frontier)` returns true only if no nodes are contained,
- `pop(frontier)` removes the top node and returns it,
- `top(frontier)` returns the top node without removing it,
- `add(node, frontier)` inserts the node in its proper place in the queue.

Three kinds of queues are used in search algorithms:
- a **priority queue** (which first pops the node with minimum cost according to an evaluation function) is used in the best-first search;
- a **FIFO queue** (first in first out) is used in the breadth-first search;
- a **stack** (LIFO queue) is used in the depth-first search.

We have to keep in mind that redundant paths are a thing, since the same node can sometimes be reached from more than a single path. We take in consideration just the one with the minor cost.<br>
But another kind of redundant paths is found in cycles, since the search is often performed in a graph, and so a path can contain a specific node more than just one time. We may have to deal with it.<br>
The best-first search remembers all previously reached states, so that the ones that have already been found are not expanded.<br>
On the other side, sometimes repeating the past can be rare or impossible, so it is legit to not worry about repeating scenarios.<br>
If an algorithm does check redundant paths, we are dealing with **graph searches**, otherwise it is a **tree search**.<br>
Note that the same algorithm can sometimes accept either approaches.<br>
In the end, it is possible to limit our checks to cycles only, by looking for the same node we are extending it its path to the root. If it is already there, we do not expand it.<br>
Finally, we can evaluate an algorithm performance in four ways.
- **Completeness**: is the algorithm guaranteed to find a solution, if there's one?
- **Cost optimality**: does it find a solution with the lowest path cost possible?
- **Time complexity**: how long does it take to find a solution?
- **Space complexity**: how much memory is needed to perform the search?

Furthermore, complexity requires some parameters to be defined:
- $d$ is the depth, or number of actions, in an optimal solution,
- $m$ is the maximum number of actions in any path,
- $b$ is the branching factor, which is the number of successors of a node,
- $|V|$ is the number of vertices in the graph,
- $|E|$ is the number of edges in the graph.
# Uninformed search strategies
An uninformed search algorithm is given no clue about how close a state is to the goal.
## Breadth-first search
- All actions have the same cost.
- When all nodes are expanded, their successors are expanded next, and so on.
- No repetition of nodes is allowed, meaning it's a graph search.
- It is complete even with infinite states.
- The frontier is a FIFO queue.
- It is possible to do an early goal test, meaning the check for a goal node can be done as soon as it is generated.
- It always finds the optimal solution.
- Time and space complexity is $O(b^d)$.
## Best-first search
- It can also be called Dijkstra's algorithm or uniform-cost search.
- Actions have different costs based on an evaluation function $f(n)$; the node with the minimum value is expanded next.
- It checks for redundancies (graph search).
- The frontier is a priority queue.
- It requires a late goal test, therefore the check for goal nodes needs to wait until the node is popped from the queue, so when it is the lowest-cost node that has just been expanded.
- It is complete and cost-optimal, also in infinite spaces.
- Complexity is $O(b^{1 + \lfloor C^\ast/\epsilon \rfloor})$, where $C^\ast$ is the cost of the optimal solution, and where $\epsilon > 0$ is the minimum between all the actions costs.
## Depth-first search
- The deepest node in the frontier is always the next to be expanded. When the search reaches a node with no successors, it backs up to the unexpanded deepest node.
- It is a tree search (it allows duplicates).
- It's not cost-optimal.
- It's complete for finite state spaces, also the ones with cycles (but they can be checked for optimization purposes). It's incomplete for infinite spaces.
- Memory requirements are $O(bm)$, or $O(m)$ if we use the **backtracking search**, which expands just one successor at a time.
## Depth-limited search
- It is a version of the depth-first search where we supply a depth limit $l$, that stops nodes' expansion when reached.
- Time complexity is $O(b^l)$ and space complexity is $O(bl)$.
- If $l$ is too low, the algorithm is incomplete.
## Iterative deepening search
- It is a variant of the depth-limited search, in which we try all values of $l$ iteratively: 0, 1, 2...
- It's optimal for cost-uniform actions and complete if we check for cycles.
- Memory requirements are $O(bd)$ if a solution exists, whereas $O(bm)$ if there's none.
- Time complexity is $O(b^d)$ if a solution exists, whereas $O(b^m)$ if there's none.
- - -
## Bidirectional search
If the cost from each state *s* to *s'* is the same backward, we can simultaneously use a search algorithm both ways, from the initial state and from the goal state. The solution will be then found when the two frontiers collide.
For example, in a breadth-first search, complexity will be $O(b^{d/2}+b^{d/2}) = O(b^{d/2})$, which is much smaller than the unidirectional $O(b^d)$.
# Informed search strategies
Informed (heuristic) search uses hints about the location of goals. These hints come in the form of a heuristic function $h(n)$, that estimates the cost of the cheapest path from the state at node $n$ to a goal state.
## Greedy best-first search
- This search is similar to a best-fist search that has as evaluation function the heuristic function: $f(n) = h(n)$. Thus, it expands first the node with lower $h(n)$.
- The solution found is not optimal in cost.
- Time and space complexity is $O(|V|)$.
## A\* search
- A\* search is a best-first search that uses as the evaluation function the sum of the path cost from the initial node to the current and the estimated cost to a goal state: $f(n) = g(n) + h(n)$. In other words, $f(n)$ represents the estimated cost of the best path that continues from $n$ to a goal.
- An admissible heuristic function is one that never overestimates the cost to reach a goal (it's therefore optimistic).
- A consistent heuristic function can be described by the triangle inequality: a side of a triangle cannot be longer than the sum of the other two sides. A consistent function is also admissible.
- A\* search is always complete.
- If the heuristic function is admissible, A\* is cost-optimal.
- With a consistent function, the optimal path is found when the first goal node is reached, therefore we can do an early goal test.
- With inadmissible heuristic, the search will find the optimal solution only if two scenarios are guaranteed: the heuristic can be admissible just for the optimal path and must not overestimate more than $C^\ast - C_2$ (given $C_2$ the cost of the second optimal solution after $C^\ast$).
- A\* with a consistent heuristic is optimally efficient, in the sense that every other algorithm doing the same search must expand all nodes that A\* surely has expanded. In fact, A\* prunes away loads of unnecessary nodes.
- A\* has a lot of qualities, but it expands a lot of nodes. It surely expands all nodes that have $f(n) < C^\ast$ and some that have $f(n) = C^\ast$. No node with $f(n) > C^\ast$ is expanded.
## Weighted A\* search
- It is a version of A\* search where the evaluation function is $f(n) = g(n) + W \times h(n)$ with $W>1$.
- This algorithm is useful if we accept satisficing solutions (suboptimal ones), found by searches with inadmissible heuristics (that overestimates), so that we can limit memory requirements.
- The solution found will have a cost between $C^\ast$ and $W \times C^\ast$.
- Weighted A\* search can be seen as a generalization of other searches.

> | Search                   | Evaluation              | Weight           |
> |:-------------------------|:-----------------------:|:----------------:|
> | [A\* search](#a-search) | $g(n) + h(n)$ | $W=1$ |
> | [Dijkstra's algorithm](#best-first-search) | $g(n)$ | $W=0$ |
> | [Greedy best-first search](#greedy-best-first-search) | $h(n)$ | $W=\infty$ |
> | [Weighted A* search](#weighted-a-search) | $g(n) + W \times h(n)$ | $1 < W < \infty$ |

- - -
## Bidirectional search
Contrariwise from the uninformed searches, bidirectional informed searches are not always more efficient than unidirectional ones.<br>
Even with admissible heuristic, there's no guarantee that this would lead to an optimal-cost solution.<br>
If the heuristic is very good, adding a bidirectional search does not help much. With an average one, a bidirectional search will expand fewer nodes, so it can be preferred. With a bad heuristic function, the complexity will not change from a unidirectional A*.