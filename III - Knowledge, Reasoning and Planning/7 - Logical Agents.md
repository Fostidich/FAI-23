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
Reasoning on sentences involves logical entailment: the idea that a sentence follows logically from another sentence. $$\alpha \models \beta \iff M(\alpha) \subseteq M(\beta)$$
The inference algorithm of **model checking** enumerates all possible models to check that $\alpha$ is true in all models in which KB is true: $M(KB) \subseteq M(\alpha)$. If an inference algorithm $i$ can derive $\alpha$ from KB, we write as follows. $$KB \vdash _i \alpha$$
An inference algorithm that derives only entailed sentences is called a sound.<br>
An inference algorithm that can derive any sentence that is entailed is complete.
# Propositional logic
First and foremost, we need to describe the syntax (the structure of a sentence) and the semantics (the way truth is determined) of propositional logic.<br>
Syntax defines allowable sentences. Sentences consists of propositional symbols, which are atomic sentences themselves. With the introduction of logical connectives, we can build complex sentences.<br>
Semantics defines rules for determining the truth of a sentence. A model is nothing more than a combination of truth value for proposition symbols, like the following example. $$m = \\\{X=true, Y=false, Z=true\\\}$$
In other words, given a set of proposition symbols such as $$A_{1}, A_{2}, B_{1}, B_{2}$$ that can be used to describe our model, we establish sentences that define the semantics of the model we look for. These sentences, that can be labelled for reference, may be $$R_1: A_1 \lor B_2$$ $$R_2: A_2 \iff (B_1 \lor B_2)$$ $$R_3: \neg A_1$$ and they (as the KB) make the model acceptable if they're all true.<br>
Using a model-checking approach, we can check whether $KB \models \alpha$ for some sentence $\alpha$. We enumerate all models with a [depth-first](../II%20-%20Problem%20Solving/3%20-%20Solving%20Problems%20by%20Searching.md#depth-first-search) search and look for a model that satisfies the requisites.<br>
This algorithm is sound and complete, with a time complexity of $O(2^n)$ and a space complexity of $O(n)$, with $n$ the number of symbols in $\alpha$.