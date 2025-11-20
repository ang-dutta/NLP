# POS Tagging with a Hidden Markov Model (HMM)

This repository contains a Python implementation of a **Part-of-Speech (POS) Tagger** using a **Hidden Markov Model (HMM)**. The code trains the HMM on tagged text data and then uses the **Viterbi Algorithm** to predict the most likely sequence of POS tags for new, unseen sentences.

---

## What the Code Does

The provided Python notebook, `Assignment_10.ipynb`, performs the following steps:

### 1. Data Preparation and Loading (`read_data`, `k_folds`)
* The `read_data` function loads the POS-tagged data from a file (e.g., `wsj_pos_tagged_en.txt`), where each word is paired with its tag (e.g., "The\_DT").
* The `k_folds` function takes the data and splits it into three (**K=3**) subsets, or "folds," for **cross-validation**. This allows us to train the model on two-thirds of the data and test its performance on the remaining one-third, repeating this process three times to get a more robust evaluation.

### 2. HMM Training (`train_hmm`)
* This function learns the parameters of the Hidden Markov Model from the training data of each fold.
    * **Transition Probabilities:** It calculates how often one tag **$t_{i-1}$** is followed by another tag **$t_i$** (e.g., the probability that a Determiner (DT) is followed by a Noun (NN)). This includes transitions from a special `<START>` state and to a special `<END>` state.
    * **Emission Probabilities:** It calculates how often a specific **word $w_i$** is "emitted" or observed when the model is in a specific **tag state $t_i$** (e.g., the probability of seeing the word "dog" when the tag state is Noun (NN)).

### 3. POS Tagging/Inference (`viterbi`)
* The `viterbi` function is the core of the prediction process. For a given sentence (sequence of words):
    * It uses the transition and emission probabilities calculated during training.
    * It finds the single **most probable sequence of tags** for that sentence.
    * It uses **log probabilities** to prevent numerical underflow and implements **smoothing** (using a minimal probability $10^{-12}$) for words or transitions that weren't seen in the training data.


### 4. Evaluation (`compute_f1`, `run`)
* The `compute_f1` function calculates standard machine learning metrics (**Precision, Recall, and F1-Score**) for each POS tag, comparing the model's predicted tags against the true ("gold standard") tags.
* The `run` function orchestrates the entire process:
    * It executes the **3-fold cross-validation**.
    * For each fold, it **trains** the HMM, **predicts** tags for the test set, and prints the resulting **Precision, Recall, and F1-Scores**.

---

## Topics Explained

### **1. Part-of-Speech (POS) Tagging**

POS Tagging is the task of labeling words in a text with their corresponding part of speech, such as Noun (NN), Verb (VB), Adjective (JJ), etc. It's a fundamental step in Natural Language Processing (NLP) because the part of speech tells us a lot about the word's function and meaning in a sentence.

* **Example:** "Time flies like an arrow"
    * Time (NN) / flies (VB) / like (IN) / an (DT) / arrow (NN)

### **2. Hidden Markov Model (HMM)**

An HMM is a **statistical sequence model** used for problems where the "states" (the POS tags) are **hidden** (we don't see them directly), and we only observe the "output" (the words).


The HMM is defined by two main sets of probabilities that it learns from the data:
* **Transition Probabilities $P(t_i | t_{i-1})$:** The probability of moving from one hidden state (tag) to the next.
* **Emission Probabilities $P(w_i | t_i)$:** The probability of observing a particular word $w_i$ from a given hidden state (tag) $t_i$.

### **3. Viterbi Algorithm**

The Viterbi Algorithm is a **dynamic programming** technique. It's used in HMMs to solve the **decoding problem**, which is: *Given a sequence of observations (words), what is the single most likely sequence of hidden states (tags) that produced them?*

Instead of calculating the probability for every possible tag sequence (which is computationally impossible for long sentences), Viterbi builds up the most likely partial sequence step-by-step, dramatically speeding up the process.