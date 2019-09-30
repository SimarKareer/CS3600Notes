# Environments 8/26/19
## Observability
Fully observed: you know the true states

Partially observed: you only have observations which are fallible
- Incomplete data and noise

## Determinism
Deterministic: world changes as desired

Stochastic: Randomness, uncertain effectors (action uncertainty) and 
sensors

A partially observable world is stochastic (bc incomplete data is in a sense random too)

Hidden information counts as sensor stochastic.  So poker is action deterministic, sensor stochastic.

## Static
Static: world doesn't change while you're making decision

Dynamic: world changes constantly or at any time

## Discrete
Discrete: World in finite pieces

Continuous: infinite pieces

These apply to time, actions, percepts

## Episodic
Episodic: History doesn't matter

Sequential: History does matter

## Agent
Single Agent

Multiagent: other autonomous agents
- cooperative and/or competetive


## Example - Self Driving Cars
Partially observable, Stochastic, Sequential bc there's 
planning.  Actions you choose effect future actions, Dynamic, 
Continuous, Cases to call it multi or single.

## Stages of Environments:
- F.O Deterministic
- F.O Action stochastic
- P.O
- Optimization / ML

