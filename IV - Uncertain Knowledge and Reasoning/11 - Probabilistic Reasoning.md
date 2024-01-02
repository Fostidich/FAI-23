Conditional independence relationships can make a good representation of a probabilistic world. A systematic way to represent this relationship is with Bayesian networks, in which dependencies are put into a graph.
# Knowledge in uncertain domains
Bayesian network are data structures used to represent dependencies among variables. It is a directed acyclic graph where each node corresponds to a random variable, for which there's the associated probability information $P(X_i|Parents(X_i))$. Therefore, for each variable the probability information is in the form of a conditional distribution given its parents.<br>
The information is stored in a conditional probability table (CPT), one for each variable, in which each row contains the conditional probability of the variable given a combination for truth value of parents.<br>
The Bayes network graph stores the conditional probabilities (that come into knowledge once they've been sampled), but to be able to query an unknown probability we have to define the Bayes semantics, that is the product of all variables.
$$P(x_1,...,x_n)=P(X_1=x_1,...,X_n=x_n)=\prod_{i=1}^{n}P(x_i|parents(X_i))$$
Making a query consists in calculating a conditional probability with the Bayes network's semantics, in which we put the dependent random variable as true (or false) and all the dependencies as true. All other variables are not imposed, so they will be seen in all their combinations.<br>
For instance, if our semantics is
$$P(a,b,c,d,e)=P(a)P(b)P(c|a,b)P(d|c)P(e|c)$$
and we want to query
$$P(b|d,e)$$
we need to impose $b$ as a vector (true and false cases), and put $d$ and $e$ as true.
$$P(b|d,e)=\alpha\sum_a\sum_c P(a)\space\big\langle P(b=T),P(b=F)\big\rangle\space P(c|a,b)P(d=T|c)P(e=T|c)$$
The $\alpha$ serves as a constant for the normalization of the result, since the final vector may not have its components summing out to $1$, but they will just have the proportion correct for true and false cases.<br>
To solve queries, algorithms can be implemented to resolve the product from Bayes semantics.