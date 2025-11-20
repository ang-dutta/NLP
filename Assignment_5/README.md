# Language Modeling: N-gram Models and Smoothing Techniques

This repository contains a Python implementation of N-gram language models, focusing on **Good-Turing Smoothing** for the Trigram (3-gram) model and **Deleted Interpolation** for the Quadrigram (4-gram) model. The code handles data splitting, N-gram extraction, smoothing calculation, and sentence probability estimation.

---

## What the Code Does

The provided Python notebook, `assignment_5.ipynb`, details the steps for building and evaluating N-gram LMs on a corpus of Bengali text.

### 1. Data Splitting (Q1)

The notebook first processes the corpus and randomly splits the sentences into three sets:
* **Training Set:** Used to calculate N-gram counts.
* **Validation Set:** Used for hyperparameter tuning.
* **Test Set:** Used for final performance evaluation.

### 2. N-Gram Extraction and Good-Turing Smoothing (Q2)

#### N-gram Extraction (`get_ngrams`)
This function preprocesses sentences by adding necessary **start tokens** (`<s>`) and an **end token** (`</s>`), and then extracts all contiguous $N$-grams from the tokenized text.

#### Good-Turing Smoothing (`good_turing_smoothing`)
This technique is applied to re-estimate the counts of N-grams, assigning a non-zero probability to **unseen** N-grams. The adjusted count $\left(C^*\right)$ is calculated using the Good-Turing formula:
$$C^* = (c+1) \cdot \frac{N_{c+1}}{N_c}$$
This reserves probability mass for events that occurred zero times.

### 3. Frequency Table (Q3)

A **trigram** model is built, and the frequency of frequencies ($N_c$) table is generated. This table demonstrates how small counts are **discounted** (their $C^*$ value is lower than their raw count $c$) to ensure mass is reserved for the unseen events.

### 4. Deleted Interpolation (Q4)

Deleted Interpolation is used to create a more robust **Quadrigram (4-gram)** model by linearly combining the probabilities of different N-gram orders (4-gram, 3-gram, 2-gram, 1-gram) using weighted averaging.

The model calculates the probability of a word $w_4$ given history $w_1 w_2 w_3$ as:
$$P(w_4|w_1w_2w_3) = \sum_{i=1}^{4} \lambda_i P_{MLE}(w_4|\text{context from } i\text{-gram})$$
The interpolation weights ($\lambda_i$) are set uniformly in the provided code.