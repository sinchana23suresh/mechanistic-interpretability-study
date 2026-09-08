# Day 0 – Deep Learning Foundations

**Date:** 15 July 2026

## Goal

Build the foundational knowledge in Machine Learning and Deep Learning required before beginning mechanistic interpretability research.

---

## Resources Used

### 3Blue1Brown
- Neural Networks Playlist
  - Videos 1–6
    - What is a Neural Network?
    - Gradient Descent
    - What is Backpropagation Really Doing?
    - Backpropagation Calculus
    - Transformers
    - Embedding Matrices

### StatQuest
- Introduction to Machine Learning
- Bias and Variance

---

## Concepts Learned

- Machine Learning
- Neural Networks
- Neurons
- Weights and Biases
- Loss Function
- Gradient
- Gradient Descent
- Backpropagation
- Embedding Matrices
- High-level Transformer Architecture
- Bias and Variance

---

## My Understanding

### Machine Learning
Machine learning is the process of training a model using data so that it can learn patterns and perform predictions or solve tasks on unseen data.

### Neural Networks
A neural network consists of interconnected neurons that process information using learned weights and biases. During training, these weights are adjusted so the network becomes better at solving a task.

### Gradient Descent
Gradient descent repeatedly updates the model's weights by moving a small step in the direction opposite the gradient in order to reduce the loss.

### Backpropagation
Backpropagation computes how sensitive the loss function is to every weight in the neural network. These gradients are then used by gradient descent to update the weights.

### Embeddings
Embedding matrices store learned vector representations for every token in the vocabulary. These vectors capture useful semantic information about the tokens.

### Transformers
Transformers first convert tokens into embeddings. Through multiple attention layers, these embeddings exchange information with one another, producing richer representations that are ultimately used to predict the next token.

---

## Biggest Takeaways

- Neural networks learn by adjusting weights.
- The loss function tells the model how wrong it is.
- Backpropagation computes gradients.
- Gradient descent uses those gradients to improve the model.
- Transformers work with embeddings rather than raw words.

---

## Things I Still Don't Understand

- Why embedding vectors capture semantic meaning.
- The mathematical intuition behind backpropagation.
- Why Query, Key and Value are all needed in attention.
- How attention is actually computed.

---

## Reflection

Today was my first day learning Machine Learning and Deep Learning. The concepts initially felt overwhelming because everything was completely new, but by the end of the day I had a basic understanding of neural networks, gradient descent, backpropagation, embeddings, and the high-level idea behind transformers. I still have many questions, but I now have a foundation to build on.

---

## Plan further

- Read *The Illustrated Transformer* by Jay Alammar.
- Read *The Illustrated GPT-2*.
- Watch **But What is a GPT?** by 3Blue1Brown.
- Understand working of a transformer and associated parts.
