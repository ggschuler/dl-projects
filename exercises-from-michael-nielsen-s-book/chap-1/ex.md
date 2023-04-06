# Chapter I (exercises)

## Sigmoid neurons simulating perceptrons, pt. I-II

**I**. Suppose we take all the weights and biases in a network of perceptrons, and multiply them by a positive constant, $c>0$. Show that the behaviour of the network doesn't change.

> If we choose the sigmoid (logistic) function as our activation function, the output of every neuron will be a number between 0 and 1. This happens because the sigmoid function works by transforming any real number into a number between 0 and 1, by means of the following equation: $$\sigma = (w⋅x+b) = \frac{1}{1+e^{-(w⋅x+b)}}$$

**II**. Suppose we have the same setup as the last problem - a network of perceptrons. Suppose also that the overall input to the network of perceptrons has been chosen. We won't need the actual input value, we just need the input to have been fixed. Suppose the weights and biases are such that $w⋅x+b≠0$ for the input $x$ to any particular perceptron in the network. Now replace all the perceptrons in the network by sigmoid neurons, and multiply the weights and biases by a positive constant $c>0$. Show that in the limit as $c→∞$ the behaviour of this network of sigmoid neurons is exactly the same as the network of perceptrons. How can this fail when $w⋅x+b=0$ for one of the perceptrons?

> Given that $w⋅x+b\ne0$ is always true, for any $w$, $x$, and $b$, it follows that the sigmoid activation would always render an output that goes from 0 to 1. Then, no matter how large the multiplier $c$ is, the 0-1 interval would remain. However, if we force $w⋅x+b=0$ then the sigmoid function output approaches $\frac{1}{2}$, since: $$\sigma = (0) = \frac{1}{1+e^{-0)}} = \frac{1}{1+1} = \frac{1}{2}$$

## Bitwise representation of digit

**III**. There is a way of determining the bitwise representation of a digit by adding an extra layer to the three-layer network above. The extra layer converts the output from the previous layer into a binary representation, as illustrated in the figure below. Find a set of weights and biases for the new output layer. Assume that the first 3 layers of neurons are such that the correct output in the third layer (i.e., the old output layer) has activation at least 0.99, and incorrect outputs have activation less than 0.01.

> For representing digits 0 to 9 > in binary, we need 4 digits-representations:
>
> | digit     | binary representation  | digit     | binary representation
> |-------    | ---------------------  | -----     | ---------------------
> | **0**     | 0000                   | **5**     | 0101
> | **1**     | 0001                   | **6**     | 0110
> | **2**     | 0010                   | **7**     | 0111
> | **3**     | 0011                   | **8**     | 1000
> | **4**     | 0100                   | **9**     | 1001
>
> In the new 4-neurons layer, the first neuron would correspond to the first digit in binary, and so on. If the network outputs a high confidence that the handwritten digit is a 2 (the output layer would look like this: $[0.01, 0.99, 0.01, 0.01, 0.01, 0.01, 0.01, 0.01, 0.01, 0.01]$), then the new layer would output a high likelihood for 0010 (which would look like this: $[0.0, 0.0, 1.0, 0.0]$).
>
> Now, for the set of weights and biases. For the weights, considering the third and four layer size, we would need a $10\times4$ matrix. For the biases, a $1\times4$ vector will do:
>
> $$
> w =
> \begin{bmatrix}
> w_1^1    & w_2^1    & w_3^1  & w_4^{1} \\
> w_1^2    & w2_2     & w_3^2  & w_4^{2} \\
> ...      & ...      & ...    & ...     \\
> w_1^{10} & w_2^{10} & w_3^{10} & w_4^{10} \\
> \end{bmatrix}
> $$
> $$
> b =
> \begin{bmatrix}
> b1 \\
> b2 \\
> b3 \\
> b4
> \end{bmatrix}
> $$
>
> Consider first the matrix $w$. For each row (decimal digit), we want the weights to transform the output from a smoothed 0-1 interval to a step 0-1. Notice how the only binary numbers that start with 1 are 8 and 9. So, we can set the first row of $w$ to be $[0,0,0,0,0,0,0,0,1,1]$, so that the fourth-layer's first neuron will only be activated if the last 2 (8 and 9 digits) are activated in the third layer. Following the same _rationale_, the full $w$ matrix is as follows:
>
>$$
>w =
>\begin{bmatrix}
>0 & 0 & 0 & 0 \\
>0 & 0 & 0 & 1 \\
>0 & 0 & 1 & 0 \\
>0 & 0 & 1 & 1 \\
>0 & 1 & 0 & 0 \\
>0 & 1 & 0 & 1 \\
>0 & 1 & 1 & 0 \\
>0 & 1 & 1 & 1 \\
>1 & 0 & 0 & 0 \\
>1 & 0 & 0 & 1 \\
>
>\end{bmatrix}
>$$
>
> Additionaly, for simplicity, the biases can be set to 0, since we're using a step function as activation and the weight matrix can do all the job.
