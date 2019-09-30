# Intro
Whats new: we got action stochasticity

## Episodic Environments
- choose single action then done
- but there is a range of outcomes for each actor

### Utility theory:
- given some actions $a_1, a_2$ with results $s_i(a_j)$ for i=1 to N
- We can determine which is best based on EU (expected utility)
- $EU(a) = \sum_{i = 1}^{N}P(result_i(a)) * U(result_i(a)))$

Example: lottery or candy bar
EU(lottery) = 1/125M * $1,000,000 + (1-1/125) * -$5 $\approx$ -5

EU(candy) = .999(yum) + 0.001(-5) $\approx$ yum

$yum > -5$ so buy candy

## Sequential Problems
Utility not often clear.  Riedl wants coffee.  Most states 
don't give him coffee so how do we measure.

Consider grid world
```
0   0   0   1
0   x   0   -1
s   0   0   0
```

If we have deterministic actions them boom A* that bish

But if we have 20% of not desired action then we actually want to go UURRR.  But A* values this the same as RRUUR


## Policies
This is a mapping $\pi: S\rightarrow A$

We do not rely on previous states!  This is an MDP
- Once we have policy it's markovian, ofc creating the policy can use past state info.

What we need for an MDP:
- sink states, states where places where process doesn't continue
- initial state
- set of actions
- Transition function: probability of getting to state $s'$ 
from state $s$ if action $a$ is chosen.
- Reward $R(s): s\rightarrow \mathbb{R}$
  


This transition function can be viewed as an $|S|*|S|$ matrix.
Represents prob transition from one to another.

Policy should maximize reward

Reward defines optimality

optimal policy: $\pi* = argmax \sum_{s'\in S}T(s, a, s')*U(s')$

iterate over the actions, for all possible successors, max the prob of getting high utilities

$reward \neq utility$, reward is points for being in state, utility is how much future reward from a given state.

```
x +10(low util state) x    me(higher util)          +1
```


Additive Utility: $U(s_0) = U([s_0, s_1,...,s_n]) = R(s_0) + ... + R(s_n)$, welp this is a hecco, bc we can't sum up to $n = \inf$  So we use discount utility.

Discounted Util: $U(s_0) = U([s_0, s_1, s_2, ..., s_n])$
$U(s_0) = \sum_{k=0}^n \gamma^nR(s_k)$ where $\gamma < 1$.  This allows us to not look super far into the future.

Bellman Equation: $U(S) = R(S) + \gamma Max\ a\in A (\sum_{s' \in S}T(S, a, s')U(s'))$

## Value Iteration
Basically, start with random util for all states except sink states / reward states.  Then update the utils with bellman until they converge.

$U_i(s)$ is the util of state s after i iteration of this process.

Bellman update: $U_{i+1}(s) = R(s) + \gamma max_{a\in A}\sum_{s'\in S}T(s, a, s')U_i(s')$

```
s1     1
s2     s3 
```
Init util of all states $.1$ arbitrarily
Update example:\
$up: (.8)(.1) + (.1)(.1) + (0.1)(1) = 0.19$
$down: (.8)(.1) + (.1)(.1) + (0.1)(1) = 0.19$
$left: (.8)(.1) + (.1)(.1) + (0.1)(.1) = 0.11$
$up: (.8)(1) + (.1)(.1) + (0.1)(.1) = 0.82$

So $s_1$ is updated to be $-0.04 + .5(.82)$ (reward + gamma * expect util)

Make sure: for update_n, don't use the updated values of util for the states you have computed for update_n+1.


## Q Learning (no transition function needed)
- Start in state $s_t$
- pick an action to execute (we'll use a variety of techniques like $\epsilon$ greedy)
- $a_t$ cause you to go to state $s_{t+1}$
- you get $r_{t+1}$

Update Eq
$Q(s_t, a_t) = Q(s_t, a_t) + \alpha(r_{t+1} + \gamma Max(Q(s_{t+1}, a')) - Q(s_t, a_t))$

Update a Q val by making it itself + learning rate(reward you got + gamma* the best advantage you get get from the rest of the states)

If in a sink state you have to...
$Q(s_t, a_t) = Q(s_t, a_t) + \alpha(r_{t+1} - Q(s_t, a_t))$

Imagine
```
s1      s4=10

s2      s3
```
Start all random at s1
no update for if it goes down.  But then left we do
$Q(s, R) = 0 + 0.5(10+0.5xmax(Q(s4, a'))-Q(s1, R)) = 5$
reward at s4 is 10

starting at s_2 up gives 0.5(0+.5*5) = 1.25
then the next trial when we're at s_1 and transition to s4 we get 7.5
trials end when u hit sinks i think

### Overview
Trial and error, do a bunch and see what happens
- you have to take some random actions.  Then u propagate rewards backwards

Must do lots of trials and long trials.  

### Q Table ex)

```
__|__|__|B |__|__|__|__|__|__|__|__|
__|__|__|5 |__|__|__|__|__|__|__|__|
__|__|__|D |__|C |A |10|__|__|__|__|
__|__|__|__|__|__|E |__|__|__|__|__|
st|__|__|__|__|__|__|__|__|__|__|__|
__|__|__|__|__|__|__|__|__|__|__|__|
__|__|__|__|__|__|__|__|__|__|__|__|
__|__|__|__|__|__|__|__|__|__|__|__|
__|__|__|__|__|__|__|__|__|__|__|__|

    L   R   U   D
A   0   2.5   0   0 (say u stumble from A into 10, AR increases)
B   0   0   0   0
D   0   0   0   0
E   0   0   0   0
```

### Right now we're picking actions at random
What can we do better?
Options for picking what to do:
- Random
- On Policy: Always pick greedy, the action where $\pi(s)=max_aQ(s, a)$
  - Initially Q is zero everywhere, but over time this will make for one greedy path and u keep visiting the same things again and again.  You'll see many states close to reward, but could easily get locked into a bad route.
- Epsilon Greedy
  - go optimal $\epsilon$ fraction of the time

### Training loop

```python
Do N trials:
reset sim (agent goes back to start, dont reset Q table lol)
while state s_t is not terminnal and t < thresh
  if random < epsilon
    at = random action
  else
    at= argmax a Q(s, a)
  Execute at, get st+1 and rt+1
  Update Q table
```

Once uve trained
```python
while forever:
    at = argmax Q(s, a)
    execute(a_y)
```

#### Expected behavior
Sigmoid function Trial vs total reward