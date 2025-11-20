# Trie Storage and N-gram Frequency Distribution

This repository contains two Jupyter notebooks that implement the foundational steps of preparing data for N-gram language modeling. The first notebook focuses on storing words using a Trie data structure, while the second notebook extracts N-grams and generates their frequency distributions from a Bengali corpus.

---

## What the Code Does

The provided notebooks, `1_StoringInTrie.ipynb` and `2_FrequencyDistribution.ipynb`, describe the steps for preprocessing and preparing N-gram data from a tokenized Bengali text file.

### 1. Token Storage using Trie (`1_StoringInTrie.ipynb`)

This notebook implements a Trie (prefix tree) for storing all unique tokens from the corpus.

#### Trie Construction
Each token from the corpus is inserted character by character into the Trie.  
The Trie supports:
* Fast prefix lookup
* Efficient memory representation for large vocabularies
* Quick search and token existence checking

This structure is useful for tasks such as prefix-based search, autocomplete systems, and dictionary-based NLP preprocessing.

### 2. N-gram Extraction and Frequency Distribution (`2_FrequencyDistribution.ipynb`)

#### Sentence Preprocessing
Each sentence is prepared by adding:
* `<s>` as the start token
* `</s>` as the end token

This ensures correct extraction of N-grams at sentence boundaries.

#### N-gram Extraction
The notebook extracts:
* Unigrams  
* Bigrams  
* Trigrams  
* Quadrigrams  

All contiguous N-grams are generated from the processed corpus.

#### Frequency Count Generation
The frequencies of each N-gram are computed and saved into the following files:
* `unigram_model.txt`
* `bigram_model.txt`
* `trigram_model.txt`
* `quadrigram_model.txt`

Each file contains the raw N-gram counts, which serve as the foundation for later smoothing and probability estimation.

---
