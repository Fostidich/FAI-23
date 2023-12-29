An agent may never know for sure what state it is in or where it will end up after a sequence of actions. In the real world, agents must deal with uncertainty.
# Probability
A logical agent believes each sentence to be either true or false, or it has no opinion on it. On the other hand, a probabilistic agent may have a numerical degree of belief (between 0 and 1).<br>
Here, the set of all possible worlds is called the sample space ($\Omega$). The probability model of the sample space is defined by the probabilities of its elements $\omega$.
$$1\le P(\omega)\le 1\hspace{20mm} \sum_{\omega \in \Omega}P(\omega)=1$$
We can introduce conditional probabilities.
$$P(a|b)=\frac{P(a\land b)}{P(b)}$$
Another well-known formula is the inclusion-exclusion principle.
$$P(a\lor b)=P(a)+P(b)-P(a\land b)$$
If two events $a$ and $b$ are independent, we can write the following.
$$P(a|b)=P(a)\hspace{10mm}P(b|a)=P(b)\hspace{10mm}P(a\land b)=P(a)P(b)$$
From the conditional probabilities we can then derive the Bayes' rule.
$$P(b|a)=\frac{P(a|b)P(b)}{P(a)}$$