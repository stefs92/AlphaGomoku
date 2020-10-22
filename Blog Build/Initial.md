# Understanding the Playing Field  
## Initial Blog Post - Stefan Stanojevic, Kevin Qualls

For this project, we set out a goal of implementing algorithms something akin to Google's DeepMind to devise an AI capable of playing the game of Gomoku at a competitive level. 

Similar to Connect Four, Gomoku is like Connect Five (Go means 5 in Japanese, and moku means pieces [[1]](http://www.opengames.com.ar/en/rules/Gomoku) ), but played on a horizontal Go board. An illustration is shown below (Image adapted from [[2]](https://www.researchgate.net/publication/312325842_Move_prediction_in_Gomoku_using_deep_learning)).

<p align="center">
<img width="249" alt="Screen Shot 2020-04-13 at 8 59 59 PM" src="https://user-images.githubusercontent.com/54907300/79174814-d85b9300-7dc9-11ea-9377-9cc909485ad2.png">
</p>
<p align="center">
  <b>Fig. 1: Gomoku is Played on a 15 x 15 Board</b><br>
</p>


There are ample documentations online for how to build a Connect Four AI, as the game is relatively simple and requires less computational power [[3]](http://www.opengames.com.ar/en/rules/Gomoku). On the other hand, documentation for how to build an AI Gomoku is sparse, likely due to its abstract strategy on a 15 x 15 gameboard setting [[2]](https://www.researchgate.net/publication/312325842_Move_prediction_in_Gomoku_using_deep_learning). Rather than seeing this as a setback, we welcomed the challenge to deepen our understanding of neural networks and machine learning AI.  

Our starting goal was to simply train a neural network to predict the next move a skilled human player would make in Gomoku. In order to do so, we've started with a dataset of competitive games pieced together from different sources on [[4]](http://mostovlyansky.narod.ru/iedown.html), mostly from Russian tournament archives. We've managed to paste together around 11000 games overall. The game entries came in the following form:

1997,382=[marik,om-by,-,88FFFE98798A6A975B4C59999A7BA86C5D5C3C7A4B896BA7B6A99687,?,?]

This entry first specifies the year of the competion, the players and the winner (- corresponds to the second player winning). Then, the long string of penta-decimal numbers specifies the board coordinates of Gomoku moves. The sequence of moves in this game of Gomoku is shown in the following figure. Note that while no player has managed to connect 5 tokens yet, the white has already won by constructing two unrestricteed sequences of 3 tokens (26,4,28 and 28,8,24), intersecting at token 28.


<p align="center">
<img width="249" alt="Screen Shot 2020-04-13 at 8 59 59 PM" src="https://user-images.githubusercontent.com/31740043/79678687-fa8b5180-81cb-11ea-9943-343c38e5bf97.PNG">
</p>
<p align="center">
  <b>Fig. 2: Sequence of Moves in an Example Game</b><br>
</p>

Next, we turned this game string into a sequence of 28 images representing the states of the board at different times during the game. Those would correspond to inputs to our neural network. The output was a single number specifying one of 15^2 = 225 possible next moves. Some additional preprocessing included removing the duplicate board game states from the dataset. This was done by first sorting the list of board states, and then iterating through this dataset and collecting neighboring identical boards. Tokens were one-hot encoded, with (1,0,0) corresponding to first player's token, (0,1,0) corresponding to second player's token and (0,0,1) corresponding to empty space.    

Since this is essentially an image classification task, it makes sense to try to use a convolutional neural network. The neural network architecture we used came from [[2]](https://www.researchgate.net/publication/312325842_Move_prediction_in_Gomoku_using_deep_learning), and took the following form:

![model](https://user-images.githubusercontent.com/31740043/79679151-57d5d180-81d1-11ea-95d3-1f3b453120d3.PNG)
<p align="center">
  <b>Fig. 3: Neural Network of Example Game from Fig 2</b><br>
</p>

This neural network achieved a decent validation accuracy of around 55% pretty quickly, as shown in the following graphs:

<p align="center">
<img width="261" alt="Screen Shot 2020-05-12 at 3 44 43 PM" src="https://user-images.githubusercontent.com/54907300/81739172-dc321200-9468-11ea-896d-1c30eeb4b2dd.png">
</p>
<p align="center">
  <b>Fig. 4: Accuracies and Losses of Example Game</b><br>
</p>


The model is shown to be overfitted, as the accuracies don't sync with one another. Compared to the training data set, the validation data set shows more stability, as the accuracy and loss number keep a relatively horizontal shape. 

## Reinforcement Learning Approach

We have now obtained a neural network capable of imitating human players by predicting their next move, and plan to use this to jump-start the training of our AI Gomoku player using DeepMind's reinforcement learning algorithm. Let us briefly describe how this would work. 

In the language of DeepMind, our model is a "policy head", advising AI which next moves to take under consideration. Another quantity that a player needs to consider is the "value" of board states, roughly the measure of how desirable they are. Our AI player can then perform a number of simulations of possible games guided by its policy and value estimates (something called "Monte Carlo Tree Search"), and decide what move to make based on the result of those simulations. We can thus play out a number of AI vs AI games, and train our neural network on the replays. This results in a very large training set of games between progressively more advanced players, which given sufficient computational power can be used to train our neural network to reach an expert level in Gomoku.

## References

[1] Rules for Gomoku - http://www.opengames.com.ar/en/rules/Gomoku

-- Gives an overview of origins of Gomoku as well as the rules of the game.

[2] Shao, Kun & Zhao, Dongbin & Tang, Zhentao & Zhu, Yuanheng. (2016). Move prediction in Gomoku using deep learning. 292-297. 10.1109/YAC.2016.7804906.  - https://www.researchgate.net/publication/312325842_Move_prediction_in_Gomoku_using_deep_learning

-- Describes methodology of how to predict moves in Gomoku, using a convolutional neural network model. 

[3] From-scratch implementation of AlphaZero for Connect4 - https://towardsdatascience.com/from-scratch-implementation-of-alphazero-for-connect4-f73d4554002a

-- Describes how to implement Google DeepMind's AlphaZero approach for Connect4. Methodology can be applied to Gomoku.

[4] Gomoku datasets http://mostovlyansky.narod.ru/iedown.html

-- Archives datasets of Gomoku games. Data is stored in a .bdt file.

[5] AlphaGomoku: An AlphaGo-based Gomoku Artificial Intelligence using Curriculum Learning - Zheng Xie, Xing Yu Fu, Jin Yuan Yu, Likelihood Lab, https://arxiv.org/pdf/1809.10595.pdf 

-- Shows how to implement curriculum learning - a technique that builds the AI Gomoku's strategy and knowledge of the game through progressively difficult tasks. 
