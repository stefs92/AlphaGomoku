# Title
## Midway Blog Post - Stefan Stanojevic, Kevin Qualls

In our initial blog post, we wrote about training a neural network on the dataset of Gomoku games, in order to predict the next move made a human player would make. Since then, we have taken our project a step further and coded an AI Gomoku player that can gradually improve its skill through self-play and reinforcement learning.

Since we were curious about whether our neural network has actually learned important elements of the game or not, we decided to quickly code a self-playing module and visualize its performance, before fully implementing the AlphaZero algorithm. We used our trained "policy head", giving us the probability distribution over possible moves, to iteratively generate the next move until one of the AI players connected 5 tokens.


