Knowledge-based agents use a process of reasoning over an internal representation of knowledge to decide what action to take.<br>
The factored representation that is used is based on propositional logic, which already comes with inference technologies, that are helpful to build a good agent.
# Knowledge-based agents
A knowledge base (KB) is a set of sentences, written in a specific knowledge representation language, that stores information for the agent. Inference can derive new sentences from old ones; initial sentences that are not derived from anywhere are called axioms.<br>
The two main operations for operating with a knowledge base are "tell" and "ask". Each time the agent program is called, it does three things:
1. *tells* the knowledge base what it perceived;
2. *asks* what actions should be taken;
3. *tells* it which action was chosen.

In the use of logic, we need to define the logical representation and reasoning concepts.<br>
If a sentence $\alpha$ is true in a model $m$, we say that $m$ satisfies $\alpha$. $M(\alpha)$ is the set of all models of $\alpha$.<br>
Reasoning on sentences involves logical entailment: the idea that a sentence follows logically from another sentence. $$\alpha \models \beta \iff (M(\alpha) \subseteq M(\beta))$$
The inference algorithm of **model checking** enumerates all possible models to check that $\alpha$ is true in all models in which KB is true: $M(KB) \subseteq M(\alpha)$. If an inference algorithm $i$ can derive $\alpha$ from KB, we write as follows. $$KB \vdash _i \alpha$$
An inference algorithm that derives only entailed sentences is called a sound.<br>
An inference algorithm that can derive any sentence that is entailed is complete.
# Propositional logic
First and foremost, we need to describe the syntax (the structure of a sentence) and the semantics (the way truth is determined) of propositional logic.<br>
Syntax defines allowable sentences. Sentences consists of propositional symbols, which are atomic sentences themselves. With the introduction of logical connectives, we can build complex sentences.<br>
Semantics defines rules for determining the truth of a sentence. A model is nothing more than a combination of truth value for proposition symbols, like the following example. $$m = \\\{A=true, B=false, C=true\\\}$$
In other words, given a set of proposition symbols such as $$A_{1}, A_{2}, B_{1}, B_{2}$$ that can be used to describe our model, we establish sentences that define the semantics of the model we look for. These sentences, that can be labelled for reference, may be $$R_1: A_1 \lor B_2$$ $$R_2: A_2 \iff (B_1 \lor B_2)$$ $$R_3: \neg A_1$$ and they (as the KB) make the model acceptable if they're all true.<br>
Using a model-checking approach, we can check whether $KB \models \alpha$ for some sentence $\alpha$. We enumerate all models with a [depth-first](../II%20-%20Problem%20Solving/3%20-%20Solving%20Problems%20by%20Searching.md#depth-first-search) search and look for a model that satisfies both the requisites ($R_i$) and $\alpha$. If it exists, $\alpha$ is entailed by the KB.<br>
This algorithm is sound and complete, with a time complexity of $O(2^n)$ and a space complexity of $O(n)$, with $n$ the number of symbols in $\alpha$.
# Theorem proving
Entailment can be done by theorem proving, that is, applying rules of inference directly to the sentences in the knowledge base.<br>
Before the explanation of the algorithm, we need to provide some concepts for entailment.<br>
Logical equivalence between two sentences $\alpha$ and $\beta$ is established if they are both true in the same set of models. $$\alpha \equiv \beta \iff (\alpha \models \beta \land \beta \models \alpha)$$
A sentence is valid if it's true in all models (tautologies). We can derive the deduction theorem, that states: $$\alpha \models \beta \iff (\alpha \implies \beta) \textnormal{ is valid.}$$
A sentence is satisfiable if it's true in some model. Satisfiability and validity are connected, in fact: $$\alpha \models \beta \iff (\alpha \land \neg \beta) \textnormal{ is unsatisfiable.}$$
This kind of proof (unsatisfiability) is called "reduction ad absurdum".<br>
To derive a proof, that is, a chain of conclusions that leads to the goal, we use inference rules. The best-known one is *Modus Ponens*, that has the notation $$\frac{\alpha \implies \beta, \space \alpha}{\beta}$$ meaning that given true the two sentences $\alpha \implies \beta$ and $\alpha$, they can be swapped with $\beta$.<br>
Another useful inference rule is *And-Elimination*. $$\frac{\alpha \land \beta}{\alpha}$$
*Modus Ponens* and *And-Elimination* can be used to generate inferences that are sound, without the need for enumerating models.<br>
Another rule can be found in the *Resolution* inference $$\frac{l_1 \lor l_2 \lor l_3, \space \neg l_2}{l_1 \lor l_3}$$ where a false element is removed from a clause (a disjunction of literals).<br>
A final property of logical systems is monotonicity, that states that a set of entailed sentences can only increase with information added to the KB. $$KB \models \alpha \implies (KB \land \beta \models \alpha)$$
Lastly, we mention factoring, that is the obvious removal of multiple copies of literals, for example $A \lor A \equiv A$.<br>
We can now define a proof problem by giving its component:
- **initial state**: the initial knowledge-base;
- **actions**: the inference rules;
- **result**: the substitution made by the inference rule;
- **goal**: the sentence we're trying to prove.

To be able to work with propositional logic algorithms, we need to transform every sentence in a conjunction of clauses (that is always possible by applying logic equivalence formulations). This kind of expression in called a conjunctive normal form (CNF) and it's used as input for the resolution procedure.<br>
## Proof algorithms
The **PL-resolution** algorithm works by contradiction: to show that $KB \models \alpha$ it proves that $KB \land \neg\alpha$ is unsatisfiable.<br>
Initially, $KB \land \neg\alpha$ is converted to CNF, and all the clauses of this conjunction are put in a set; the *Resolution* inference rule is then applied to all elements it contains, repeatedly, forming new clauses that are added to the set. In the end, if no more clauses can be added KB does not entail $\alpha$, otherwise if two clauses resolve to and empty clause the theorem is proven. PL-resolution is complete, as it always find the proof if it exists.<br>
Faster algorithms can be found if inference is made only in Horn clauses (disjunctions that have at most one positive literal) like the forward-chaining and backward-chaining algorithms.<br>
In **forward-chaining**, a single proposition symbol $q$ (the query) is proven from a KB set of definite clauses (disjunctions with a single positive literal).<br>
Starting from positive literals in the KB, if all the premises of an implication are true, the conclusion is added to the set. For example, if $A$ and $B$ are true, and the KB contains $A \land B \implies C$, then $C$ is added to the set. The process continues until $q$ is added (and so proved), or until no further inferences can be made. Forward-chaining is sound and complete.<br>
In **backward-chaining**, the algorithm works in the opposite way of forward-chaining. It looks for the implications in the KB, starting from the conclusion $q$. If one of these implication is found, then $q$ is true.<br>
Both chaining algorithms are linear in time complexity.
## SAT problem: DPLL algorithm
The **DPLL** algorithm (the name comes from the initials of the authors) takes as input a sentence in conjunctive normal form (CNF) as a set of clauses. Essentially, it is a recursive [depth-first](../II%20-%20Problem%20Solving/3%20-%20Solving%20Problems%20by%20Searching.md#depth-first-search) enumeration of possible models.<br>
The efficiency of the algorithm stands in three main improvements.
1. Early termination: a clause is true if any of its literal is true; that means that if a literal is detected as true, the followings literals in the clause can be ignored. The same thing can be applied to conjunction of clauses: if a clause is false, the full sentence is false.
2. Pure symbol heuristic: a symbol is pure if it always appears with the same sign (natural or negated). If a symbol is pure, all models must have its literal set as true, so that all clauses it is in are necessarily true, so they can be pruned.
3. Unit clause heuristic: unit clauses are clauses where all but one literal have been already assigned as false. These clauses can be pruned to contain only the last unknown literal, such that useless branches are pruned on the next iteration (unit propagation).

There can be some other little improvements we can do.
- If all components do not share variables, we can work on each element separately.
- Instead of trying always the true value before false, we can use a degree heuristic to choose first the option that appears more frequently on remaining clauses.
- Conflict cause learning with intelligent backtracking can be preferred over chronological backtracking, by keeping a small set of conflicts.
- Random restarts, over different variables with different values, can prune branches that interfere with the search's progress.
- Clever indexing is useful while indexing structures dynamically.

Finally, we mention the **walkSAT** algorithm.<br>
One every iteration, the algorithm chooses an unsatisfied clause in which it picks a symbol to flip (from true to false, or the other way around). The pick is done randomly between two ways: min-conflict, which choose the variable that minimize the number of unsatisfied clause in the new state, or random walk, that just picks a symbol randomly.<br>
This algorithm is very effective, but if the sentence is unsatisfiable, so no proof exists, the search never terminates.