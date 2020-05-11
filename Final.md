# Some Final Exploration 
## Final Blog Post - Stefan Stanojevic, Kevin Qualls

After training our neural networks on a dataset of Gomoku games and setting up the code that would run self-play, we seemed to be in a good place. However, the computational complexity of the task ahead of us proved to be a significant difficulty. AlphaZero algorithm runs a number of game simulations (in AphaGo's case, around 1000) at each game step in order to determine which move to make. Furthermore, their game agent has spent a human equivalent of several thousand years playing Go. On the other hand, due to the fact that we are running an unparallelized Python code, a single simulation takes of the order of a minute to complete. So, we have decided to take a step back and think of some other ways we can improve our understanding of Gomoku and related games through deep learning.

Prof. Potter has let us know that, since Connect-4 is a solved game, there exists a great dataset that might be worthwhile for us to explore. This dataset, due to Dr. John Tromp, contains winner information for a full set of 8-token board states in which neither player has won yet. Since we were curious how good machine learning can be in capturing this information, we decided to go ahead and train a neural net to predict a winner given a board state. This is similar to the analysis we've done for Gomoku, but this time we had an advantage of starting from a clean dataset for an exactly solved game. 

After applying the same CNN architecture from previous two blog posts (inspired by [1]), we managed to squeeze some extra accuracy out of our model by going deeper and applying skip connections, in the spirit of AlphaGo architecture from [2]. Our best-performing model consisted of a stack of 10 residual blocks, each of the following architecture,

<p align="center">
<img width="200" alt="accuracy" src="https://user-images.githubusercontent.com/31740043/81613680-e68ed600-93ac-11ea-840a-c94590e37cbc.png">
</p>

This stack of residual blocks was preceeded by a single convolutional layer, and followed by two dense layers, with 100 hidden neurons. Every convolutional layer in this network had 16 filters. We found that using both dropouts and L1 regularizations yields the best results, and tuned the dropout parameter to 0.3 and L1 regularization parameter to 0.5 in all convolutional layers.

We got the following accuracy and loss curves,

<p align="center">
<img width="400" alt="accuracy" src="https://user-images.githubusercontent.com/31740043/81614631-53ef3680-93ae-11ea-9396-aed87004b5c0.png">
</p>

<p align="center">
<img width="400" alt="accuracy" src="https://user-images.githubusercontent.com/31740043/81521369-fd81e980-9314-11ea-9beb-9213ecd440a2.png">
</p>

Our neural network is able to predict the winner of a given 8-token board state with a validation accuracy of around 86%. This sounds less impressive, though, if we remember that Connect-4 is a highly asymmetrical game between the two players, with the first player being a significant advantage. In the dataset we were working with, optimally behaving first player would win in around 65% of cases, second player in around 25% and around 10% of games would end in a draw.

Since tinkering with different types of CNN architectures is unable to produce a better accuracy on this dataset, it can be interesting to contemplate what this might mean for the applications of deep learning to game theory. Algorithms such as AplhaZero do not directly use neural nets to decide on the next move; rather, they perform a version of minimax algorithm in which the neural network is used to select moves to base simulations on. In order for this to be successful, it may not be necessary to have neural network calculate the value of the state with extreme precision.

# Future Work

Gomoku ...

# References

[5] https://arxiv.org/pdf/1801.05134.pdf
