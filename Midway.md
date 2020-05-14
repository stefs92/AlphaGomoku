# Some Attempts at Self-Play Reinforcement Learning
## Midway Blog Post - Stefan Stanojevic, Kevin Qualls

In our initial blog post, we wrote about training a neural network on the dataset of Gomoku games, in order to predict the next move a human player would make. Since then, we have taken our project a step further and coded an AI Gomoku player that can gradually improve its skill through self-play and reinforcement learning.

Since we were curious about whether our neural network has actually learned important elements of the game or not, we decided to quickly code a self-playing module and visualize its performance, before fully implementing the AlphaZero algorithm. We used our trained "policy head", giving us the probability distribution over possible moves, to iteratively generate the next move until one of the AI players was in the position to win by connecting 5 tokens. Python's ipywidget library proved to be very useful for visualizing the games, specifically its objects interact and Play. You can see one of the sample games below:

<p align="center">
<img src="https://user-images.githubusercontent.com/31740043/80993579-4c0d2080-8e09-11ea-9149-3533c65e79e7.gif" >
</p>
<p align="center">
  <b>Fig. 5: Sample Game of AI Gomoku Playing Against Itself</b><br>
</p>



Predictably, our AI is pretty bad at playing the game, as it is missing key components - the "value head" evaluating the chance of winning for different board states, as well as the ability to simulate the future. In order to rectify the first problem, we went back to our dataset of human games. Keeping track of the winner of each game, we assigned a score of +1, -1 or 0 (in case of a draw) to each board state of a given game and averaged those out over the dataset. Then, a neural network of the same architecture as in the initial blog post (except the final layer, adopted to the new regression task) is trained to predict the board state value. We obtain a not great, not terrible performance as seen in the plot below:

<p align="center">
<img width="469" alt="Fig 6 final project" src="https://user-images.githubusercontent.com/54907300/81741226-38e2fc00-946c-11ea-9120-3e620d4a3dd2.png">
</p>
<p align="center">
  <b>Fig. 6: Model Performance of Predicting the Winner for Each Game</b><br>
</p>

As we can see, the best validation accuracy is achieved very early in the training process, and there is significant overfitting later on.

At this point, we were in a position to try out a slightly more sophisticated algorithm. Our agent can decide on which move to make considering the value functions of different moves, as well as its probability distribution over the space of moves. The first one is related to "exploitation" of its knowledge, more useful in the later stages of the game, and the second one to "exploration" of possibilities, in theory useful for innovation early in the game. 

We wanted to emulate the AlphaZero algorithm, which worked in the following way. Prior to making each move, simulations of the remainder of the game are made. Since the space of possible games is generally way too large to efficiently cover with a search algorithm, the search is done by looking at a set of the likely games randomly sampled using the probabilities from "policy head" and values from "value head". While AlphaGo does around 1000 simulations at each step, even a much smaller number of simulations looks too computationally intensive for us, with each simulation taking on the order of a minute to finish.


## Adjustments to Consider for Reinforcement Learning Approach

The most important task going forward is to speed up the self-play process by removing inefficiencies in our code and using parallel processing. Learning more about Cuda library seems like a good place to start.

Something else to explore is experimenting with discounted state values for states that are far removed from the end of the game. Authors of AlphaGo assigned +1 to every state in the winning game and -1 to every losing state; however, due to their immense computational power, they were able to generate huge training datasets. With our more limited resources, however, this may not be the best way to go. Something else to consider if we are unable to achieve significant speedups is doing away with the "policy head" entirely and focusing on just the values. In this case, every "simulation" of the game could serve as a training entry to further improve the neural network estimating the value of game states. 

Another idea to play with is trying to incorporate the symmetries of the problem into our neural network, in order to effectively decrease the size of the game configuration space, along the lines of [[8]](https://arxiv.org/pdf/1602.02660.pdf).  Our 15x15 board has reflection symmetries around a horizontal, vertical, and two diagonal axes containing this point. An option to consider if we are unable to construct a neural network that encodes those symmetries is simply enlarging our training sets by applying those symmetry transformations.

## References

1. Rules for Gomoku - http://www.opengames.com.ar/en/rules/Gomoku
2. Shao, Kun & Zhao, Dongbin & Tang, Zhentao & Zhu, Yuanheng. (2016). Move prediction in Gomoku using deep learning. 292-297. 10.1109/YAC.2016.7804906.  - https://www.researchgate.net/publication/312325842_Move_prediction_in_Gomoku_using_deep_learning

3. From-scratch implementation of AlphaZero for Connect4 - https://towardsdatascience.com/from-scratch-implementation-of-alphazero-for-connect4-f73d4554002a

4. Gomoku datasets http://mostovlyansky.narod.ru/iedown.html

5. AlphaGomoku: An AlphaGo-based Gomoku Artificial Intelligence using Curriculum Learning - Zheng Xie, Xing Yu Fu, Jin Yuan Yu, Likelihood Lab, https://arxiv.org/pdf/1809.10595.pdf 

6. Wang, Y. (n.d.). Mastering the Game of Gomoku without Human Knowledge. doi: 10.15368/theses.2018.47

7. Silver, D., Schrittwieser, J., Simonyan, K., Antonoglou, I., Huang, A., Guez, A., … Hassabis, D. (2017). Mastering the game of Go without human knowledge. Nature, 550(7676), 354–359. doi: 10.1038/nature24270

8. Dieleman, S., De Fauw, J., Kavukcuoglu, K. (2016). Exploiting Cyclic Symmetry in Convolutional Neural Networks, https://arxiv.org/pdf/1602.02660.pdf
