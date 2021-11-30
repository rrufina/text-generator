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
  

## Models

### 1. LSTM

We decided to make next word prediction model that takes as an input two words and output the possible following word. To prepare data for such mosel, we split each sentence on samples of length 3 words, where the first two words are input data and the last word is the target.

#### Architure

The model has following architecture:
```
LSTM_Model(
  (embedding): Embedding(VOCAB_SIZE, EMBEDDING_SIZE, padding_idx=0)
  (lstm): LSTM(EMBEDDING_SIZE, HIDDEN_DIM, batch_first=True)
  (linear): Linear(in_features=HIDDEN_DIM, out_features=VOCAB_SIZE, bias=True)
  (dropout): Dropout(p=0.2, inplace=False)
  (softmax): Softmax(dim=None)
)
```
Since the dataset is not big enough, a large number of layers would negatively affect the training of the model. For embedding layer we used pretrained GloVe embeddings with 100 dimensions.
 


### 3. GPT

## Results

## Further work

