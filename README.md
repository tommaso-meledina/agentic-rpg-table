# Agentic RPG Table

## Abstract

This project aims to recreate a TTRPG table, except the GM and the players are LLM-powered AI agents.

It clearly serves no purpose but to give me an excuse to explore the agentic AI landscape.

## Initial sketch

The following is the initial sketch I fed to ChatGPT in order to get some help for laying out the architectural documentation.

> We are going to build an agentic AI solution that plays tabletop roleplaying games. There will be an agent acting as the Game Master (GM) and one or more agents acting as players impersonating their Playing Characters (PCs).

> At a very rudimentary level, one would not need a whole agentic solution to do that: just open up an AI chatbot (ChatGPT, Gemini, Grok, whatever) and ask them to impersonate a GM, do some back-and-forth with them; that will look like playing RPGs alright.

> But what we want here is much more refined. We want every "persona" involved to play the game knowing the rules to the detail. We want the GM to follow a storyline that is roughly laid out, and improvise on those rough strokes based on the actions of the PCs. We want each player to behave independently from each other. We want each player to be aware of the character sheet of their PC, and play accordingly. We want different players to have different personalities. We also want different PCs to have their own "flavour" (as it usually happens in RPGs, each character is uniquely created by the player, with its quirks and all). The GM and the players will interact and take turns just like human players do at a TTRPG table: the GM describes a situation, a player maybe asks for some clarification, another decides to take action, and so on, without a tightly fixed script. When relevant, the GM will ask the players to roll in order to establish whether their actions are successful or not. Moreover, the GM will perform rolls for Non-playing characters (NPCs).

> In order to achieve what I presented above, we will need an agentic solution; each agent will be powered by an LLM, but deterministic operations (such as the logic for rolling dice) will have to be handled deterministically (e.g. with MCP tools). Agents will need access to static resources (e.g. the rulebook of the RPG they have to play, one or more sourcebooks explaining the setting and lore); the GM will have access to restricted additional resources (e.g. one or more sourcebooks laying out the plot of the adventure, the stats of the NPCs, etc).

> We want to take this requirement as a chance to experiment with authentication/authorization. We want to persist a "chronicle" of each session that the agents play, complete with every relevant event that played out in that session. Other than recreating yet another aspect of real-life TTRPG, this will allow our virtual table of roleplaying agents to pick up from where they had left it, i.e. play a number of separate sessions, potentially shutting down the whole system in between sessions.

> We want some sort of "user interface", so that a human can "watch the agents play" in real-time, basically reading the messages they exchange.

> We want to somehow recreate the fact that whatever the GM or a player says at the table, that is heard by everyone else at the table. For now, we will not implement the possibility for the GM to share confidential information with a subset of PCs, but we still want to think of a solution for it - let's say this will be in phase 2. 