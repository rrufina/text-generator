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

The dataset we were going to work with initially contained [love letters](https://www.kaggle.com/fillerink/love-letters). 
Since this data was not enough for decent results, we expanded it and added compliments collected manually from several sites([1](https://www.thetalka.com/love-letters-for-her/), [2](https://memesbams.com/love-letters-for-her/), [3](https://www.legit.ng/1218152-deep-love-letters-ll-cry.html)).


## Data preparation

First of all, we processed all the examples we collected, removed duplicates, and divided each piece of text into sentences. 
Then we checked all the sentences by ourselves and removed those that were too long or lost their meaning without context. 
For each sentence the tags <Start-Of-Sequence> and <End-Of-Sequence> were added respectively.
With prepared corpus we tokenized each word and constructed the vocabulary. The vocabulary consists of around 1284 words with special index for unknown words.   


## Models

### 1. LSTM Model

We decided to make next word prediction model that takes as an input two words and output the possible following word. 
To prepare data for such model, we split each sentence on samples of length 3 words, where the first two words are input data and the last word is the target.

The model has following architecture:
```
LSTM_Model(
  (embedding): Embedding(VOCAB_SIZE, EMBEDDING_SIZE, padding_idx=0)
  (lstm): LSTM(EMBEDDING_SIZE, HIDDEN_DIM, batch_first=True)
  (linear): Linear(in_features=HIDDEN_DIM, out_features=VOCAB_SIZE, bias=True)
  (dropout): Dropout(p=0.2)
  (softmax): Softmax()
)
```
Since the dataset is not big enough, a large number of layers would negatively affect the training of the model. 
For embedding layer we used pretrained GloVe embeddings with 100 dimensions. 
As activation function we used Softmax that will calculate probability of each word in dictionary to be the right predicted word. 
For loss function we choose Cross Entropy since the problem of word prediction could be considered as classification task with the number of classes equals the size of vocabulary.
 
For fine-tuning this model, we conducted a series of experiments where changed parameters and monitored results. 
We determined, that RMSprop optimizer shows better results than others. 
Also we found out the optimal size for hidden dimension that will not interfere with training.

When we generate a new text, the first two tokens for models's input are selected as <SOS> tag and random word from the vocabulary. Then the model repeatedly predicts next word until it predicts the <EOS> tag. To make the outputs more diverse, we randomly choose one of the two words with the highest probability.

### 2. GPT-2

## Results

The LSTM model was trained for 250 epochs so that the output results looked more or less acceptable. The quality of the generated sentences can only be assessed by ourselves. For evaluation we choosed the generated sentences with length more than 3 and less than 25. In the most cases, the sentences look like normal sentences but don't make sence. Some outputs could be considered as compliments, however this is more an accident than an indicator of the success of the model.

### Examples of generated compliments
  
* You are the way a blessing my life of great.
* My heart for you are so much and love you are the way you is best life.
* I am in love you.
* Thank for all my heart.
* Your love affection right to be so much.
* All my life i to you is addiction same one are a life.
  
For GPT-2 results are better. 
  
  
## Further work

The most important limitation we have encountered is not enough data to successfully train the model, therefore the main goal for the further work is to expand the dataset. 

Another essential issue is tracking the meaning of the entire sentence and predicting the next word based on all the previous words in the sentence. To do this, you can use an autoencoder architecture with an attention mechanism.
