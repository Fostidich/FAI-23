A problem solving agent searches for a correct sequence of action that reaches the goal state. This can be not immediately obvious so it is to be planned ahead. Representation used in this case are atomic, and for the environment it's easier to consider it episodic, single agent, fully observable, deterministic, static, discrete and known.
Furthermore, two types of algorithms can be distinguished:
- in the **informed algorithms** the agents is able to estimate how far the goal is;
- in the **uninformed algorithms** the agent has no such estimation.
# Problem solving agents
Assuming the agent always has access to information about the world, a four-phase process can be followed for problem solving:
1. the **goal formulation** is useful for limiting objectives and thus actions to be considered;
2. the **problem formulation** wants to provide a mathematical abstract model to describe states and actions necessary to reach the goal. Abstraction is the process of removing not useful details from the real world problem;
3. before any action is taken, a **search** is made by simulating sequences of actions in the model until a solution path reaching the goal is found;
4. when a solution is found, the **execution** of the actions takes place. If the system is "open-loop", actions can be taken blindfolded (ignoring percepts), since the environment wont change; otherwise, if the environment is nondeterministic the percepts have to be monitored, and the system is therefore "closed-loop".

A search problem can be defined formally by providing its components.
- **State space**: this is the set of possible states the environment can be in.
- **Initial space**: it is where the agent does starts.
- **Goal states**: these (if they are multiple indeed) are defined in a set, and can be identified by the `isGoal()` method.
- **Actions**: a finite set of applicable actions, given a state, can be retrieved from an `action(s)` method.
- **Transition model**: it describes what each action does. The method `result(s, a)` is to return the state resulting by taking action 'a' in state 's'.
- **Action cost function**: denoted by `actionCost(s, a, s')`, which gives the numeric cost of applying action 'a' in state 's' to reach state 's''.

An optimal solution is one that has the lowest path cost among all other solutions.
The state space can be represented as a graph, but the search itself requires other data structures.
## Search algorithms