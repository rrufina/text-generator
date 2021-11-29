# PMLDL Project: Technical Report


## Topic
Compliment Generator


## Team
* Ravida Saitova, DS-01
* Rufina Sirgalina, DS-02


## Overview

Мб добавить какое нибудь описание text generation в целом.

For text generation we could determine two approaches:
* Character-Level Generation
* Word-Level Generation

After reading some explanations about these two techniques, we decided to choose word prediction method, since it takes less time for training and gives a smaller error.


## Dataset

The dataset we were going to work with initially contained [love letters](https://www.kaggle.com/fillerink/love-letters). Since this data was not enough for decent results, we expanded it and added compliments collected manually from several sites.


## Data preparation

First of all, we processed all the examples we collected and divided each piece of text into sentences. Then we checked all the sentences by ourselves and removed those that were too long or lost their meaning without context. With prepared corpus we tokenized each word and constructed the vocabulary.  

We decided to make next word prediction based on two previos words, therefore we needed to split each sentence on saples of length 3 words, where the first two words are input data and the last word is the target.  

## Models

### 1. Markov chain

### 2. LSTM
#### Architure


#### Embeddings
We use Glove embedding for our data. 


### 3. GPT


