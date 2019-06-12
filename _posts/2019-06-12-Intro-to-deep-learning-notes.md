>This is a note for MIT 6.S191 course
>
>[link](http://introtodeeplearning.com/)

# course 1. Intro to deep learning

---

**The Perceptron: Forward Propagation**

Single layer neural network with **tensorflow**:

```python
from tf.keras.layers import *

inputs = Inputs(m)
hidden = Dense(d1)(inputs)

outputs = Dense(d2)(hidden)

model = Model(inputs, outputs)
```

This four lines of code computes the single layer NN.

---

**Deep Neural Network**

More hidden layers

---

Applying Neural Networks

---

Quantifying Loss

Compare **Predicted loss** vs **actual loss**

---

Minimize loss

**Different loss functions**

1. Binary cross entropy loss
```python
loss = tf.reduce_mean(
  tf.nn.softmax_cross_entropy_with_logits(
    model.y,
    model.pred
    ))
```

2. Mean squared error loss
```python
loss = tf.reduce_mean(
  tf.square(
    tf.subtract(model.y, model.pred)
  )
)
```

---

**Training Neural Network**

Find `W` matrices that results the minimum loss

- Gradient descent

```python
# randomly initialize W
weights = tf.random_normal(shape, stddev=sigma)

# loop over the following two steps
# 1. gradient
grads = tf.gradients(ys=loss, xs=weights)
# 2. update weights
weights_new = weights.assign(weights - lr * grads)
# until grads converge
# return the weights

```

---

**Neural Network in practice**

- Learning rate is hard to set
  - try out
  - Design adaptive learning rates
    - Methods
      - Momentum `tf.train.MomentumOptimizer`
      - Adagrad `tf.train.AdagradOptimizer`
      - Adadelta `tf.train.AdadeltaOptimizer`
      - Adam `tf.train.AdamOptimizer`
      - RMSProp `tf.train.RMSPropOptimizer`

---

Mini-batching Gradient descent

- can utilize GPU to do the gradient descent

---

The problem of Over-fitting

---

Regularization

avoid over-fitting

**Regularization method**

1. Dropout randomly `tf.keras.layers.Dropout(p=0.5)`
2. Early stopping. stop training before over-fitting
   1. training loss should always decay
   2. if validation set loss start to grow, we will just stop training here

---

**Core Fundation Review**

1. The Perception

```math
y = g({x}*[W]+w0)
```
2. Neural Networks, how does it work

3. Training in practice, over-fitting, etc.

