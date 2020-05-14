# Some Final Exploration 
## Final Blog Post - Stefan Stanojevic, Kevin Qualls

After training our neural networks on a dataset of Gomoku games and setting up the code that would run self-play, we seemed to be in a good place. However, the computational complexity of the task ahead of us proved to be a significant difficulty. AlphaZero algorithm runs a number of game simulations (in AphaGo's case, around 1000) at each game step in order to determine which move to make. Furthermore, their game agent has spent a human equivalent of several thousand years playing Go. On the other hand, due to the fact that we are running an unparallelized Python code, a single simulation takes of the order of a minute to complete. So, we have decided to take a step back and think of some other ways we can improve our understanding of Gomoku and related games through deep learning.

Our DATA 2040 Professor, Dr. Potter, let us know that since Connect-4 is a solved game, there exists a great dataset that might be worthwhile for us to explore. This dataset, due to Dr. John Tromp, contains winner information for a full set of 8-token board states in which neither player has won yet. Since we were curious how good machine learning can be in capturing this information, we decided to go ahead and train a neural net to predict a winner given a board state. This is similar to the analysis we've done for Gomoku, but this time we had an advantage of starting from a clean dataset for an exactly solved game. 

After applying the same CNN architecture from the previous two blog posts (inspired by [[2]](https://www.researchgate.net/publication/312325842_Move_prediction_in_Gomoku_using_deep_learning)), we managed to squeeze some extra accuracy out of our model by going deeper and applying skip connections, in the spirit of AlphaGo architecture from [[10]](https://doi.org/10.1038/nature16961). Our best-performing model consisted of a stack of 10 residual blocks, each of the following architecture,

<p align="center">
<img width="200" alt="accuracy" src="https://user-images.githubusercontent.com/31740043/81613680-e68ed600-93ac-11ea-840a-c94590e37cbc.png">
</p>
<p align="center">
  <b>Fig. 7: Schematic of Best Performing Model</b><br>
</p>

This stack of residual blocks was preceeded by a single convolutional layer, and followed by two dense layers, with 100 hidden neurons. Every convolutional layer in this network had 16 filters. We found that using both dropouts and L1 regularizations yields the best results, and tuned the dropout parameter to 0.3 and L1 regularization parameter to 0.5 in all convolutional layers.

Batch normalizations played a pretty important role in our Gomoku model. During our initial experimentation with this model, we also used batch normalizations as a part of our residual blocks. However, this produced slightly suboptimal results compared to our final model, which got rid of batch normalizations altogether. After doing a bit of research on this phenomonon, we realized that this is actually a pretty common theme, among several different neural network architectures, as documented in [[11]](https://arxiv.org/pdf/1801.05134.pdf). Dropouts (when implemented before batch normalization) mess with batch statistics, causing a "variance shift", ultimately dropping the performance [[11]](https://arxiv.org/pdf/1801.05134.pdf).

We got the following accuracy and loss curves:

<p align="center">
<img width="316" alt="fig 8 final project" src="https://user-images.githubusercontent.com/54907300/81743367-80b75280-946f-11ea-9c9a-98521a0d261b.png">
</p>
<p align="center">
  <b>Fig. 8: Accuracy and Loss Curves of Final Model</b><br>
</p>


Our neural network is able to predict the winner of a given 8-token board state with a validation accuracy of around 87%. Furthermore, the validation and accuracy curves came closer together and look more in sync compared to our first model in Fig. 4. 

Since tinkering with different types of CNN architectures seems unable to produce a better accuracy on this dataset, it can be interesting to contemplate what this might mean for the applications of deep learning to game theory. Algorithms such as AplhaZero do not directly use neural nets to decide on the next move; rather, they perform a version of minimax algorithm in which the neural network is used to select moves to base simulations on. In order for this to be successful, it may not be necessary to have neural network calculate the value of the state with extreme precision.

## Future Work


Although we didn't implement reinforcement learning as planned - due to limitations with  computational power - we were able to still achieve a validation accuracy of around 87% for prediciting the next winner in Connect-4, given an 8-token board state. This result can be used to efficiently jump start a great Connect-4 player.

More importantly, we plan to continue to work on implementing a Gomoku player. The most important task going forward is to speed up the self-play process by removing inefficiencies in our code and using parallel processing. Learning more about Cuda library seems like a good place to start.

Something else to explore is experimenting with discounted state values for states that are far removed from the end of the game. Authors of AlphaGo assigned +1 to every state in the winning game and -1 to every losing state; however, due to their immense computational power, they were able to generate huge training datasets. With our more limited resources, however, this may not be the best way to go. Something else to consider if we are unable to achieve significant speedups is doing away with the "policy head" entirely and focusing on just the values. In this case, every "simulation" of the game could serve as a training entry to further improve the neural network estimating the value of game states. 

Another idea to play with is trying to incorporate the symmetries of the problem into our neural network, in order to effectively decrease the size of the game configuration space, along the lines of [[12]](https://arxiv.org/pdf/1602.02660.pdf).  Our 15x15 board has reflection symmetries around a horizontal, vertical, and two diagonal axes containing this point. An option to consider if we are unable to construct a neural network that encodes those symmetries is simply enlarging our training sets by applying those symmetry transformations.

Then, we have encountered several interesting ideas  in the literature that we would like to implement. One potentially very useful idea is called curriculum learning [[5]](https://arxiv.org/pdf/1809.10595.pdf), and has to do with introducing simpler ideas to our player first and then building up its skill. For example, like the authors of this paper, we may try to generate a dataset of important moves ourselves, and feed it to our neural network. Additionally, we can use non - deep learning based mentor AI to generate more training data, and play against our model.

In any case, we believe that with more computational time and coffee we can eventually make AlphaGomoku work.



## References

1. Rules for Gomoku - http://www.opengames.com.ar/en/rules/Gomoku

2. Shao, Kun & Zhao, Dongbin & Tang, Zhentao & Zhu, Yuanheng. (2016). Move prediction in Gomoku using deep learning. 292-297. 10.1109/YAC.2016.7804906.  - https://www.researchgate.net/publication/312325842_Move_prediction_in_Gomoku_using_deep_learning

3. From-scratch implementation of AlphaZero for Connect4 - https://towardsdatascience.com/from-scratch-implementation-of-alphazero-for-connect4-f73d4554002a

4. Gomoku datasets http://mostovlyansky.narod.ru/iedown.html

5. AlphaGomoku: An AlphaGo-based Gomoku Artificial Intelligence using Curriculum Learning - Zheng Xie, Xing Yu Fu, Jin Yuan Yu, Likelihood Lab, https://arxiv.org/pdf/1809.10595.pdf 

6. Wang, Y. (n.d.). Mastering the Game of Gomoku without Human Knowledge. doi: 10.15368/theses.2018.47

7. Silver, D., Schrittwieser, J., Simonyan, K., Antonoglou, I., Huang, A., Guez, A., … Hassabis, D. (2017). Mastering the game of Go without human knowledge. Nature, 550(7676), 354–359. doi: 10.1038/nature24270

8. Dieleman, S., De Fauw, J., Kavukcuoglu, K. (2016). Exploiting Cyclic Symmetry in Convolutional Neural Networks, https://arxiv.org/pdf/1602.02660.pdf

9. DATA 2040 Lecture 21 - Lecture 22: Reinforcement Learning 

10. Silver, D., Huang, A., Maddison, C. et al. Mastering the game of Go with deep neural networks and tree search. Nature 529, 484–489 (2016). https://doi.org/10.1038/nature16961

11. Li, Xiang, et al. "Understanding the disharmony between dropout and batch normalization by variance shift." Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2019. https://arxiv.org/pdf/1801.05134.pdf

12. Dieleman, S., De Fauw, J., Kavukcuoglu, K. (2016). Exploiting Cyclic Symmetry in Convolutional Neural Networks, https://arxiv.org/pdf/1602.02660.pdf
