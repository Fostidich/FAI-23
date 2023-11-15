<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Problem solving agents](#problem-solving-agents)
- [Search algorithms](#search-algorithms)
  - [Search data structure](#search-data-structure)
  - [Best-first search](#best-first-search)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

A problem solving agent searches for a correct sequence of action that reaches the goal state. This can be not immediately obvious so it is to be planned ahead. Representation used in this case are atomic, and for the environment it's easier to consider it episodic, single agent, fully observable, deterministic, static, discrete and known.
Furthermore, two types of algorithms can be distinguished:
- in the **informed algorithms** the agents is able to estimate how far the goal is;
- in the **uninformed algorithms** the agent has no such estimation.
# Problem solving agents
Assuming the agent always has access to information about the world, a four-phase process can be followed for problem solving:
1. the **goal formulation** is useful for limiting objectives and thus actions to be considered;
2. the **problem formulation** wants to provide a mathematical abstract model to describe states and actions necessary to reach the goal. Abstraction is the process of removing not useful details from the real world problem;
3. before any action is taken, a **search** is made by simulating sequences of actions in the model until a solution path reaching the goal is found;
4. when a solution is found, the **execution** of the actions takes place. If the system is "open-loop", actions can be taken blindfolded (ignoring percepts), since the environment won't change; otherwise, if the environment is nondeterministic the percepts have to be monitored, and the system is therefore "closed-loop".

A search problem can be defined formally by providing its components.
- **State space**: this is the set of possible states the environment can be in.
- **Initial state**: it is where the agent does start.
- **Goal states**: these (if they are multiple indeed) are defined in a set, and can be identified by the `isGoal()` method.
- **Actions**: a finite set of applicable actions, given a state, can be retrieved from an `action(s)` method.
- **Transition model**: it describes what each action does. The method `result(s, a)` is to return the state resulting by taking action 'a' in state 's'.
- **Action cost function**: denoted by `actionCost(s, a, s')`, which gives the numeric cost of applying action 'a' in state 's' to reach state 's''.

An optimal solution is one that has the lowest path cost among all other solutions.
The state space can be represented as a graph, but the search itself requires other data structures.
# Search algorithms
Search algorithms takes a search problem as input and returns a solution (or a failure). These are algorithms that superimpose a search tree over the state-space graph.
Each node corresponds to a state and each edge corresponds to an action. The root is the initial state.
The search tree may have multiple paths to any given state, but each state has a unique path to the root.
The region of the tree where not expanded nodes lay is called the frontier.
How we decide which node of the frontier is to be expanded next is what defines each and every algorithm itself.
## Search data structure
A node from the tree is represented by a data structure containing:
- `node.state` stores the corresponding state of the node,
- `node.parent` stores the node that generated this node,
- `node.action` is the action that lead from the parent node to this,
- `node.pathCost` is the total cost of the path from the initial state (`g(node)`).

The data structure for the frontier has its appropriate choice in a queue of some kind. The operation it requires are:
- `isEmpty(frontier)` returns true only if no nodes are contained,
- `pop(frontier)` removes the top node and returns it,
- `top(frontier)` returns the top node without removing it,
- `add(node, frontier)` inserts the node in its proper place in the queue.

Three kinds of queues are used in search algorithms:
- a **priority queue** (which first pops the node with minimum cost according to an evaluation function) is used in the best-first search;
- a **FIFO queue** (first in first out) is used in the breadth-first search;
- a **stack** (LIFO queue) is used in the depth-first search.

We have to keep in mind that redundant paths are a thing, since the same node can sometimes be reached from more than a single path. We take in consideration just the one with the minor cost.
But another kind of redundant paths is found in cycles, since the search is often performed in a graph, and so a path can contain a specific node more than just one time. We may have to deal with it.
The best-first search remembers all previously reached states so to not expand the ones that have already been found.
On the other side, sometimes repeating the past can be rare or impossible, so it is legit to not worry about repeating scenarios.
If an algorithm does check redundant paths, we are dealing with **graph searches**, otherwise it is a **tree search**.
Note that the same algorithm can sometimes accept either approaches.
In the end, it is possible to limit our checks to cycles only, by looking for the same node we are extending it its path to the root. If it is already there, we do not expand it.
Finally, we can evaluate an algorithm performance in four ways.
- **Completeness**: is the algorithm guaranteed to find a solution, if there's one?
- **Cost optimality**: does it find a solution with the lowest path cost possible?
- **Time complexity**: how long does it takes to find a solution?
- **Space complexity**: how much memory is needed to perform the search?

Furthermore, complexity requires some parameters to be defined:
- *d* is the depth, or number of actions, in an optimal solution,
- *m* is the maximum number of actions in any path,
- *b* is the branching factor, which is the number of successors of a node.
## Best-first search
A very simple approach is the one that chooses the following node to expand based on an evaluation function `f(n)`.
Each node given as input returns a value; the node with the minimum value is the one that is chosen next.