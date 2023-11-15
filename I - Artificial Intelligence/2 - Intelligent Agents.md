<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Agents and environments](#agents-and-environments)
- [The concept of rationality](#the-concept-of-rationality)
- [The nature of environments](#the-nature-of-environments)
- [The structure of agents](#the-structure-of-agents)
  - [Simple reflex agents](#simple-reflex-agents)
  - [Model-based reflex agents](#model-based-reflex-agents)
  - [Goal-based agents](#goal-based-agents)
  - [Utility-based agents](#utility-based-agents)
  - [Learning agents](#learning-agents)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

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

Rationality has been defined to depend on four specific factors at any given time:
- the performance measure of the success,
- the internal knowledge of the agent,
- the available actions to do,
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
- **Fully vs partially observable**: a task environment can be fully observable if sensors detects all relevant aspects for taking choices. A partial one may have noise o inaccuracy, or simply lack sensor data. If no sensor is available, environment is then unobservable; note that goals may be achievable even in this case scenario.
- **Single vs multiagent**: just one agent can be implemented, or many of them can appear in a given scenario. These can be classified by their relationship, in fact, a multiagent environment can be competitive or cooperative. Communication can emerge between agents, or randomized behavior can appear as it is rational to avoid predictability.
- **Deterministic vs nondeterministic**: if the following state is completely dependent on the current action, we say that the environment is deterministic, otherwise it is not. Nondeterministic and stochastic models found their difference in whether probabilities are explicitly dealt with: stochastic environments provides precise probability values for its future possible branches of states, nondeterministic do not.
- **Episodic vs sequential**: in sequential environment current decisions can affect all future decisions, otherwise it is episodic.
- **Static vs dynamic**: the environment can change while the agent is deliberating. In case it does, we deal with dynamicity. If it is just the agent's performance score what changes over time, we talk about semidynamic systems.
- **Discrete vs continuous**: the distinction can occur and apply on the states representation, on the way time is handled or on the percepts and actions of the agent.
- **Known vs unknown**: this refers to the agent's state of knowledge about the "rules" or "laws" of the environment. It is possible for a known environment to be partially observable, and conversely for an unknown environment to be fully observable.
# The structure of agents
The end goal of AI is to design an agent program that reflects and implements the agent function.
We can outline four basic kinds of agent programs:
1. [simple reflex agents](#simple-reflex-agents),
2. [model-based reflex agents](#model-based-reflex-agents),
3. [goal-based agents](#goal-based-agents),
4. [utility-based agents](#utility-based-agents).
## Simple reflex agents
This is the simplest kind of agent. The actions are just selected on the basis of the current percept, ignoring the history.
This simple connection between perception and behavior is called condition-action rule.
Often, infinite loops of actions are unavoidable for this agent when operating in partially observable environments, unless randomization can come into play.
## Model-based reflex agents
Here, the agent does maintain some sort of history, in fact, it stores an internal state that depends on previous percepts. This history thereby reflects on some of the unobserved aspects of the current state.
The stored information requires two kinds of knowledge, defined in two different models.
In a transitional model, data is retrieved by analyzing how the world changes over time, focusing on the effects on the agent's actions and on the world evolution, independently.
In a sensor model, data comes from how the state of the world is reflected on the agent's percept.
Altogether, transitional and sensor model defines the model-based agent.
Regardless of the representations used, if the current state is in a partially observable environment, the agent will try the "best guess".
## Goal-based agents
As well as the current state description, the agent will need some kind of goal information, which is used to describe if the state is desirable.
Finding the action sequences that achieves the goal is the focus of the AI subfields for planning and searching.
Decision making here is very different from a condition-action system, since consideration of the future if fundamental.
On the other side, a goal-based agent's behavior is more flexible since changing the goal does not imply changing the whole rule set.
## Utility-based agents
A performance measure for any given sequence of states can be used to assign scores, so to have a distinction between more and less desirable ways of getting to the goal.
This task is defined by an utility function.
After all, maximizing the utility choosing the right actions is rational.
- - -
## Learning agents
Learning allows the agent to operate in initially unknown environments and to become more competent than its initial knowledge alone might have allowed.
A learning agent can be divided into four conceptual components:
- **a learning element** is responsible for making improvements;
- **a performance element** is responsible for selecting external actions, in particular it takes the percepts and decides the actions;
- **a critic** is used to get feedback for the learning element, and thus how the performance elements should be tweaked to do better. This is necessary because the percepts themselves do not provide indication of success to the agent;
- **a problem generator** is used to suggest actions leading to new and informative experiences, since explore a little and trying suboptimal paths will lead to much better decisions for the long run.

The performance standard distinguishes part of the incoming percept as a reward or a penalty to provide direct feedback on the quality of the agent behavior.
Learning in intelligent agents can be summarized as a process of modification of each component with the ultimate goal of improving the overall performance.