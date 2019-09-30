# Fully Observable Deterministic, Sequential, Discrete, Static
State: Unique world config, everything you need to know to make decisions.

Goal: The state(s) you want to be in

Problem: We're not at the goal and we don't know how to get 
there.
```
|a

s -a

|a
```

Actions change ur state

## Graph Search probs
Navigation, towers of hanoi (interesting, but there are way better ways for search)

## Setting up Search Probs
You must know...
- What is a state
- What is the initial state
- Goal situation: defines the state/states u wanna be in
- Actions: usually not a list of explicit possible actions, 
 rather rules that dictate them.
- Action Costs(optional):
- Search problem: Find seq of actions to transform state into 
 state where goal situation is recognized
- Solution: Sequence of actions (plan)

### Classes of Search Algs
1) Brute force (uninformed): Try at random until u hit goal state
2) Informed Search: additional knowledge to solve problem

### State space: u know dis
Set of states u can be in

Actions: link the states

Successor function: generates legal new states from old legal states

A = set of actions

Successor(s, A) = {$s_1', s_2'...$}

### Start with Random Search, lets improve
Why does random search suck
- checks the same thing a multiple times
- loops

Generic Search Alg
``` python
ops = {} #actions
closed = None #Everywhere we've been
open = {Initial State} # Places we know exist, but haven't visited (sometimes called fringe)
current = initState
while (not isgoal(current) AND open != None):
    closed = closed + {current}
    open = open - {current} ⨁ (successors(current, ops) - closed)
    current = first(open)

if(isgoal(current))
    return success
else 
    return fail

```

if open is a queue then we have BFS, Stack then DFS, if priority Queue we get Uniform Cost Search or A* based on priority criteria


# Types of Search Algs
Evaluating algs
- Completeness (always finds sol)
- time complexity
- space complexity
- optimality

Completeness
- BFS always complete bc u must search depth $n$ before $n+1$
- DFS complete only if SS is finite.

Time
- BFS: exponential O($b^d$) where b is avg num of successors, d is solution depth
- DFS: exponential O($b^m$) m: max depth of sol found, m can be larger than d.  But on avg DFS better

Space
- BFS keeps all states in memory (exponential)
- DFS: if you're searching through a tree (one parent), then you have linear space recursive alg.  Else exponential, but avg space still better.  

Optimality
- BFS is optimal, basically same reason it's complete
- DFS big nah

## Uniform Cost Search
define g(s) = distance to initial state.

ex)
```
        S
      /   \
     R     F
     |      |
     P     |
      \   /
        B
       /  \
      G    U  
```

Say g(r) = 80, g(f) = 99.  So first we go to R and discover g(p) = 177 so we go back to F.  We continue until we visit goal

Open List: priority queue by g(s) lowest first

1) Priority queue needs to be updated
2) keep track of parents - parents can change
3) wait until you do a visit on the goal (every time you visit something you're certain to have taken the shortest path there)

### Questions on UCS
- Complete: yes - we must look at every state with a g(s) val less than current state (basically you see all the g(6) before g(7) and so on)
- time: exponential
- space: keeps all states in memory
- optimal: yes


## Informed / Heuristic Search
heuristic: "rule of thumb" to help solve problem.  a human intuition

heuristic function: $h: state \rightarrow \mathbb{N}$ to tell us
how good the state is.  The heuristic estimates the state "goodness".  It estimates how far a state is from a goal state, this isn't the actual dist bc then the problem would be solved.

### Greedy Best First:
Open is a priority queue sorted by h(s)

## A*
### Overview:


uses both UCS and greedy best

Simple: sort OL on $f(s) = g(s) + h(s)$

A* optimality:
only if ur heuristic is good

Overestimating h: it makes good states look unappealing

### Other stuff
Admissable heuristic: Only underestimates, this gaurantees optimality.  (manhattan distance is not this)

$f^*(s) = g^*(s) + h^*(s)$
true f, true cost, perfect guess

Admissable heuristic: $h(s) \leq h^*s\ |\ \forall s \in S$

This ensures that we don't make any nodes look bad based on heuristic.

## Heuristics
### 8 puzzle example
- number of misplaced tiles
- sum of manhattan distances of each tile to their home

Why is manhattan better than num misplaced?

Informedness: degree to which h(s) captures insight

informedness = size of total search space/avg # of states explored with h(s)}

Number of misplaced tiles is bad bc the heuristic doesn't change that much, you basically just search aimlessly until you luckily reduce

With manhattan the heuristic changes a bunch

inf(manhattan) > inf(# out of place)


## Measuring Heuristics
- admissable always underestimates: this ensures optimality
- dominance, $h_1$ dominates $h_2$ if $h_2(s_i) \leq h_1(s_i) \leq h*(s_i)$
- consistent: Ur heuristic changes by no more than cost on each 
move, which reduces average time to solve problem

## Coming up with Heuristics
- You can always make an admissable heuristic

Technique: Relax the problem
- Make problem simpler, solve simpler problem in linear time
- then use simple solver as heuristic of original problem

Solve 8 puzzle
Constraints: you can move A to B if
- they are adjacent
- B is blank

Remove constraint 2
So now your heuristic is the manhattan distance of each tile to their home.

Remove both constrains:
now you can move from one place to another so we have num out of place.

Remove constraint 1:
Gashnigs heuristic.  u can move any tile into the open space.

Removing more constraints doesn't necessarily mean better or worse heuristic.

if r() is an exact solution to relaxed prob then r is admissable heuristic to original prob, not necessarily consistent.