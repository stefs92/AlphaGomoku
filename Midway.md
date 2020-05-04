# Title
## Midway Blog Post - Stefan Stanojevic, Kevin Qualls

In our initial blog post, we wrote about training a neural network on the dataset of Gomoku games, in order to predict the next move made a human player would make. Since then, we have taken our project a step further and coded an AI Gomoku player that can gradually improve its skill through self-play and reinforcement learning.

Since we were curious about whether our neural network has actually learned important elements of the game or not, we decided to quickly code a self-playing module and visualize its performance, before fully implementing the AlphaZero algorithm. We used our trained "policy head", giving us the probability distribution over possible moves, to iteratively generate the next move until one of the AI players was in the position to win by connecting 5 tokens. Python's ipywidget library proved to be very useful for visualizing the games, specifically its objects interact and Play. You can see one of the sample games generated this way below,

<p align="center">
<src="https://user-images.githubusercontent.com/31740043/80986301-6261af00-8dfe-11ea-99b3-8e09b56f3c4f.gif">
</p>

Predictably, our AI is pretty bad at playing the game, as it is missing key components - the "value head" evaluating the chance of winning for different board states, as well as the ability to simulate the future. In order to rectify the first problem, we went back to our dataset of human games. Keeping track of the winner of each game, we assigned a score of +1, -1 or 0 (in case of a draw) to each board state of a given game and averaged those out over the dataset. Then, a neural network of the same architecture as in the initial blog post (except the final layer, adopted to the new regression task) is trained to predict the board state value. We obtain a not great, not terrible performance as seen in the plot below,

<p align="center">
<img width="400" alt="accuracy" src="https://user-images.githubusercontent.com/31740043/80948589-6bcc2680-8dc0-11ea-9827-2d1b30cf1757.png">
</p>

As we can see, the best validation accuracy is achieved very early in the training process, and there is significant overfitting later on.

At this point, we were in a position to try out a slightly more sophisticated algorithm. Our agent can decide on which move to make considering the value functions of the 
