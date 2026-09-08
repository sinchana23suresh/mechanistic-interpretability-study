# Research Log 

**Topic:** Transformers — Self-Attention, Multi-Head Attention & Positional Encoding

---

## Today's Goal

To understand the basic architecture of Transformers and study how self-attention works, including Query, Key, and Value vectors, scaled dot-product attention, multi-head attention, positional encoding, and the encoder-decoder architecture.

---

## 1. Transformers

Transformers are neural network architectures designed to process sequential data using attention mechanisms rather than relying primarily on recurrent processing.

A major advantage of Transformers is that attention allows the model to consider relationships between different words in a sequence and enables parallel processing during training.

The Transformer architecture was introduced in the paper **"Attention Is All You Need."**

A basic encoder-decoder Transformer can be represented as:

```text
Input
  ↓
Encoder
  ↓
Decoder
  ↓
Output
````

For example:

```text
Input: "Namaste"
        ↓
    Transformer
        ↓
Output: "Hello"
```

---

## 2. Encoder-Decoder Architecture

The Transformer consists of two major components:

* **Encoder**
* **Decoder**

### Encoder

The encoder receives the input sequence and transforms it into contextual representations.

A simplified encoder layer contains:

```text
Input
  ↓
Self-Attention
  ↓
Add & Normalize
  ↓
Feed Forward Neural Network
  ↓
Add & Normalize
  ↓
Output
```

Multiple encoder layers can be stacked together.

### Decoder

The decoder generates the output sequence one token at a time.

A simplified decoder contains:

```text
Masked Self-Attention
        ↓
Add & Normalize
        ↓
Encoder-Decoder Attention
        ↓
Add & Normalize
        ↓
Feed Forward Neural Network
        ↓
Add & Normalize
        ↓
Linear Layer
        ↓
Softmax
        ↓
Next Token Probability
```

---

## 3. Self-Attention

Self-attention allows each word in a sequence to determine how much attention it should give to other words in the same sequence.

For example:

> "The animal didn't cross the road because it was tired."

To understand what **"it"** refers to, the model needs to look at other words in the sentence.

Self-attention helps the model determine which words are relevant to each other.

The basic process is:

```text
Input Sequence
      ↓
Create Q, K, V
      ↓
Calculate Attention Scores
      ↓
Scale Scores
      ↓
Apply Softmax
      ↓
Calculate Weighted Values
      ↓
Attention Output
```

---

## 4. Query, Key and Value Vectors

For every input word, the Transformer creates three vectors:

* **Query (Q)**
* **Key (K)**
* **Value (V)**

These are obtained by applying learned weight matrices to the word embeddings.

### Query

The Query represents the information that a token is looking for.

### Key

The Key represents the information associated with a token that can be compared against Queries.

### Value

The Value contains the actual information that is passed forward after the attention weights are calculated.

The Query and Key vectors are used to calculate how strongly two tokens should attend to each other. The Values are then combined according to these attention weights.

---

## 5. Scaled Dot-Product Attention

The attention operation is:

$$
Attention(Q,K,V)
=
softmax\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

Where:

* $Q$ = Query matrix
* $K$ = Key matrix
* $V$ = Value matrix
* $d_k$ = dimension of the Key vectors

### Why divide by $\sqrt{d_k}$?

As the dimensionality of the vectors increases, the dot products can become large.

Dividing by $\sqrt{d_k}$ scales the scores and helps keep the Softmax function numerically stable.

---

## 6. How Attention Scores Are Calculated

For each token:

1. Generate its Query, Key and Value vectors.
2. Take the Query of a token.
3. Calculate its dot product with the Keys of all tokens.
4. This produces attention scores.
5. Divide the scores by $\sqrt{d_k}$.
6. Apply Softmax.
7. Use the resulting values as weights for the Value vectors.
8. Compute the weighted sum of the Values.

The resulting vector becomes the attention representation for that token.

---

## 7. Softmax

Softmax converts the attention scores into attention weights.

The resulting weights are positive and sum to 1.

Therefore, the model can interpret them as indicating how strongly a particular token should focus on other tokens.

For example:

```text
Word A → 0.10
Word B → 0.60
Word C → 0.20
Word D → 0.10
```

The model would focus most strongly on **Word B**.

---

## 8. Multi-Head Attention

Instead of performing only one attention operation, Transformers use multiple attention heads.

Each head can learn different relationships between tokens.

For example, different attention heads may learn:

* grammatical relationships
* semantic relationships
* long-range dependencies

The outputs from all attention heads are concatenated and then projected using another learned weight matrix.

Conceptually:

```text
Input
  ↓
 ┌────────┬────────┬────────┐
 ↓        ↓        ↓
Head 1   Head 2   Head 3   ... Head h
 ↓        ↓        ↓
 └────────┴────────┴────────┘
          ↓
      Concatenate
          ↓
      Linear Layer
          ↓
        Output
```

Multi-head attention allows the model to learn multiple representation subspaces simultaneously.

---

## 9. Positional Encoding

One important property of Transformers is that attention itself does not inherently provide information about the order of tokens.

For example:

```text
Man eats grass
Grass eats man
```

These sentences contain the same words but have completely different meanings because the positions of the words are different.

Therefore, Transformers need information about **where each token occurs in the sequence**.

This is provided through **positional encoding**.

The input to the Transformer can be thought of as:

```text
Word Embedding + Positional Encoding
                  ↓
              Transformer
```

### Intuition

Word embeddings tell the model:

> **What is this word?**

Positional encoding tells the model:

> **Where is this word?**

The combination gives the Transformer both semantic and positional information.

---

## 10. Feed Forward Neural Network

After the attention operation, the representation is passed through a position-wise Feed Forward Neural Network.

A simplified flow is:

```text
Attention Output
      ↓
Feed Forward Neural Network
      ↓
Output
```

The feed-forward network processes each position independently and applies learned transformations.

---

## 11. Residual Connections & Layer Normalization

Transformer layers also use **residual connections** and **layer normalization**.

A simplified encoder block can be represented as:

```text
Input
  ↓
Self-Attention
  ↓
Add & Normalize
  ↓
Feed Forward Network
  ↓
Add & Normalize
  ↓
Output
```

Residual connections help information and gradients flow through deeper networks.

Layer normalization helps stabilize the representations during training.

---

## 12. Masked Self-Attention

The decoder uses **masked self-attention**.

The decoder should not be allowed to look at future tokens while generating the current token.

For example:

```text
I am going to ___
```

The model should only use the tokens that have already been generated and must not see the correct future word during generation.

The mask prevents attention from being given to future positions.

This prevents the decoder from "cheating" during training.

---

## 13. Encoder-Decoder Attention

The decoder also uses attention over the encoder's output.

This allows the decoder to determine which parts of the input sequence are relevant while generating each output token.

The general idea is:

```text
Encoder Output
      ↓
   Keys + Values
      ↑
      |
Decoder Query
      ↓
Encoder-Decoder Attention
      ↓
Decoder Output
```

The decoder's Query comes from the decoder, while the Keys and Values come from the encoder.

This allows the decoder to focus on the relevant parts of the input sequence.

---

## 14. Loss Function

The model needs a way to measure how good its prediction is.

A common loss function for classification/token prediction is **Cross-Entropy Loss**.

Conceptually:

$$
Loss = -\log(P(\text{correct word}))
$$

If the model assigns a high probability to the correct word:

```text
High probability → Low loss
```

If the model assigns a low probability to the correct word:

```text
Low probability → High loss
```

The loss is then used during **backpropagation** to update the model's parameters.

---

## 15. Overall Transformer Flow

The overall process I understood can be summarized as:

```text
Input Tokens
     ↓
Token Embeddings
     +
Positional Encoding
     ↓
Encoder
     ↓
Self-Attention
     ↓
Add & Normalize
     ↓
Feed Forward Network
     ↓
Add & Normalize
     ↓
Encoder Output
     ↓
Decoder
     ↓
Masked Self-Attention
     ↓
Encoder-Decoder Attention
     ↓
Feed Forward Network
     ↓
Linear Layer
     ↓
Softmax
     ↓
Next Token Prediction
```

---

## Key Takeaways

* Transformers use **attention** to model relationships between tokens.
* Self-attention allows a token to consider other tokens in the same sequence.
* Every token is transformed into **Query, Key and Value vectors**.
* Attention scores are calculated using the dot product between Queries and Keys.
* The scores are scaled by $\sqrt{d_k}$ before applying Softmax.
* Softmax converts the scores into attention weights.
* The attention weights are used to create a weighted combination of Value vectors.
* **Multi-head attention** allows the model to learn different relationships simultaneously.
* **Positional encoding** provides information about token order.
* The encoder processes the input representation.
* The decoder generates the output.
* **Masked self-attention** prevents the decoder from looking at future tokens.
* **Encoder-decoder attention** allows the decoder to focus on relevant parts of the encoder output.
* Feed-forward networks further transform the representations.
* Residual connections and layer normalization help stabilize and train deep Transformer networks.
* Cross-entropy loss measures how well the model predicts the correct token.

---

## Questions / Things I Still Need to Understand

1. How exactly are the Query, Key and Value weight matrices learned during training?
2. How does the attention mechanism work mathematically for a complete sequence?
3. How does multi-head attention divide the embedding dimensions between heads?
4. How are positional encodings calculated using sine and cosine functions?
5. How does backpropagation update the attention weight matrices?
6. How does the complete Transformer architecture translate into code?
7. How does attention relate to mechanistic interpretability?
8. How can individual attention heads be analyzed and interpreted?

---

## Next Steps

* Work through a numerical example of self-attention manually.
* Implement scaled dot-product attention from scratch.
* Understand the Transformer architecture at the matrix-operation level.
* Study positional encoding mathematically.
* Explore how attention heads can be analyzed for interpretability.
* Begin connecting Transformer architecture to mechanistic interpretability.

---

## Summary

Today I studied the core architecture of Transformers and developed an initial understanding of how attention allows the model to determine relationships between different tokens.

The most important concept I learned is that self-attention converts each token into Query, Key and Value representations. Queries and Keys determine **how much attention should be given**, while Values determine **what information is passed forward**.

I also learned why positional encoding is necessary, how multi-head attention provides multiple representation subspaces, and how the encoder and decoder interact in a sequence-to-sequence Transformer.

```

**Now THIS is one single copy-paste.** Click the copy button on the code block → paste into the GitHub editor → done. 😭
```
