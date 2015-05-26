
[TOC]

# Text Understanding from Scratch

## network structure

$ g_i(x), i = 1, 2, ..., m $ are input frames, while $ h_j(y), j = 1, 2, ..., n $ are output frames, and $ f_{ij}(x) $ is the convolution weights from input frame $i$ to output frame $j$. We call $m$ input frame size, and $n$ output frame size. 

Other settings:

1. nonlinearity: **rectifier** $ h(x) = \max\{x, 0\} $. We always apply this function after a convolutional or linear module. 
2. training algorithm: **SGD**
3. minibatch size: **128**
4. momentum: **0.9**
5. step size: initial value **0.01**, which is halved every 3 epoches for 10 times
6. implementation: **Torch7**

Layers:

1. 6 convolution, 3 fully-connected, 2 dropout (rate 0.5) between fully connected layers
2. weight initialization: Gaussian; large model $(\mu, \sigma) = (0 , 0 .02)$; small model $(\mu, \sigma) = (0 , 0 .05)$

## character quantization

1. Each char is represented using 1-of-m encoding, where $m$ is alphabet size
2. blank and unknown (not in the alphabet) are all-zero vectors
3. quantize chars in backward order
4. fixed size $l$
5. input to our network: $m$ (frame size) frames of length $l$; each row is a frame?
6. alphabet: 69 chars: a-z0-9-,;.!?:'"/\|_@#$%ˆ&*˜`+-=<>()[]{}

## experiments

### English News Categorization

Corpus: **496,835** categorized news articles from more than **2000** news sources. We choose **4** largest categories from this corpus to construct our dataset, using only the **title** and **description** fields. From **each category**, we randomly chose **40,000** samples as training and **1,100** as testing.

- input length: 1014
- output size: 4

**dataset：**
Vocabulary size: 69
Number of training samples: 210906
Number of valid samples: 23435
Number of test samples: 4400

I can only achieve about 90% accuracy on validation data.

# Weakly Supervised Memory Networks

Proposed in 2015, explore how long term memory can be combined with NN. 
Input: a question, and a set of text sentences;
Output: a predicted answer

limitation: needs extensive supervision to train, and require explicit indication of the supporting sentences within the text.

There are a total of 20 different types of bAbI tasks that probe different forms of reasoning and deduction. 