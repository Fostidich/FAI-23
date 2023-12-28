Planning a course of actions is key for an intelligent agent. With the right factored representation we can represent a wide variety of domains, and with the right algorithms (and their heuristic) we can solve large planning problems.
# Classical planning
Classical planning is defined as the task of finding a sequence of actions to accomplish a goal in a discrete, deterministic, static, fully observable environment.<br>
We do use a factored representation that comes from a family of languages called PDDL (planning domain definition language).<br>
In PDDL, a state is a conjunction of ground atomic fluents, where ground means "without variables", atomic means "with a single predicate" and fluent means "the aspect of the world changes".<br>
The available ground actions are described in an action schema.
$$\begin{align} 
Action(Fly(p,from,t&o), \newline
{\small PRECOND:}\space A&t(p,from)\land Plane(p)\land Airport(from)\land Airport(to) \newline
{\small EFFECT:}\space A&t(p,from)\land At(p,to))
\end{align}$$
The schema consists of the action name, a list of used variables, a precondition and an effect.

$$\begin{align}
&allora \newline
allo&ra \\
\end{align}
$$