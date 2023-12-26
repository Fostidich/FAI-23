Propositional logic is limited in what it can say. Thus, we introduce first-order logic, which can concisely represent much more, and which can be the starting point for inference and logic implementation for knowledge-based agents.
# Syntax and semantics
In first-order logic, we use models. The domain of a model is the set of objects it contains. These are required to be non-empty.<br>
The objects of a model may be related; certain kinds of relationships are best considered as functions. Models in first-order logic require total functions, that is, there must be a value for every input.<br>
The basic syntactic elements are:
- constant symbols, which stand for objects;
- predicate symbols, which stand for relations;
- function symbols, which stand for functions.

In the end, on an implementation side, because the number of first-order models is unbounded, we cannot check entailment by enumeration (as we do for propositional logic). Thus, we must look for different strategies.
## Example
Sentences are added to a knowledge base using $TELL$. Such sentences are called assertions.
$$TELL(KB, King(Jhon))$$
$$TELL(KB, Person(Richard))$$
$$TELL(KB, \forall x \space King(x) \implies Person(x))$$
We can ask questions of the knowledge base using $ASK$. These kinds of interrogations are called queries.
$$ASK(KB, King(Jhon))$$
$$ASK(KB, Person(Jhon))$$
$$ASK(KB, \exists x \space Person(x))$$
The last query in the example may not be very useful, since it just returns a boolean such as $true$. To acknowledge which values for $x$ make the sentence $true$ we need to use the $ASKVARS$ function.
$$ASKVARS(KB, Person(x))$$
The return type of this query is a substitution, or binding list. For the example, the answer will be $\\\{x/Jhon\\\}$ and $\\\{x/Richard\\\}$.<br>
The knowledge base needs to contain only axioms to be useful. A special and fundamental kind of axioms are definitions, which come in the form $\forall x,y \space P(x, y) \iff \dots$.
$$\forall m,c \space Mother(c) = m \iff Female(m) \land Parent(m,c)$$
$$\forall p,c \space Parent(p,c) \iff Child (c,p)$$
$$\forall g,c \space Grandparent(g,c) \iff \exists p \space Parent(g,p) \land Parent(p,c)$$
$$\forall x,y \space Sibling(x,y) \iff x \neq y \land \exists p \space Parent(p,x) \land Parent(p,y)$$
Not all logical sentences about domains are axioms. Some are theorems, and they are entailed by the axioms. They may look like definitions, but they can actually be derived from the knowledge base.
$$\forall x,y \space Sibling(x,y) \iff Sibling(y,x)$$
As we said, theorems are not required to be in the KB, but they can reduce the computational costs by a lot.