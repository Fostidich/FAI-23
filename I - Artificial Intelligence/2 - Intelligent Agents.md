A central approach to artificial intelligence comes with the concept of rational agents.
The concept of rationality can be applied to a wide variety of agents operating in whichever environment is imaginable.
Thanks to some design principles it is possible to build successful agents that can be called "intelligent".
# Agents and environments
An agent can be identified as anything perceiving its environment via sensors, and which acts upon that environment through actuators.
The agent's choice of action at any given instant can depend on its complete built-in knowledge and on the entire percept sequence of states observed so far.
Shutout to the concept of function and program: the agent function is the abstract mathematical description, whilst the agent program is a concrete implementation on physical systems.
# The concept of rationality
A rational agent is one that does the right thing. But what is the right thing?
When an agent is deployed in an environment, it generates a sequence of action based on the perceptions it receives. If that sequence is desirable then the agent has performed well.
Often the built agent does have an initial uncertainty, thus the true performance measure will increase over time since the agent has to learn more.
Furthermore, if the geography of the environment is unknown, the agent needs to explore it.
- - -
Rationality has been defined to depend on four specific factors at any given time:
- the performance measure of the success
- the internal knowledge of the agent
- the available actions to do
- the percept sequence of states to date.

Therefore, we can approach a definition:
> for each possible percept sequence, a rational agent should select an action that is expected to maximize its performance measure, given the evidence provided by the percept sequence and whatever built-in knowledge the agent has.

Note that rationality is not omniscience, impossible in reality. Rationality focus on maximizing the expected performance.
There can be environment which are completely known a priori or completely predictable, but this is not an implication for omniscience: the agent does not perceive or learn, but still has to act, even if it simply acts correctly all the time.
Another extent in which to analyze agents, is the designing of these to be autonomous, since partial or incorrect initial knowledge should be integrated by learning.
Another important concept shelters in the ability of an agent to act randomly, both in case of little experience or if loops appears between states.
# The nature of environments
In designing an agent, the first step is always to specify the task environment as fully as possible.
Many properties of this thus arise.
- Fully vs partially observable: a task environment can be fully observable if sensors detects all relevant aspects for taking choices. A partial one may have noise o inaccuracy, or simply lack sensor data. If no sensor is available, environment is then unobservable; note that goals may be achievable even in this case scenario.
- Single vs multiagent: just one agent can be implemented, or many of them can appear in a given scenario. These can be classified by their relationship, in fact, a multiagent environment can be competitive or cooperative. Communication can emerge between agents, or randomized behavior can appear as it is rational to avoid predictability.
- Deterministic vs nondeterministic: if the following state is completely dependent on the current action, we say that the environment is deterministic, otherwise it is not. Nondeterministic and stochastic models found their difference whether probabilities are explicitly dealt with: stochastic environments provides precise probability values for its future possible branches of states, nondeterministic do not.
- Episodic vs sequential: in sequential environment current decisions can affect all future decisions, otherwise it is episodic.
- Static vs dynamic: the environment can change while the agent is deliberating. In case it does, we deal with dynamicity. If it is just the agent's performance score what changes over time, we talk about semidynamic systems.
- Discrete vs continuous: the distinction can occur and apply on the states representation, on the way time is handled or on the percepts and actions of the agent.
- Known vs unknown: this refers to the agent's state of knowledge about the "rules" or "laws" of the environment. It is possible for a known environment to be partially observable, and conversely for an unknown environment to be fully observable.