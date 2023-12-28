Planning a course of actions is key for an intelligent agent. With the right factored representation we can represent a wide variety of domains, and with the right algorithms (and their heuristic) we can solve large planning problems.
# Classical planning
Classical planning is defined as the task of finding a sequence of actions to accomplish a goal in a discrete, deterministic, static, fully observable environment.<br>
We do use a factored representation that comes from a family of languages called PDDL (planning domain definition language).<br>
In PDDL, a state is a conjunction of ground atomic fluents, where ground means "without variables", atomic means "with a single predicate" and fluent means "the aspect of the world changes".<br>
The available ground actions are described in an action schema.

$$\begin{align} 
Ac&tion(Fly(p,\mathit{from},to), \newline
&{\small PRECOND:}\space At(p,\mathit{from})\land Plane(p) \land Airport(\mathit{from}) \land Airport(to), \newline
&{\small EFFECT:}\space \neg At(p,\mathit{from})\land At(p,to))
\end{align}$$

The schema consists of the action name, a list of used variables, a precondition and an effect.<br>
A ground action $a$ is applicable in a state $s$ if $s$ entails the precondition of $a$. The result of taking action $a$ in $s$ is defined as state $s'$, defined as 
$$s' = {\small RESULT}(s, a) = (s - {\small DEL}(a)) \cup {\small ADD}(a)$$
where 
- $DEL(a)$ is the delete list of $a$, composed of the fluents that appear as negative literals in action's effect,
- $ADD(a)$ is the add list of $a$, composed of the fluents that appear as positive literals in the action's effect.

A set of action schemas serves as a definition for a planning domain; with the inclusion of an initial state and a goal state, we form the full planning problem.<br>
The initial state is a conjunction of ground fluents, and it is introduced with the $Init$ keyword. The goal state is a conjunction of literals (positive and negative) that may contain variables; it is introduced with $Goal$.<br>
With all states, we apply the close-word assumption: any atoms that are not mentioned are false.

$$\begin{align}
Init&(On(A,Table)\land On(B,Table)\land On(C,Table) \\
& \land Block(A)\land Block(B)\land Block(C)) \\
Goal&(On(A,B)\land On(B,C)) \\
Acti&on(Move(b,x,y), \\
& {\small PRECOND:} \space On(b,x) \land Block(b) \land Block(x) \land Block(y) \\
& \hspace{10mm} \land (b \ne x) \land (b \ne y) \land (x \ne y), \\
& {\small EFFECT:} \space On(b,y) \land \neg On(b,x)) \\
Acti&on(MoveToTable(b,x), \\
& {\small PRECOND:} \space On(b,x) \land Block(b) \land Block(x), \\
& {\small EFFECT:} \space On(b,Table) \land \neg On(b,x))
\end{align}$$

The state-space can be searched with algorithms that may look forward for the goal, given the initial state, or backward by looking for the initial state from the goal.<br>
We need to mention a third alternative, that is to translate the problem description into a set of logical sentences, to which apply a logical inference algorithm to find a solution. SAT-based planner such as SATPlan operate by translating a PDDL problem description into propositional form.
## Forward planning search
In forward state-space planning problems, we can look for a solution by applying any of the [heuristic search algorithms](../II%20-%20Problem%20Solving/3%20-%20Solving%20Problems%20by%20Searching.md#informed-search-strategies).<br>
States in this search are ground states where every fluent is either true or false. The goal is a state that has all the positive fluents. The applicable actions in a state are grounded instantiations of the action schemas, so, in every action, variables have all been replaced by constant values.<br>
To determine the applicable actions, the current state is unified with the precondition of each action schema. For each successful result, we apply the substitution. In the end, the resulting states are still ground, since it is a requirement for action schemas to have any variable in the effect appear in the precondition.
## Backward planning search
If we want to search backward from the goal to the initial state, at each step we need to consider relevant actions instead of applicable ones. A relevant action have the effect that unifies with the goal without negating any other part of it.<br>
Given a state $s$ and an action $a$, the regression from $s$ over $a$ (that is, the backward application of $a$ to $s$) gives us a state $s'$.

$$\begin{align}
&\mathit{POS(s')=(POS(s)-ADD(a))\cup POS(Precond(a))} \\
&\mathit{NEG(s')=(NEG(s)-DEL(a))\cup NEG(Precond(a))}
\end{align}$$

This defines the reached state $s'$, in which we find the positive literals of the state description with $POS(s')$ and negatives with $NEG(s')$.<br>
Backward search uses states with variables rather than ground states.
# Nondeterministic planning
When we extend planning to partially observable, nondeterministic and unknown environment we have to represent belief states in which the agent might be in.<br>
We use sensorless (conformant) planning for environment with no observations, contingency planning for partially observable and nondeterministic environments and online planning (or replanning) for unknown environment.<br>
Preconditions and effects now may contain variables that are not part of action's variable list; that was not allowed in fully observable cases.<br>
Furthermore, the PDDL need to be augmented with the percept schema.

$$\begin{align}
Pe&rcept(Color(x, c), \\
& {\small PRECOND:} \space Object(x) \land InView(x)) \\
Pe&rcept(Color(can, c), \\
& {\small PRECOND:} \space Can(can) \land InView(can) \land Open(can))
\end{align}$$

In a fully observable environment we would have all percepts with no preconditions, whereas a sensorless agent would not have any.
## Sensorless planning
A sensorless planning problem can be converted to a belief-state problem in which the transition model is represented by a collection of action schemas, and the belief state is represented by a logical formula.<br>
We then replace the close-world assumption with the open-world one: if a fluent does not appear, its value is unknown instead of false.<br>
Given a belief state $b$, the agent can consider any action whose preconditions are satisfied by $b$. For literals $l$ whose truth value is unknown in $b$ there are three cases:
- if the action add $l$, then it will be true in $b'$;
- if the action deletes $l$, then it will be false in $b'$;
- if the action does not affect $l$, then it will not appear in $b'$.

$$b' = {\small RESULT}(b,a) = (b - {\small DEL}(a)) \cup {\small ADD}(a)$$
This is computed by starting with $b$, setting all atoms in $DEL$ to false and all atoms in $ADD$ to true. Notice that if the belief state starts as a conjunction of literals, then any update yields another conjunction of literals.<br>
We need to also introduce a conditional effect, in the form "*when condition: effect*", where the condition is a logical formula to be compared with the current state, and the effect is the formula describing the resulting state.

$$\begin{align}
Ac&tion(Suck, \\
& {\small EFFECT:} \space \text{when} \space AtL:CleanL \land \text{when} \space AtR:CleanR)
\end{align}$$

It's important to understand the difference between preconditions and conditional effects.<br>
Conditional effects that are satisfied have their effect applied to generate the resulting belief state; if none is satisfied, nothing changes. On the other hand, if a precondition is unsatisfied, the resulting state is undefined.
## Contingent planning
In contingent planning, we avoid ending up in a belief state where the condition formula's truth value is unknown, by using conditional branching based on percepts.

$$\begin{align}
LookAt&(Table),LookAt(Chair), \\
\text{if} \space C&olor(Table,c) \land Color(Chair,c) \space \text{then} \space NoOp \\
\text{else}& \space LookAt(Can), \\
& \text{if} \space Color(Table,c) \land Color(Can,c) \space \text{then} \space Paint(Chair,can) \\
& \text{else if} \space Color(Chair,c) \land Color(Can,c) \space \text{then} \space Paint(Table,can) \\
& \text{else} \space Paint(Chair,can), Paint(Table,can) \\
\end{align}$$

## Online planning
An online planning agent differs from other planning agents by monitoring the environment during plan execution, before taking an action. There can be many approaches for the monitoring.
- Action monitoring: preconditions are verified to still hold.
- Plan monitoring: the remaining plan is verified to still be successful.
- Goal monitoring: the agent checks if there is a better set of goals to be achieved.

At the end, the agent also checks that the plan has succeeded. It also checks for serendipity, that is, the accidental success.<br>
In this kind of planning, we also introduce replanning. Sometimes, when going down a path that may not promise an efficient success, the algorithm should prefer to replan the problem to look for better branches. This is also key if dead ends can be found.<br>
Furthermore, the process of plan-execute-replan can be more efficient and effective than an explicit loop in the plan.<br>
In the end, prediction failure is an opportunity for learning; the agents should modify its model of the world to accord with its percepts. From then on, the replanner will be able to choose the repair sequence that gets to the root problem, instead of relying on luck.