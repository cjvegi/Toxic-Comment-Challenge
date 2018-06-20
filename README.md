# Identifying Toxic Comments

## Objective:

To build a multi-headed model that’s capable of detecting different types of of toxicity like threats, obscenity, insults, and identity-based comments from Wikipedia’s talk page edits.

## Business Aspect:

To effectively facilitate conversations on online community, leading many communities to limit or completely shut down such user comments.

## Project Description:

With the recent growth of people on the internet, civil conversations are seeing a decline. "Whatever intelligent observations do lurk there are often drowned out by obscenities, ad-hominem attacks, and off-topic rants."  These things are forcing many online platforms which once flourished with intellectual discussions to close the comment sections. To facilitate meaningful conversations on their online platform The New York Times employed full-time moderators who moderate nearly 11,000 comments per day on the selected article(roughly 10% of Times articles). However, for small firms operating people for these tasks might be out of scope. To aid, the Conversation AI team, a research initiative founded by Jigsaw and Google (both a part of Alphabet) are working on tools to help improve online conversation. One area of focus is the study of negative online behaviors, like toxic comments (i.e. comments that are rude, disrespectful or otherwise likely to make someone leave a discussion). So far they’ve built a range of publicly available models served through the Perspective API, including toxicity. But the current models still make errors, and they don’t allow users to select which types of toxicity they’re interested in finding (e.g. some platforms may be fine with profanity, but not with other types of toxic content).

Resources:
1. [A Bot That Identifies 'Toxic' Comments Online](https://www.theatlantic.com/technology/archive/2017/02/a-bot-that-identifies-toxic-comments-online/517563/)
2. [A Brief History of the End of the Comments](https://www.wired.com/2015/10/brief-history-of-the-demise-of-the-comments-timeline/)
3. [The Times is Partnering with Jigsaw to Expand Comment Capabilities](https://www.nytco.com/the-times-is-partnering-with-jigsaw-to-expand-comment-capabilities/)

## Data

Source: [Kaggle: Toxic Comment Classification Challenge](https://www.kaggle.com/c/jigsaw-toxic-comment-classification-challenge). It consists of large number of Wikipedia comments which have been labeled by human raters for toxic behavior. 
The types of toxicity are:   
  a. toxic   
  b. severe_toxic  
  c. obscene  
  d. threat 
  e. insult    
  f. identity_hate     


## Pipeline

![Pipeline](img_gh/Toxic.png)
### 1. Label/Target Distribution:

                                                   Labels Distribution
                                                  
![Pipeline](img_gh/label_distribution.png)

As we notice from the above plot, data is imbalanced i.e., target/labels are heavily skewed. For example, we have 99.12% non identity hate comments and less than 1% identity comments. 

### Base Accuracy for each Label

Base accuracy on the train data is obtained by prediciting all labels as non-toxic comments. Thus, we have base accuracy for the each of the following label/targets

  1. Toxic: 90.42 %
  2. Severe_toxic: 99.00 %
  3. Obscene: 94.71 %
  4. Threat: 99.70 %
  5. Insult: 95.06 %
  6. Identity_hate: 99.12 %

Let's take a look at the that are frequent in a toxic label:

![WordCloud](img_gh/word_cloud.png)



### 2. Cleaning text

Steps involved in cleaning:   
  a. Removing all irrelevant characters such as any non alphanumeric characters   
  b. Convert all characters to lowercase   
  c. Tokenizing comments into individual words   
  d. lemmatization    
  e. Stemming    

### 3. Feature Generation

Generating features is important step to a good predictive model. They are many way to generate feature vector from text. However, they are few methods to assess the quality of the generated features. In this project, features generated are validated using t-SNE on TensorBoard.

Feature generation techniques
  a. Bag of Words
  b. Term frequency–inverse document frequency(tf-idf)
  c. Word2Vec
  d. sentiment polarity using TextBlob
  
### 4. Machine Learning:

Since we have mutlitple labels, we are dealing with Mulit-label classification model. We can approach this problem in different way, some of which used in this project are

### i. Transforming the problem to a single label classification problem

   a. Binary Relevance: In this labels are treated idependently i.e., for each label, a classifier is trained on the input data. Since, we have six labels, we would have six different classifers
  
   b. Classifier Chains: In this, the first classifier is trained just on the input data and one label and then each next classifier is trained on the input space and all the previous classifiers in the chain.
 
 Example:
  
  ![WordCloud](img_gh/chain_1.png)
  
  Lets say we have our data represented as,
  
  The problem would be transformed into 4 different single label problems as shown below.  
    Yellow colour portion is the input space 
    White coloured portion represent the target/label variable
    
  ![WordCloud](img_gh/Chain_2.png)

### ii. Different Machine learning models used:

   a. Random Forest    
   b. Logistic Regression     
   c. XGBoost      
   d. Artificial Neural Netrwork(ANN)     
    
The below plot shows increase in accuracy above base line training a logistic regression on input data, by adopting two methods mentioned above i.e., Binary Relevance and Classifier Chains.

  ![WordCloud](img_gh/binvschain.png)
  
Finally, to improve accuracy different ensembling approaches are adopted
  

### Accuracy: 

Using Ensembleing methods, Mean column wise(sum of all labels) area under receiver operating characteristic curve is 0.9835 on the Kaggle Leaderboard(Ongoing) is achieved. Current placed in top17% on Kaggle leaderboard.

Detailed Project report and Code
[GitHub](https://github.com/cjvegi/Toxic-Comment-Challenge)

## 5. Web Deployment

As a way to gather more data and test the model, trained machine learning model is deployed using Flask which is hosted at [chiranjeevivegi.pythonanywhere.com](http://chiranjeevivegi.pythonanywhere.com/). The users reviews and the label(toxic or non toxic) specified by users are stored in database which can be used to train the model on.

![WordCloud](img_gh/Webdev.png)


## 6. Future Work:
  a. Feature generation: Word Embedding(Word2Vec, GLobal Vectors for Word Representation(GloVe)
  b. Long short term memory(LSTM), Gated Recurrent Unit(GRU), Convolutional Neural Network(CNN)


### References:
1. https://www.analyticsvidhya.com/blog/2017/08/introduction-to-multi-label-classification/
2. https://blog.insightdatascience.com/how-to-solve-90-of-nlp-problems-a-step-by-step-guide-fda605278e4e
3. https://www.kaggle.com/c/jigsaw-toxic-comment-classification-challenge


<iframe src="https://docs.google.com/document/d/e/2PACX-1vQTl2IB3WDrCoFlNTmuDdgELBYyiV4TbKLIQKNaeCce0MgNGiR3W-LyGjrAxuimyu7ax4qgDJ4ytPCF/pub?embedded=true"></iframe>
