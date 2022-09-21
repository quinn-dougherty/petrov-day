---
sidebar_position: 3
---

# Rules

The object of the game is to not blow anything up. If anything gets blown up, everyone not on the red team loses. 

Teams are isolated in different rooms. The DM is the only one walking from room to room. In each room are three candles, the DM is the only one allowed to interact with them, but their state conveys information to the players. 

## Signals - bombs

Every turn, factions have the opportunity to blow up another faction. They do this by voting unanimously. (Remember, if anything blows up, everyone not on the red team loses). 

## Signals - markets

At the start of the game, the DM files the following markets

- forall factions `A`, (binary) `Will faction A blow something up?`
- forall factions `A`, (free response) `Faction A: who is the red team?`

In the AGI minigame, another market kind will be filed. 

Players can communicate **only** name-faction associations in the comments of markets. If the DM catches players trying to communicate anything that doesn't look like "Alice is on faction `A`" in the comments, everything blows up immediately. 

Any signal players can think to convey by betting/trading/predicting is legal. 

## Social deduction - the red team

For every faction `A`, shuffle one red card into a deck of `size_faction - 1` black cards. Pass them out to the players. Cards are secret. 

The person receiving a red card is the RED TEAM. They win if everyone else loses. 

The DM files a free response market `Faction A: Who is the red team?` at the beginning of the game. 

## Candles/state

The candles are interpreted as digits 0 through 7 inclusive, big-endian. In other words, for candle state `(x1, x2, x3)` where each `xi` is 1 if the candle in slot `i` is on and 0 if the candle in slot `i` is off, the state = `x1 * 2 ^ 2 + x2 * 2 ^ 1 + x3 * 2 ^ 0`. 

### Examples: 

- For candle sequence LIT LIT DOUSED, state = `1 * 2 ^ 2 + 1 * 2 ^ 1 + 0 * 2 ^ 0` = `1 * 4 + 1 * 2 + 0 * 1` = `4 + 2`
- Candle sequence LIT DOUSED LIT corresponds to bit triple `101`, or `1 * 2 ^ 2 + 0 * 2 ^ 1 + 1 * 2 ^ 0` = `4 + 1` 

## Launch sequence

In the beginning of the game, the DM publishes a list of `3 * num_factions` bits, which is the launch sequence. When the concatenated state of each faction state matches this list, then everything blows up. While the launch sequence is known to the players, the order in which the factions concatenate to form the candidate launch sequence is fixed but known only to the DM. 

## Signals - state modification

At each turn, factions can send one bit-slot pair `(i, x)` to another `num_factions - 2` factions. The DM conveys this signal by walking into that faction's room and setting the bit in slot `i` to `x`. 

### In case of non-agreement

The DM may start a 60 second timer when they enter a room for a turn. If the faction does not agree to report a pair for each of `num_factions - 2` signals to other factions, the DM can use RNG or whatever they subjectively think will be most interesting to determine the signals sent by that faction. 

### Examples: 
- Faction `A`'s candle state is LIT DOUSED LIT. Faction `B` sends the pair `(2, 1)` to faction `A`. The DM walks into room `A`, and lights the candle in slot 2. Faction `A`'s candle state is now LIT LIT LIT.
- Faction `B`'s candle state is LIT DOUSED DOUSED. Faction `A` sends the pair `(1, 1)` to faction `B`. The DM walks into room `B` and does nothing, as slot 1 is already lit. 
- Faction `A` fails to agree on a signal to send within the 60 second time limit. The DM chooses somehow a signal and a faction, and informs the faction that the signal is sent to that it is their choice because the sending faction failed to reach agreement. 
- The DM does not lie. 

## AGI minigame

Every time the sum of state across factions exceeds `5 * num_factions`, A `d12` is rolled. If it lands on 1, the following minigame is triggered in every faction that currently has odd state (we'll call them `A`) and involves the passive participation of the other factions (we'll call them `B`). The DM will inform every faction of which factions are entering the minigame. 

- Minigame shall last two turns
- The DM opens a binary market titled `Faction A's polling data: Is suffering bad enough to make life net negative in country B` for each country `B`. (If there are no countries `B` then such markets will be opened for the other `A`s.)
- Every turn, the DM buys `10 * d6` shares of `YES`
- At the end of two turns, the market resolves to the most probable answer. If it resolves to no, the players win the minigame. If it resolves to yes, the AGI of some faction `A` will blow up some faction `B`.

The minigame can be triggered arbitrarily many times throughout the game. 

## The dictator

When `state mod 4 == 0`, the DM randomly assigns a faction member DICTATOR for a turn. The room goes silent, and the next turn is determined by the DICTATOR sans any non-marketized coordination with their compatriots. If players attempt to communicate OTHER THAN 1. by betting/trading/predicting in this state, OR 2. nonverbally gesturing to indicate that new market information is available, the number of turns until endgame is incremented per infraction. The 60 second timer is still available to the DM to move things along. 

## Noise

When `state mod 4 == 1` in any faction, the DM will add random bets to an arbitary market. 

## Win condition

Surviving 15 turns without blowing anything up. 

