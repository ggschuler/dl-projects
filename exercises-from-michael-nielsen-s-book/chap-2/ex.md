# Chapter II (exercises)

## Proving backpropagation equations (I-IV)

**I**. Prove the following equations (I-IV):
$$\delta_j^L = \frac{\partial C}{\partial a_j^L}\sigma'(z^L_j)$$
$$\delta^l = ((w^{l+1})^T \delta^{l+1}) \odot \sigma'(z^l)$$
$$\frac{\partial C}{\partial b^l_j} = \delta^l_j$$
$$\frac{\partial C}{\partial w^l_{jk}}=a^{l-1}_k \delta^l_j$$

>_I_. By definition, $\delta^L$ is the partial derivative of $C$ with respect to the weighted input of the last layer: $\delta_j^L = \frac{\partial C}{\partial z_j^L}$. By applying the chain rule, we express the latter as $$\delta_j^L = \sum_k\frac{\partial C}{\partial a_k^L}\frac{\partial a^L_k}{\partial z_j^L}$$ where the sum is over all neurons $k$ in the output layer. Recalling that the activation of the $j$-th neuron in the $L$-th layer is equals to $\sigma(z^L_j)$, we rewrite the second partial derivative and get the following, which is Equation 1 in component form: $$\delta_j^L = \frac{\partial C}{\partial a^L_j} \sigma '(z_j^L)$$
> ___
> _II_. We can start by defining the error term $\delta$ for a layer $l$ as $\delta^l = \partial C / \partial z^l$. Using the chain rule, we get that $$\frac{\partial C}{\partial z^l} = \sum_k \frac{\partial C}{\partial z^{l+1}_k}\frac{\partial z^{l+1}_k}{\partial z^l}$$ which gives $$\frac{\partial C}{\partial z^l} = \delta^{l+1}_k\frac{\partial z^{l+1}_k}{\partial z^l}$$
>...

## Backpropagation with a single modified neuron

**II**. Suppose we modify a single neuron in a feedforward network so that the output from the neuron is given by $f(\sum_j w_jx_j+b)$, where $f$ is some function other than the sigmoid. How should we modify the backpropagation algorithm in this case?
> Because $f$ is a different function, we would need to compute the derivative of the cost function #C# with respect to its output and to its input, for then to compute the derivative of the weights and biases via chain rule.

## Backpropagation with linear neurons

**III**. Suppose we replace the usual non-linear σ function with σ(z)=z throughout the network. Rewrite the backpropagation algorithm for this case.
> By replacing the sigmoid function with the identity function, we can rewrite equations 1 and 2 as follows: $$\delta^L = \nabla_aC \odot 1$$ $$\delta^L = ((w^{l+1})^T\delta^{l+1}\odot 1)$$ which can be simplified, respectively, to $$\delta^L = \nabla_aC$$ and $$\delta^L = (w^{l+1})^T\delta^{l+1}$$ Equations 3 and 4 for updating biases and weights can be rewritten as $$\frac{\partial C}{\partial b^l_j} = \delta^l_j$$ and $$\frac{\partial C}{\partial w^l_{jk}}=a^{l-1}_k\delta^l_j$$

## Fully matrix-based approach to backpropagation over a mini-batch

**IV**. Our implementation of stochastic gradient descent loops over training examples in a mini-batch. It's possible to modify the backpropagation algorithm so that it computes the gradients for all training examples in a mini-batch simultaneously. The idea is that instead of beginning with a single input vector, x, we can begin with a matrix X=[x1x2…xm] whose columns are the vectors in the mini-batch. We forward-propagate by multiplying by the weight matrices, adding a suitable matrix for the bias terms, and applying the sigmoid function everywhere. We backpropagate along similar lines. Explicitly write out pseudocode for this approach to the backpropagation algorithm. Modify network.py so that it uses this fully matrix-based approach. The advantage of this approach is that it takes full advantage of modern libraries for linear algebra. As a result it can be quite a bit faster than looping over the mini-batch. (On my laptop, for example, the speedup is about a factor of two when run on MNIST classification problems like those we considered in the last chapter.) In practice, all serious libraries for backpropagation use this fully matrix-based approach or some variant.
> Pseudocode:
>>
