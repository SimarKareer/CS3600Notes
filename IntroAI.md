# 8/19 Intro to AI
AI: Replicate intelligence with computing

Intelligent: An entity is intelligent if it does something a human would consider intelligent.


## History Lesson
Turing: birth of computing theory.  Like decidability and stuff.
He thinks about AI, but then makes the turing test.

Judge talks to human (pretending to be comp) and comp (pretending to be human) and if the judge
can't tell them apart.


### Problems with Turing Test
Winning bots use deception/deflection

Turing never specified who the judges should be

## Broad VS Narrow AI
Broad: Emulating humans, beating the Turing test etc

Narrow: solving a specific task

## Human vs Superhuman
Human: able to perform as well as avg human

Superhuman: able to perform better than experts

Narrow Superhuman AI is a good thing.

## ML
Automated discover of pattern in data AND acting on it to make decisions

ML is part of AI and data analytics.  The pattern and decisions is what makes it ML

### DL
Type of Ml concerning multilayer Neural Networks


# 8/21 History of AI + Agents
- 1200's: some fake ass writing robot
- 1770: Mechanical Meme (Turk).  Apparently midgets are AI.
- 1880: "Computer" is name for person to do computations.  Babbage tried to make the "analytical engine".  Ava lovelace did a lot of philosophical
thinking here.
- Turing: 1950 turing test
- 1965: Dartmouth coins "Artificial Intelligence"

## Intersection between psych and math
Theres behaviorism

Cognition: input, learning, output


## Modern History
First AI: proof bots, checkers/chess

Knowledge based AI: "dead" - Dr. CS, Rule based systems.

AI ded for a bit

2012: NN's get kewl cuz GPUs.  Do well on imagenet

2019: Accuracy is over 98% where just 10 yrs ago it was 75

## Agents
Agent: can perceive the env through sensors and act using effectors

Agent reads info from sensor -> some box -> effector -> changes env

Agent Function: mathematical description of an agents behavior.  Maps 
sensor data to actions

Agent Program: A concrete implementation of an agent function
- Naive: hash obs sequence to action.  This is... naive cuz it sucks

Good agent programs:
- Objective Function: what agent finna do
- Actions: how actions change world
- sensors: what kinds of percepts
- Prior knowledge: information preloaded into agent

Objective Function: Of all my actions what gets me closer to objective?
Choosing best action based on what i know (depends on agents percepts.  We don't actually know true state, we can only estimate and measure it)

ok so we want to pick "best" action.  Best is based on objective function.
For vampire: minimize blood in ppl, min effort and max blood intake

``` python
if(obstacle)
    doNotCrash()
```

## Hard Agent Problems (Vampire):
What if you lose pts on move?: might have to collect some data then determine

What if you don't have a map

What if sensors aren't perfect, effectors toooooooooooo