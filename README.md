# PMLDL Project: Technical Report


## Topic
Compliment Generator


## Team
* Ravida Saitova, DS-01
* Rufina Sirgalina, DS-02


## Overview

The text generation is the subfield of the Natural Language Processing(NLP) task. The main goal of it is to generate random but still meaningful texts. The generated list of sentences has to satisfy some requirements, one of them could be sentiments or emotions shown. These requirements bring other problems to appear: could we teach the generator the sense of humor or could the generated text be polite? One of these tasks we will try to solve in this project: The generation of compliments. 

In general, for text generation we could determine two approaches:
* Character-Level Generation
* Word-Level Generation

After reading some explanations about these two techniques, we decided to choose the word prediction method, since it takes less time for training and gives a smaller error. In addition, we can use some state-of-the-art pre-trained models for text generation in word-level generation method.


## Dataset

The initial intent was to find ready-to-use dataset of the compliments, but since there is no such dataset we decided to find suplimentary ones. The dataset we were going to work with initially contained [love letters](https://www.kaggle.com/fillerink/love-letters). Using this dataset the model will learn to produce text with love, warm intent. But this is not the results we expect, to teach the model to make not only right intended text but compliments we expanded the dataset and added compliments collected manually from several sites([1](https://www.thetalka.com/love-letters-for-her/), [2](https://memesbams.com/love-letters-for-her/), [3](https://www.legit.ng/1218152-deep-love-letters-ll-cry.html)).


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

The model generated from the scratch gave us an understanding of the text generation process, we learned what techniques to what problems are applicable. But we understood the shortcut of this approach, we learn the model to generate meaningful text and force it to make this text look like a compliment at the same time and the dataset. The task could be split into 2 subtasks, according to the issue above. The generation of meaningful text is a very complicated task, it requires an enormous dataset to work with and a very deep neural network to produce sophisticated results. The training will require computing and time resources unavailable for us, that's why we decided to use a pre-trained model for text generation. 
The state-of-the-art text generation models are GPT-2 and GPT-3 developed by the OpenAI. Since the 3rd version model's API is closed now due to the regularisations, we used the gpt2-medium pre-trained model in our problem-solving task. This model will answer for the generation of the meaningful text, but we need to train it so the compliments will be generated at the end.
  
The training of the model to produce compliments is straight forward, prepared dataset is used to train and validate 2-3 times in epoch gpt2-medium model, and change it's weights accordingly. The AdamW optimizer was the best resulting optimizer.
  
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

The most important limitation we have encountered is not enough data for training model, therefore the main goal for the further work is to expand the dataset. 

Another essential issue is tracking the meaning of the entire sentence and predicting the next word based on all the previous words in the sentence. To do this, you can use an autoencoder architecture with an attention mechanism.
