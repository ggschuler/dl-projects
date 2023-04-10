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
> $$
> w =
> \begin{bmatrix}
> 0 & 0 & 0 & 0 \\
> 0 & 0 & 0 & 1 \\
> 0 & 0 & 1 & 0 \\
> 0 & 0 & 1 & 1 \\
> 0 & 1 & 0 & 0 \\
> 0 & 1 & 0 & 1 \\
> 0 & 1 & 1 & 0 \\
> 0 & 1 & 1 & 1 \\
> 1 & 0 & 0 & 0 \\
> 1 & 0 & 0 & 1 \\
> 
> \end{bmatrix}
> $$
>
> Additionaly, for simplicity, the biases can be set to 0, since we're using a step function as activation and the weight matrix can do all the job.

## Gradient descent

**IV**.  "Indeed, there's even a sense in which gradient descent is the optimal strategy for searching for a minimum. Let's suppose that we're trying to make a move $Δv$ in position so as to decrease $C$ as much as possible. This is equivalent to minimizing $ΔC≈∇C⋅Δv$. We'll constrain the size of the move so that $∥Δv∥=ϵ$ for some small fixed $ϵ>0$. In other words, we want a move that is a small step of a fixed size, and we're trying to find the movement direction which decreases $C$ as much as possible. It can be proved that the choice of $Δv$ which minimizes $∇C⋅Δv$ is $Δv=−η∇C$, where $η=ϵ/∥∇C∥$ is determined by the size constraint $∥Δv∥=ϵ$. So gradient descent can be viewed as a way of taking small steps in the direction which does the most to immediately decrease $C$."

Prove the assertion of the last paragraph. Hint: If you're not already familiar with the Cauchy-Schwarz inequality, you may find it helpful to familiarize yourself with it.

> The Cauchy-Schwarz inequality asserts that for any two vectors $\bold{v}$ and $\bold{u}$ in n-dimensional space, it is true that:
>
>$$|u \cdot v| \le ||u||||v|| $$
>
> In our case, the vectors of interest are $\Delta v$ and $\nabla C$. We then have:
>
>$$\nabla C \cdot \Delta v \le ||\nabla C||||\Delta v||$$
>
> which implies that
>
>$$||\nabla C||||\Delta v|| \ge \nabla C \cdot \Delta v \ge -(||\nabla C||||\Delta v||)$$
>
>and since we're assuming that $∥Δv∥=ϵ$, it is true that
>
>$$ϵ||\nabla C|| \ge \nabla C \cdot \Delta v \ge -ϵ||\nabla C||$$
>
>which is to say that the inner product of the gradient vector and the displacement vector is bounded between negative and positive $ϵ||\nabla C||$.
>
>the inequality is saturated when $\Delta v$ is a scalar multilple of $\nabla C$, such as when $\Delta v = -η\nabla C$ for some scalar η. Assume that $\Delta v = -η\nabla C$, then $\nabla C\cdot\Delta v = -η||\nabla C||^2$. The magnitude of $\nabla C \cdot \Delta v$ is maximized when η is the negative of the ratio of the magnitudes of $\nabla C$ and $\Delta v$, which is $-ϵ/||\nabla C||$.

**V**. I explained gradient descent when C is a function of two variables, and when it's a function of more than two variables. What happens when C is a function of just one variable? Can you provide a geometric interpretation of what gradient descent is doing in the one-dimensional case?

> When gradient descent is applied onto one-variable functions, the curve describing the function may have a number of valleys corresponding to its degree ($x^2, x^3,...,x^n$). The minimum cost will be the valley whose value is the lesser. Depending on which random point of the function that you start computing the gradient, you might find different values for the minimum, despite the function having a true minimum.

## Mini batches

**VI**. An extreme version of gradient descent is to use a mini-batch size of just 1. That is, given a training input, $x$, we update our weights and biases according to the rules $w_k→w^′_k=w_k−η∂C_x/∂w_k$ and $b_l→b^′_l=b_l−η∂C_x/∂b_l$. Then we choose another training input, and update the weights and biases again. And so on, repeatedly. This procedure is known as online, on-line, or incremental learning. In online learning, a neural network learns from just one training input at a time (just as human beings do). Name one advantage and one disadvantage of online learning, compared to stochastic gradient descent with a mini-batch size of, say, 20.

> When compared to a stochastic gradient descent of batch size 20, one advantage of online learning is that the weights and biases will adapt quickly to new data. This is useful when training on a stream of new data, i.e. real-time applications of DL.
> A disadvantage is that the model is prone to overfitting, given that the training is happening on single data points. Particularly, if data is noisy, a generalizable model might be hard to produce. Furthermore, training with 1-sized batches require frequent updates to the model's parameters, which is loathsome.

## Implementing the network

**VII**. Write out Equation (22) in component form, and verify that it gives the same result as the rule (4) for computing the output of a sigmoid neuron.

Equation 22: $a^′ = \sigma(wa+b)$

Equation 4: $\frac{1}{1+exp(-\Sigma_j w_j x_j-b)}$

> We can rewrite Equation 22 in component form as follows:
> $$a^′_i = \sigma(\Sigma_j w_{ij} a_j + b_i)$$
> where $i$ is the output of a neuron and $j$ is the input neuron, $w_{ij}$ is the weight connecting the $i$-th to the $j$-th neuron, $a_j$ is the activation of the $j$-th input neuron, and $b_i$ is the bias for the $i$-th output neuron. Since we know that $\sigma(x)=1/1+e^{-z}$, we get:
> $$a^′_i = \frac{1}{(1+e^{\Sigma_j w_{ij} a_j - b_i)}}$$
> which is equivalent to Equation 4.
