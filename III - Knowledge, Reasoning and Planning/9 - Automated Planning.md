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
A ground action $a$ is applicable in a state $s$ if $s$ entails the precondition of $a$. The result of taking action $a$ in $s$ is defined as state $s'$, defined as $$ s' = {\small RESULT}(s, a) = (s - {\small DEL}(a)) \cup {\small ADD}(a)$$ where 
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

