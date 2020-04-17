
# Initial Blog Post
## Stefan Stanojevic, Kevin Qualls

For this project, we set out a goal of implementing algorithms something akin to Google's DeepMind to devise an AI capable of playing the game of Gomoku at a competitive level. 

Similar to Connect Four, Gomoku is like Connect Five (Go means 5 in Japanese, and moku means pieces [[1]](http://www.opengames.com.ar/en/rules/Gomoku) ). An illustration is shown below (Image adapted from [[2]](https://www.researchgate.net/publication/312325842_Move_prediction_in_Gomoku_using_deep_learning)).

<p align="center">
<img width="249" alt="Screen Shot 2020-04-13 at 8 59 59 PM" src="https://user-images.githubusercontent.com/54907300/79174814-d85b9300-7dc9-11ea-9377-9cc909485ad2.png">
</p>


There are ample documentations online for how to build a Connect Four AI, as the game is relatively simple and requires less computational power [[3]](http://www.opengames.com.ar/en/rules/Gomoku). On the other hand, documentation for how to build an AI Gomoku is sparse, likely due to its abstract strategy on a 15 x 15 gameboard setting [[2]](https://www.researchgate.net/publication/312325842_Move_prediction_in_Gomoku_using_deep_learning). Rather than seeing this as a setback, we welcomed the challenge to deepen our understanding of neural networks and machine learning AI.  

Our starting goal was to simply train a neural network to predict the next move a skilled human player would make in Gomoku. In order to do so, we've started with a dataset of competitive games pieced together from different sources on [5], mostly from Russian tournament archives. We've managed to paste together around 11000 games overall. The game entries came in the following form,

1997,382=[marik,om-by,-,88FFFE98798A6A975B4C59999A7BA86C5D5C3C7A4B896BA7B6A99687,?,?]

Where the long string of penta-decimal numbers specifies the board coordinates of Gomoku moves.

Training the network on expert games (around 10000) to recognize the next move and reward function. Some snapshots of our dataset ...

Neural net architecture and training stuff ... this will take a day or so


# References

1. Rules for Gomoku - http://www.opengames.com.ar/en/rules/Gomoku
2. Shao, Kun & Zhao, Dongbin & Tang, Zhentao & Zhu, Yuanheng. (2016). Move prediction in Gomoku using deep learning. 292-297. 10.1109/YAC.2016.7804906.  - https://www.researchgate.net/publication/312325842_Move_prediction_in_Gomoku_using_deep_learning

3. From-scratch implementation of AlphaZero for Connect4 - https://towardsdatascience.com/from-scratch-implementation-of-alphazero-for-connect4-f73d4554002a

4. Gomoku datasets http://mostovlyansky.narod.ru/iedown.html
