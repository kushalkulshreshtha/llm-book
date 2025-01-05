<h1> Text classification </h1>

LLMs can be used for a variety of tasks such as classification, text summarization, ranking etc.

Text classification is a task wherein a sentence is classified into buckets like positive/negative (sentiment analysis). 

Both text representation and generation models can be used to do text classification.

<h2> Classification using text representation models </h2>

Text representation models are encoder-only models like BERT, RoBERTa etc. They generate embeddings of the text.
It can be done using a  model fine-tuned for classification or generic embedding model. Let's see both for sentiment analysis.

<h3> Using Fine-tuned Model </h3>

* These are the models which are fine-tuned to do a particular task (like sentiment analysis in this case)
* Model might be trained on a totally different dataset though (like tweets) whereas we might be doing sentiment analysis for movie reviews. Hence model selection is important.
* Model directly outputs the class

![Using fine-tuned model to do classification](https://github.com/user-attachments/assets/e752b37b-5eb8-4069-ba31-d973d0e32e60)

<img src="https://github.com/user-attachments/assets/e752b37b-5eb8-4069-ba31-d973d0e32e60" width="480" height="360">
<h3> Using Embedding Model </h3>

We can utilize this in two different ways: 
* Supervised classification: Using the embedding from the model and training a classifier (like logistic regression) on top of that.
* Zero shot classification: Using cosine similarity (see below)



<h4> Supervised Classification </h4>

* Need training data with sentences and sentiment labels.
* Use the model to generate embeddings.
* Use embeddings as feature and label as output to train a classifier.
* Use this classifier to predict.

![Using Supervised Classification- training Logistic Regression over embeddings](https://github.com/user-attachments/assets/c13fad2b-5320-420a-bf71-0f0806b109e2)


<h4> Zero-shot Classification </h4>

* Ideal when we do not have labelled data.
* Generate embeddings for the sentences (for eg. The movie was amazing). Let's call them e1
* Generate embeddings for the positive and negative classes (for eg. 'a positive review' and 'a negative review'). Lets call them e2 and e3 respectively.
* Calculate cosine similarity of sentences with classes and compare. cosine(e1.e2) / cosine(e1.e3). The class with a higher cosine similarity score is our target class.
* **TODO: Add code examples**
  
<h2> Classification using text generation models </h2>

* Text generation models are either decoder-only (like GPT family of models) or encoder-decoder based (T5 family)
* They need a prompt to do the task, and they will generate the output in the form of a text. We will need to parse the text to find the output.
* Prompt can be something like: "You are a helpful assistant. Predict whether the following sentence is a positive or negative movie review. Do not output anything else. [SENTENCE]
* Can be followed by the format you want output in: "Return 1 if positive and 0 if negative"
* The results of the model like ChatGPT may come as good but that should not be counted as the model's predictive power. A good accuracy score could be because the model had seen the data before.
