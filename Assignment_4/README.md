# Language Modeling: N-gram Models and Simple Smoothing Techniques

This script implements fundamental methods for building **N-gram language models (LMs)** and estimating probabilities using simple **add-additive smoothing techniques**. The code focuses on calculating raw frequency counts for N-grams from a tokenized corpus and comparing three smoothing methods on the **Bigram** model.

---

## What the Code Does

The provided Python script performs the following steps on the Bengali corpus (`tokenized_bengali.txt`):

---

## 1. N-gram Extraction and Model Building

The script preprocesses each sentence by adding:

- `<s>` — Start-of-sentence token  
- `</s>` — End-of-sentence token  

It then extracts and counts **all contiguous N-grams** from the corpus.

### **Models Built**
- Unigram  
- Bigram  
- Trigram  
- Quadrigram  

### **Output Files**
Raw frequency counts are saved into:

- `unigram_model.txt`  
- `bigram_model.txt`  
- `trigram_model.txt`  
- `quadrigram_model.txt`

---

## 2. Add-Additive Smoothing Techniques

The script demonstrates three smoothing techniques for **Bigram probability estimation** to handle zero-frequency issues and reassign probability mass.

---

### Add-One Smoothing (Laplace Smoothing)

Adds **1** to every N-gram count and increases the denominator by the vocabulary size \(V\).

$$
P(w_2 \mid w_1) \;=\; \frac{\text{count}(w_1, w_2) + 1}{\text{count}(w_1) + V}
$$


### Add-K Smoothing

Adds a fractional constant \(k\) (here \(k = 0.3\)) to each count.

$$
P(w_2 \mid w_1) \;=\; \frac{\text{count}(w_1, w_2) + k}{\text{count}(w_1) + k \cdot V}
$$


### Token Type Smoothing

Reserves additional probability mass for unseen events by adding \(V\) to the numerator and \(V^2\) to the denominator.

$$
P(w_2 \mid w_1) \;=\; \frac{\text{count}(w_1, w_2) + V}{\text{count}(w_1) + V^2}
$$

---

## 3. Smoothing Results Output

A file named **`smoothing_output.txt`** is generated.  
It contains a **comparative table** of probabilities for all observed bigrams computed using:

- Add-One smoothing  
- Add-K smoothing  
- Token Type smoothing  

---

