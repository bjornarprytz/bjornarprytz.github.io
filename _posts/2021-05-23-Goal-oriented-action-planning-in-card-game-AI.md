---
tags: game-design, game-ai, card-game
---

# Card game AI

I've read an [article on GOAP](https://alumni.media.mit.edu/~jorkin/GOAP_draft_AIWisdom2_2003.pdf) in No One Lives Forever 2. It got me inspired to think about using GOAP for card games. Actions are very discrete and results from those actions can be known. It would be nice to add some risk calculation in there in case of unknown (say, the opponent has mana open and a card in hand). It could get a bit advanced, but maybe the AI can keep a "working theory" of what's in the opponent's hand. Say they play around a card early on. They then "place" that card in the working theory. This working theory should always be updated, and cards should go "stale" if they haven't been played many times when they could have been.

Examples of goals:
    - Keep board parity / superiority
    - Gain card advantage / attrition
    - Win the game
    - Spot an infinite loop (maybe this one isn't particularly fun for the player).

I will try to express how I approach games of magic, the goals I use, and the heuristics I use to reach them.

## Terms

### Agent

The player (me)

### Goal

>A goal is any condition that the agent wants to satisfy. An agent may have any number of goals

Primary (non-negotiable):

- Win: Get the opponent to 0 life
- Win: Deck the opponent
- Not lose: Don't reach 0 life
- Not lose: Don't deck

Board goals:
*The clock* is central - who wins on board will win the game. How confident you are that you can manipulate the board to keep the status quo (where you're ahead).

- Maintain the fastest clock
- Increase the clock
- Slow down the opposing clock

Resource goals:

- Gain card advantage (to have better options)
- Gain a mana advantage (to cast more spells per turn)
- Protect your life total

Evaluation of life as a resource can be tricky. Certainly the last point of life is worth everything, but what about 20? 14? 5? 2? There's probably a function which will give a good heuristic, but it certainly matters if your opponent is likely to have a lightning bolt in their deck.

### Plan

>The plan is simply the name for a sequence of actions. A plan that satisfies a goal refers to the valid sequence of actions that will take a character from some starting state to some state that satisfies the goal.

Plans should be able to span multiple turns, but future turns need to carry some uncertainty

### Actions

>A plan that satisfies a goal refers to the valid sequence of actions that will take a character from some starting state to some state that satisfies the goal.

Actions are the cards you can play, and other game actions like `endTurn()`, which is mandatory when you run out of things to do.

It would be interesting to see if the AI could pass the turn, even if it didn't have to. in order to save a good removal spell for later.

### Plan Formation

>A character generates a plan in real-time by supplying some goal to satisfy to a system called a planner. The planner searches the space of actions for a sequence that will take the character from his starting state to his goal state. This process is referred to as formulating a plan. If the planner is successful, it returns a plan for the character to follow to direct his behavior. The character follows this plan to completion, invalidation, or until another goal becomes more relevant. If another goal activates, or the plan in-progress becomes invalid for any reason, the character aborts the current plan and formulates a new one.

The stack in magic facilitates counter-actions which means that agents could take this uncertainty into account when planning their turn(s). The risk you can take into account is a function of your opponent's:

- Cards in hand (count + known cards)
- Available mana
- Other inferrences (Maybe too advanced for a game AI, remember fun)
  - Likely cards in the deck (based on color, metagame, educated guess).
- Whether or not the risk affects the planning depends on
  - Tactical impact (how much it adversely affects your goals, or advances your opponent's goal).
  - Probability
  - Impendability (can it be avoided? Is it possible to play around?)
- When you play around a card, the working theory is that the card is in the opponent's hand.

- Other reading: 
  - ([medium article](https://medium.com/@vedantchaudhari/goal-oriented-action-planning-34035ed40d0b) not read yet)