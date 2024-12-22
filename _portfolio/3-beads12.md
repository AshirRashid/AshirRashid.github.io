---
title: "Beads 12"
excerpt: "A Digital Board Game with an AI opponent<br/>"
# excerpt: "A Digital Board Game with an AI opponent<br/><img src='/images/500x300.png'>"
collection: portfolio
---

A digital implementation of the board game Beads 12. There is an option for two players to play against each other, or for a single player to play against an AI opponent capable of adapting strategies during game-play.

While developing this AI agent, I experimented with various techniques:
1. Reinforcement Learning: Q-learning - with and without neural networks
2. Minimax Algorithm - Based on Decision Trees

For the final deployment of the game, I chose the Minimax approach since the Minimax approach was simple and required the least computation. For the case of this game, the Minimax algorithm using decision trees with depth = 3 resulted in identical performance with the other approaches.

The game is playable online here: [Beads 12](https://ashir-r.itch.io/beads-12)


Technologies: Python, GDScript

Concepts/Algorithms: Reinforcement Learning, Decision Trees
