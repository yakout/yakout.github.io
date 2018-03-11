---
title:  "Coursera deep learning specialization by Andrew Ng [Course 2 - Week 3]"
last_modified_at: 2018-02-18T11:45:04-04:00
categories: 
  - deeplearning
tags:
  - knowledge
header:
  overlay_image: /assets/images/nn-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
---
<p></p>



{% include toc title="Week Contents" %}

Objectives:
* Master the process of hyperparameter tuning.
* Learn how to use TensorFlow.

this is test $ x = y$

## Hyperparamter tuning
We have seen a lot of hyperparamters (Grouped by importance of tuning):
- Group 1
  * \\(\alpha\\)

- Group 2
  * \\(\beta\\) (momentum term).
  * Mini-batch size.
  * Number of hidden units.

- Group 3
  * Number of layers.
  * Learning rate decay.

- Group 4
  * \\(\beta_1, \beta_2\\) (Adam) We can use: \\(\beta_1 = 0.9, \beta_2 = 0.999, \varepsilon = 10^{-8}\\)  Which will work well most of the time.


### Tuning Process
Efficiently finding the best combination of these hyperparameters is the key to any DL/ML solution. There are basically two famous ways to tune hyperparameters:
* Grid search: We try all possible combinations of hyperparamters.
* Random Search: We try random values.

### Using an appropriate scale.
Sampling at random doesn't mean sampling uniformly at random over the range of valid values, so it's important to pick the appropriate scale on which to explore the Hyperparameters.

**How?**
Imagine you want to tune the learning rate \\(\alpha\\) and you said that it could be as low as 0.0001 and as high as 1.

So we need to explore the values \\([0.0001, ...., 1]\\)
but if we sampled the values uniformly at random, about 90% of the sampled values will be from \\([0.1 \Rightarrow 1]\\)
and only 10% will be from \\([0.0001 \Rightarrow 0.1 ]\\).
So instead of using just a linear scale we must use larger scale:

\\([0.0001, ..., 0.001, ..., 0.01, ..., 0.1, ..., 1]\\)

And sample uniformly at random over:
* \\(0.0001 \Rightarrow 0.001\\)
* \\(0.001 \Rightarrow 0.01\\)
* \\(0.01 \Rightarrow 0.1\\)
* \\(0.1 \Rightarrow 1\\)

This can be done in python like this:
```python
r = -4 * np.random.rand()
learning_rate = 10**r
```
**Recall**: ```np.random.rand()``` returns random sample from a uniform distribution over [0, 1).

To understand why this work notice that \\(r \in [-4, ..., 0] \\)
thus \\(\text{learning_rate }(\alpha) \in [10^{-4}, ..., 10^0] = [0.0001, ..., 1]\\)

**In General:**
If you want to sample or take values uniformly at random between \\([10^a, ..., 10^b]\\) on large scale:
you first find the values a,b:

$$ a = log_{10}(\text{min_value}) $$ (min_value in our example was 0.0001)
$$ b = log_{10}(\text{max_value}) $$ (max_value in our example was 1)

and you choose \\(r \in [a, ..., b]\\) then you get the correct value \\(parameter = 10^r \\).

Choosing Hyperparameter for exponentially weighted averages \\((\beta)\\) can be a little tricky:

If we found someway that \\(\beta \in [0.9, ..., 0.999] \\)
we can search instead for \\(1 - \beta \in [0.1, ..., 0.001]\\)
thus \\(r \in [log_{10}(0.1), ..., log_{10}(0.001)] = [-3, ..., -1] \\)
so \\(1 - \beta = 10^r \\)
and finally: <br>
\\(\beta = 1 - 10^r\\)


### Pandas VS Caviar
* Babysitting one model:
you use this when you have huge data and low resources CPI/GPU.
* Training many models in parallel:
you train different model in parallel with different hyperparameter setup, then choose the best one.

* Choosing between pandas and caviar is determined by the computational power you can access.


## Batch normalization
We could either normalize \\(Z^{[l]} \text{ or } A^{[l]}\\) but often we normalize \\(Z^{[l]}\\) before applying any activation.

**Implementation:**

$$ \mu = \frac{1}{m} \sum_i Z^{(i)}$$

$$ \sigma^2 = \frac{1}{m} \sum_i (Z^{(i)} - \mu)^2$$

$$ Z_{norm}^{(i)} = \frac{Z^{(i)} - \mu}{\sqrt{\sigma^2 + \varepsilon}}$$

$$ \tilde{Z^{(i)}} = \gamma Z^{(i)} + \beta$$

Notes:
* \\(\varepsilon\\) is to avoid division by zero.
* \\(\gamma \text{ and } \beta \\) are learnable parameters in the model.

## Multi-class classificatin
Use Softmax in the last layer.

## Intro to Deeplearning frameworks

Some of deeplearning frameworks:
* Caffe/Caffe2
* CNTK
* DL4J
* Keras
* Lasagne
* mxnet
* PaddlePaddle
* TensorFlow
* Theano
* Torch

Criteria for choose a framework:
- Ease of programming (Fast development Cycle)
- Running Speed
- Truly open (open source with good governance)



TensorFlow Code example 
```python
import numpy as np
import tensorflow as tf

coefficients = np.array([[1], [-20], [25]]) # input data
w = tf.Variable([0], dtype=tf.float32)
x = tf.placeholder(tf.float32, [3,1])

cost = x[0][0] * w**2 + x[1][0] * w + x[2][0]
optimizer = tf.train.GradientDescentOptimzer(learning_rate = 0.01).minimize(cost)
init = tf.global_variables_initializer()

session = tf.Session()
session.run(init)
print(session.run(w))

# or more safe method and better at cleaning up in case of errors and exceptions while executing:
#with tf.Session() as sess:
# sess.run(init)
# print(sess.run(w))

for i in range(1000):
  session.run(optimizer, feed_dict = {x:coefficients})
print(session.run(w))
```

Some notes about TensorFlow:
* Tensorflow is a programming framework used in deep learning
* The two main object classes in tensorflow are Tensors (Variables, Placeholders ...) and Operators (tf.matmul, tf.add, ...).
* When you code in tensorflow you have to take the following steps:
  * Create a graph containing Tensors and Operations.
  * Create a session
  * Initialize the session
  * Run the session to execute the graph
  * You can execute the graph multiple times in for loop as seen before.
* The backpropagation and optimization is automatically done when running the session on the "optimizer" object.
* Placeholder is a variable whose values you assign later.
  * You use it to get your data into the cost function.
  * or Feed different mini-batches.
* TensorFlow knows how to compute the derivates and minimize the cost functin J
* TensorFlow has built-in the neccessary backward functions, so no need to explicity implement backpropagation.

---


**Resources**: [Deep Learning
Specialization on Coursera, by Andrew Ng](https://www.coursera.org/specializations/deep-learning)
