# Pointwise Mutual Information (PMI) and TF-IDF Analysis

This repository contains a Python implementation that performs two core Natural Language Processing (NLP) tasks on a corpus of Bengali text:

1.  **Collocation Analysis:** Calculating **Pointwise Mutual Information (PMI)** to identify statistically significant word co-occurrences (bigrams).
2.  **Sentence Similarity and Retrieval:** Using **Term Frequency-Inverse Document Frequency (TF-IDF)** vectors and **Cosine Similarity** to find the nearest neighbor sentences.

---

## What the Code Does

The provided Python notebook, `pmi_tfidf.ipynb`, processes a tokenized corpus and generates several output files for analysis.

### 1. Data Loading and Splitting
* The code loads sentences from `tokenized_bengali.txt`.
* The sentences are shuffled and split into three sets: **80% Training**, **10% Validation**, and **10% Test**.

### 2. PMI Calculation (`compute_pmi`)
* **Counting:** Unigram (single word) and Bigram (two adjacent words) counts are collected *only* from the **Training** set.
* **PMI Score:** The `compute_pmi` function calculates the PMI score for all bigrams found in the **Validation** and **Test** sets, using the counts derived from the Training set.
    $$PMI(w_1, w_2) = \log_2 \left( \frac{P(w_1, w_2)}{P(w_1) \cdot P(w_2)} \right)$$
* **Output:** The calculated PMI scores are saved to `pmi_validation.txt` and `pmi_test.txt`, sorted by score. Higher PMI scores indicate stronger statistical association or "collocation" between the two words.

### 3. TF-IDF Vectorization
* The `TfidfVectorizer` is trained on the **Training** set to learn the vocabulary and inverse document frequencies.
* The fitted vectorizer is then used to transform the **Training**, **Validation**, and **Test** sets into sparse TF-IDF matrices. Each sentence is represented as a high-dimensional vector.

### 4. Nearest Neighbor (NN) Search and Retrieval

The code performs two types of nearest neighbor searches using the TF-IDF vectors and Cosine Similarity:

* **NN within Same Set (`nearest_neighbors_within`):**
    * Finds the most similar sentence for every sentence *within* the Validation set and *within* the Test set.
    * Uses a custom function that computes the full pairwise cosine similarity matrix and excludes self-similarity.
    * **Output:** `nearest_neighbors_validation.txt` and `nearest_neighbors_test.txt`.

* **NN from Test/Validation to Training:**
    * Uses `sklearn.neighbors.NearestNeighbors` to efficiently find the single closest sentence in the **Training** set for every sentence in the **Validation** set and **Test** set.
    * The search uses **cosine distance** (which is $1 - \text{cosine similarity}$).
    * **Output:** `nn_val_to_train.txt` and `nn_test_to_