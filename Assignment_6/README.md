# Language Modeling: Katz Backoff and Kneser-Ney Smoothing

This repository contains a **Python implementation** of two advanced language modeling techniques, **Katz Backoff** and **Kneser-Ney Smoothing**, both configured for a **Quadrigram (4-gram)** context. It also includes methods for generating new sentences using **Greedy Search** and **Beam Search**.

---

## What the Code Does

The provided Python notebook, `assignment_6.ipynb`, focuses on building and utilizing 4-gram language models (LMs).

### 1. N-Gram Extraction (`get_ngrams`)

This utility function preprocesses the input text by:
* **Tokenizing** sentences.
* Adding **start tokens** (`<s>`) and the **end token** (`</s>`) to each sentence, necessary for language modeling probability calculation. For a 4-gram, three start tokens are added (`<s> <s> <s>`).
It then generates all $N$-grams (where $N \in \{1, 2, 3, 4\}$) from the processed text.

### 2. Katz Backoff Language Model (`KatzBackoff`)

This class implements a 4-gram language model using **Katz Backoff** with **Simple Discounting** (Good-Turing is not used here; a fixed $\delta=0.5$ is applied).

* **Training:** It calculates the counts for unigrams, bigrams, trigrams, and quadrigrams from the training data.
* **Probability Calculation (`prob`):** When predicting the probability of a word $w_4$ given history $w_1 w_2 w_3$:
    * If the 4-gram $w_1 w_2 w_3 w_4$ has a count greater than 0, a **discounted Maximum Likelihood Estimate (MLE)** is used.
    * If the 4-gram count is 0, it **backs off** to the 3-gram model, then 2-gram, and finally the 1-gram (unigram) MLE if all higher-order counts are zero.


### 3. Kneser-Ney Smoothing Language Model (`KneserNey`)

This class implements a **Kneser-Ney (KN) Smoothing** model, which is generally regarded as one of the best techniques for language modeling due to its superior handling of lower-order probabilities.

* **Training:** It calculates $N$-gram counts and identifies **continuation counts** (the number of unique word types that follow a given prefix).
* **Probability Calculation (`prob_quad`, `prob_tri`, `prob_bi`, `prob_uni`):** KN smoothing uses a **recursive structure**:
    * The probability of an $N$-gram is a weighted combination of its **discounted $N$-gram probability** and the probability derived from the $(N-1)$-gram backoff model.
    * The backoff probability is the probability of the lower-order context being generated according to the **continuation probability** (the chance of seeing $w_i$ as a novel word, often based on how often it appeared preceded by a unique context).

---

## Sentence Generation Techniques

### 4. Greedy Generation (`generate_greedy`)

This is the simplest decoding strategy.

* At each step, the model considers every possible next word in the vocabulary and selects the one that has the **highest probability** according to the language model.
* This process continues until an `</s>` token is generated or the maximum length is reached. Greedy search is fast but often produces **locally optimal** (and sometimes unnatural) sequences.

### 5. Beam Search (`generate_beam`)

Beam search is a more sophisticated decoding strategy that maintains multiple high-probability sequences (hypotheses) simultaneously.

* **Beam:** It maintains a list of the top $K$ (where $K$ is the `beam_size`) most probable partial sentences (sequences).
* **Expansion:** At each step, every sequence in the beam is expanded by all possible next words.
* **Pruning:** The expanded set is then pruned back down to the **top $K$ sequences** based on their total **log-probability score**.

This process explores a wider search space than greedy search, leading to **higher-quality**, globally better sentences at the cost of being slower.