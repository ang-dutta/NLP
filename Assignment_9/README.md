# Subword Tokenization: BPE and WordPiece-style Training

This repository contains Python code for training two popular types of subword tokenization models from scratch: **Byte Pair Encoding (BPE)** and a **WordPiece-style** approach.

The goal of subword tokenization is to balance the large vocabulary size of pure word-based models and the inability to handle unknown words in pure character-based models.

---

## What the Code Does

The provided Python notebook, `train_subwords.ipynb`, implements the training and saving procedures for BPE and a frequency-driven WordPiece-style model.

### 1. Utilities and Data Loading (`read_corpus`)
* The `read_corpus` function reads a tokenized text file (where tokens are separated by spaces on each line).
* It counts the frequency of each unique "word" (space-separated token) in the corpus, returning a `Counter` object of word-to-frequency mappings. This serves as the initial vocabulary for subword training.

### 2. Byte Pair Encoding (BPE) Implementation (`train_bpe`)
* **Initialization:** Every unique word is split into its constituent characters, and the special end-of-word marker `</w>` is appended. The initial vocabulary is the set of all unique characters plus `</w>`.
* **Merging:** The `train_bpe` function iteratively finds the **most frequent adjacent pair of symbols** across all words in the corpus (weighted by word frequency).
* **Update:** This most frequent pair is merged into a single new symbol, and this merge is applied across all word representations. The new symbol is added to the vocabulary.
* **Stopping Criteria:** Training stops when either a fixed number of merge operations (`max_merges`) is completed or the vocabulary size reaches a `target_vocab_size`.
* **Encoding (`bpe_encode_word`):** This function applies the learned merges to a new word string to break it down into the trained subword tokens.

### 3. WordPiece-style Implementation (`train_wordpiece_like`)
* This implementation uses a frequency-driven approach similar to BPE but allows for the merging of **contiguous substrings of arbitrary length** (length 2 or more), not just pairs.
* **Merging:** In each iteration, it finds the **most frequent contiguous substring** (up to a defined `max_subword_len`) across all word representations.
* **Update:** The chosen substring is converted into a single new token, added to the vocabulary, and replaces all occurrences in the word sequences.
* **Encoding (`wordpiece_encode_word`):** This function uses a **greedy, longest-match** algorithm to segment a word. It attempts to find the longest token from the learned vocabulary that matches a beginning substring of the word.

### 4. Running the Training (`main` function)
The `main` function executes four separate training runs, demonstrating different parameter settings:

| Model | Stopping Condition | Output Files |
| :--- | :--- | :--- |
| **BPE** | Fixed number of merges (e.g., 32000) | `merges_bpe_<N>merges.txt`, `vocab_bpe_<N>merges.txt` |
| **BPE** | Target vocabulary size (e.g., 32000) | `merges_bpe_vocab_<V>.txt`, `vocab_bpe_vocab_<V>.txt` |
| **WordPiece** | Fixed number of merges (e.g., 32000) | `merges_wp_<N>merges.txt`, `vocab_wp_<N>merges.txt` |
| **WordPiece** | Target vocabulary size (e.g., 32000) | `merges_wp_vocab_<V>.txt`, `vocab_wp_vocab_<V>.txt` |

---

## Topics Explained

### 1. Subword Tokenization

Subword tokenization is a technique used in modern NLP models (like BERT, GPT, and T5) to represent text. It splits words into smaller, linguistically meaningful units called **subwords**.

* **Benefits:**
    * It handles **Out-of-Vocabulary (OOV) words** by breaking them down into known subwords.
    * It reduces the overall **vocabulary size** compared to pure word models.
    * It allows the model to learn relationships between morphology (word parts, prefixes, suffixes).

### 2. Byte Pair Encoding (BPE)

BPE is a simple, data-compression-derived algorithm for building a subword vocabulary.

* It starts with characters.
* It repeatedly finds the **most frequent adjacent pair of characters or character sequences** and replaces every occurrence of that pair with a single, new token.
* The entire process is driven by **frequency**.

### 3. WordPiece (WordPiece-style)

WordPiece is a subword tokenization algorithm used in models like BERT. The approach implemented here is a practical, frequency-driven approximation:

* It is similar to BPE but can merge longer contiguous units, not just pairs.
* The actual WordPiece algorithm uses a **likelihood maximization** approach, selecting the merge that increases the overall likelihood of the training data when added to the vocabulary.
* **Encoding:** When tokenizing a new word, it uses a **greedy, longest-match approach** to ensure the word is segmented into the fewest possible subword tokens, prioritizing longer, learned units.