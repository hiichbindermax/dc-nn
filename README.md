# dc-nn
A toy neural network for binary classification in `dc`.

`dc` is one of the oldest UNIX programs that is still around.
It is a reverse polish notation (RPN) calculator ([Wikipedia](https://en.wikipedia.org/wiki/Dc_(computer_program))).
The version used here is GNU `dc`, which comes preinstalled with all major GNU systems.

This program is purely as a proof of concept and has no real value other than maybe some educational value. 

## Inspiration

One day I thought to myself if it would be possible to run a neural network in a RPN calculator and here it is. 

## Concepts

`dc` works with multiple registers, stacks, macros, and even some kind of arrays. The input data is stored into arrays, 
while all calculation is done through macros stored in various registers.

### Command palette that was used

Read more in the [manual](https://www.gnu.org/software/bc/manual/dc-1.05/html_mono/dc.html).

 - `sn`: store the top-of-stack in register `n`
 - `ln`: copy value in register `n` to top-of-stack
 - `z`: put the depth of the stack on the stack
 - `x`: execute macro on top-of-stack
 - `:r`: top-of-stack is index to array `r` while next-on-stack is saved at that position
 - `;r`: retrive contenten from array `r` with index from top-of-stack
 - Conditionals
 - Arithmetic
 - `#` everything that follows is interpreted as a comment

## Neural network
The neural network is used for binary classifcation and has two input neurons, one hidden layer with two neurons and one output neuron.
The weigths and biases are initiated as random values. `dc` has no random function so these were generated using python and then copied to the program code.

[Victor Zhou's blog post](https://victorzhou.com/blog/intro-to-neural-networks/) was used as the main guide. The structure of the network is the same, as well as the used input data. 

The network uses backpropagation with gradient descent for learning. The (partial) derivatives of the neurons are all hardcoded. 

The activation function is not the sigmoid, as in the blog post, because the exponential function doesn't exist and exponentiation can only be done with integer exponents.
Instead an algebraic function is used, which is very similar to the sigmoid function:

$$\sigma(x)=\frac{x}{2\sqrt{1+x^2}}+0.5$$

This function returns values between 0 and 1, which is perfect for binary classification. 

MSE loss is used and reported after each training step.

## How to run

```bash
dc nn.dc
```

## Compressed view
`dc` doesn's require any whitespace (besides between different numbers to be put on the stack). That's why almost all whitespace (and comments) can be omitted and the program can be executed as a (loooong) one-liner:

```
20k[q]sQ0.1sp1000sE[d2^1+v2*/0.5+]sA[2^1+3^v2*1r/]sa[dd;yr;Y-2^LM+sM1-d0!=m]sm[0sMlnlmxlMln/dsM]sl0.8761123145115244 1:w0.1113624055358604 2:w_2.4816227332892038 3:w 0.1630997065297297 4:w0.8309874697929129 5:w_0.4641056543425773 6:w_0.1252385391632506 1:b0.3514078803331424 2:b0.0999569028091161 3:b[lXx2;w*r1;w*+1;b+d1:1lAxd2:1lXx4;w*r3;w*+2;b+d1:2lAxd2:26;w*r5;w*+3;b+d1:olAxd2:o]sf[[_2 _1]1][[ 25 6]0][[ 17 4]0][[_15 _6]1]zsn[zszxlz:ylz:xlZx]s_[z0<_]sZlZx[li;xsXlfxdli:Yli;yr-_2*0:L1;olax2;1*1:L1;olax2;2*2:L1;olax3:L1;olax5;w*4:L1;olax6;w*5:LlXx1;1lax*6:L1;1lax*7:L1;1lax8:LlXx1;2lax*9:L1;2lax*10:L1;2lax11:L1;wlp0;L4;L7;L***-1:w2;wlp0;L4;L6;L***-2:w1;blp0;L4;L8;L***-1:b3;wlp0;L5;L10;L***-3:w4;wlp0;L5;L9;L***-4:w2;blp0;L5;L11;L***-2:b5;wlp0;L1;L**-5:w6;wlp0;L2;L**-6:w3;blp0;L3;L**-3:bli1-sili0<T]sT[lEd1-sE0=QlnsilTxllx[loss: ]nnAPDPcltx]stltx
```
